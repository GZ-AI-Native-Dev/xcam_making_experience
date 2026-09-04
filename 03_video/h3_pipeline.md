---
name: spicycam-h3-pipeline
description: >
  多卡 ComfyUI MiniMax H3 FL2VA 批量生产管线：单条 runner、批量 driver、
  收割器、autopilot 转码、本地增量回收。含登录鉴权、断点续跑、salvage、
  和所有踩过的坑（PowerShell 引号、UTF-16、ssh 挂起、set -u 等）。
  环境无关：服务器、ComfyUI 账号、域名、路径均由接手者按自己环境配置。
---

# H3 批量管线 Skill

> 把角色首帧 + prompt md 变成本地 mp4 的全链路。已在「15 角色 × 17 prompt × 2 候选 = 510 条」规模验证。
> **本文档不绑定任何机器**：下表配置项由使用者按自己的环境填写。

## 0. 环境配置（接手者必填）

| 配置项 | 说明 | 示例格式 |
|---|---|---|
| `<SERVER>` | GPU 服务器 ssh 别名/地址 | `gpu-box` |
| `<USER>` | 服务器登录用户（**不一定是 root，用实际用户**） | `alice` |
| `<H3_ROOT>` | 服务器侧管线根目录 | `/home/alice/h3_batch` |
| `<COMFY_USER>` / `<COMFY_PASS>` | **自己的 ComfyUI 登录账号**（runner 内硬编码或改环境变量） | — |
| `<BASE_DOMAIN>` 或端口映射 | 卡0 的 base URL；卡N 的映射规则（子域名或端口） | `https://comfy.example.com` / `http://localhost:818N` |
| `<CARDS>` | 可用卡数与编号 | `0..7` |
| `<LOCAL>` | 本地工作区根目录（脚本源、首帧目录） | 自定义 |
| `<LOCAL_RESULTS>` | 本地成品回收目录 | 自定义 |

runner 脚本里有一张「卡号 → base URL」映射表和硬编码登录，**接手第一件事是改成自己的账号和域名**。

## 1. 组件清单

| 脚本 | 位置 | 职责 |
|---|---|---|
| `minimax_h3_fl2va_audio_allcards.sh` | 本地 + 服务器 | 单条 runner：login→传首帧→提交 workflow→轮询 history→下载 webm→写 downloaded_files.tsv。参数 `--card N --first png --prompt txt --slug s --out-dir d` |
| `h3_batch_driver.py` | 服务器 | 扫 `<H3_ROOT>/first_frames/{角色}/{角色}.png + action_prompts.md + gift_prompts.md`，解析 ```text 块拆 txt，建任务表，每卡一线程派 runner，镜像到 `out/results/{角色}/{cat}_{key}/candN.webm`。参数 `--first-dir --runner --out --candidates N --cards 4,5 --only 角色 --cat action\|gift --dry-run` |
| `launch_batch.sh` | 服务器 | `dry` 看任务表 / `run` nohup 起 driver |
| `collect_v2.sh` | 服务器 | 收割器：对已提交未下载的 job 按 submit.json 的 prompt_id 轮询 history，完成即下载+镜像。**每次查 history 前先 login**；返回非 JSON 视为未登录，重新 login 重试 |
| `autopilot.sh` | 服务器 | 每 5 分钟把 `out/results` 新 webm 转 mp4 到 `out/mp4_results`（libx264 crf18 fast + aac + faststart） |
| `make_pull_tar.sh` | 服务器 | 增量打包 marker 之后新 mp4 到 /tmp/pull.tar，输出 HASNEW/NONEW |
| `pullback_loop.ps1` | 本地 Windows | 隐藏窗口每 10 分钟 ssh 触发打包→scp tar→本地 tar 解到 `<LOCAL_RESULTS>` |
| `sync_and_prepare.ps1` | 本地 Windows | 同步 runner/driver/首帧到 `<H3_ROOT>`（路径按实际用户 home 写，别假设 /root） |

## 2. 单条 runner 产物约定

```
<H3_ROOT>/out/runs/{角色}/{cat}_{key}/candN/
  comfy_cookie.txt          # 本 job 独立 cookie jar
  status.tsv                # base=<该卡域名> ; submitted <run_id> <prompt_id>
  *_request.json            # 提交的 workflow
  *_submit.json             # {prompt_id, number, node_errors}
  history/<prompt_id>.json  # 完成态 history
  videos/<file>.webm        # 下载的视频
  downloaded_files.tsv      # 下载清单（=完成标记，driver 断点依据）
```

driver 断点规则：`videos/*` 非空 **且** `downloaded_files.tsv` 非空 → 跳过。

## 3. 鉴权模型（最坑）

- ComfyUI 所有接口（/prompt /history /queue /view）都要 session，**用使用者自己的 ComfyUI 账号登录**。
- 登录必须用 **cookie jar**：`curl -c jar -b jar GET /auth/login?next=...` 然后 `POST /auth/login` 带 username/password/remember/next。
- **未登录或 cookie 过期时，接口返回 200 + 登录页 HTML**（不是 401！）。`curl -f` 不报错，`jq` 解析失败。
- 因此任何轮询逻辑必须：**响应非 JSON → 重新 login → 重试一次 → 仍非 JSON 才算失败**。
- cookie jar 文件为空不代表没登录成功（session 可能走 IP/连接），但跨进程复用必须带 jar。

## 4. 标准操作流程

### 新角色上线
1. 本地角色目录放 `{角色}.png + action_prompts.md + gift_prompts.md`。
2. `scp -r <LOCAL首帧目录>/. <SERVER>:<H3_ROOT>/first_frames/`。
3. 服务器 `python3 h3_batch_driver.py ... --dry-run` 看任务数。
4. `--cards` 选卡（默认全卡；要留卡就 `--cards 4,5`）。
5. nohup 起 driver；runner 自己下载，driver 自己镜像 results。

### 只跑部分
`--only 角色名` / `--cat action|gift`。

### salvage（runner 死了但任务已提交）
1. 不要重跑 driver（会重复提交）。
2. 起 `collect_v2.sh`：按 submit.json 的 prompt_id 收割 history，完成即下载+镜像，error 标记跳过。
3. 收割器退出条件：pending=0 或轮数上限（720）。

### QC 与交付
1. `ffprobe` 验收 webm：VP9 768×1344 yuv420p + audio。
2. autopilot 转 mp4；pullback_loop 增量回 `<LOCAL_RESULTS>`。
3. 抽 0.5s/4s/7.5s 三帧看人物大小和背景是否一致（推近拉远 QC，见 action skill）。

## 5. 踩坑清单（血泪）

| 坑 | 现象 | 解 |
|---|---|---|
| `set -u` + `local a="$1" b="${a}.x"` | 同行自引用 → unbound variable → runner 提交后秒退，driver 全报 FAIL 但队列里有任务 | 声明与赋值分两行 |
| PowerShell 传 ssh 内联 bash | `$var`、`$1`、`$c:` 被 PowerShell 展开/报错 | **复杂 bash 一律写文件 → scp → `ssh <SERVER> "bash /tmp/x.sh"`** |
| PowerShell `Set-Content` | 默认 UTF-16 → bash syntax error | 用 UTF-8 编辑器/写文件工具，或 `-Encoding utf8` |
| ssh 里 nohup 后台 | ssh 会话挂住直到超时 | `nohup cmd >/dev/null 2>&1 </dev/null &`；接受超时，另开 ssh 验证 |
| python 重定向 stdout | log 0 字节（缓冲） | `python3 -u` 或看 GPU 利用率/队列判断 |
| `/history` 返回 HTML | 误判 CURL_FAIL 全军覆没 | 见 §3 鉴权模型 |
| `$HOME` 坏 | 某些 ssh 客户端环境下 `~` 展开到本地路径 | 全绝对路径，不用 `~` |
| 假设 root | home 目录/权限不对 | 用实际登录用户的 home |
| 队列接口未登录 | running/pending 全 0 但 GPU 100% | 先 login 再查；或直接看 `nvidia-smi` |
| 重跑 driver 重复提交 | 队列里出现双胞胎任务 | 先确认 resume 标记；salvage 用收割器不用 driver |

## 6. 性能基线（8 步 turbo，768×1344）

- 单条约 2-3 分钟/卡。
- 8 卡跑 136 条 ≈ 40 分钟。
- 2 卡跑 374 条 ≈ 6-8 小时（隔夜跑）。
- autopilot 转码百条级 8 并行 ≈ 1-2 分钟。

## 7. 检查清单

- [ ] 配置项（§0）都改成自己环境了？ComfyUI 账号是自己的？
- [ ] 首帧+md 已同步到 `<H3_ROOT>/first_frames/{角色}/`？
- [ ] dry-run 任务数 = 角色数 × prompt 数 × candidates？
- [ ] `--cards` 选对了（留卡需求）？
- [ ] runner 是修过 set -u 的版本？
- [ ] 收割器在跑时，driver 没有同时重跑同一批？
- [ ] results 镜像 + autopilot + pullback_loop 三件套都在？
- [ ] `<LOCAL_RESULTS>` 按 `{角色}\{cat}_{key}\candN.mp4` 组织？
