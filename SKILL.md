---
name: spicycam-prompt-latest
description: >
  Master dispatcher for the latest spicycam livestream production system,
  organized in three stages aligned with the creation platform:
  01_role (character, first frame, anchor, mode, personality),
  02_prompt (Idle/Gift/Action/Typing/Preview H3 FL2VA prompts + dual-route
  Complex Action: Wan 3.0 chained OR MiniMax H3 I2VA/FL2VA/L2VA/Ref2VA),
  03_video (canvas manual generation, multi-card ComfyUI batch pipeline,
  QC and delivery).
---

# Spicycam Prompt System Latest

> 三阶段生产管线，对齐创作平台的「角色视觉 → 声音内容 → 镜头直播」：

## 目录导航

```
01_role/      阶段一：角色与锚点（角色定义→首帧生产与验收→锚点→模式→性格）
02_prompt/    阶段二：提示词写作（idle/gift/action/typing/preview + complex_action 双路线
              + rules/ 四条写作铁律 + references/ MiniMax 官方提示词指南）
03_video/     阶段三：出片与交付（画布手工小批量 / 脚本批量量产 / QC 与交接交付物）
appendix/     经验笔记（生产实测积累）
```

> `Idle`、`Gift`、`Action`、`Typing`、`Preview` 使用 MiniMax H3 FL2VA，同一张参考图作为 Picture 1 和 Picture 2，生成首尾一致 loop。
> 多段复杂动作（Complex Action）双路线：Wan 3.0 i2v 链式分段 或 MiniMax H3 全模式（I2VA/FL2VA/L2VA/Ref2VA），详见 `02_prompt/complex_action/SKILL.md`。
> 画布手工视频生成详见 `03_video/canvas_manual.md`；大规模量产走 `03_video/h3_pipeline.md`。

## 模式定义

| 模式 | 场景定位 | 服装状态 | 互动强度 | 使用状态 |
| --- | --- | --- | --- | --- |
| `public_live` | 1v多公屏直播间 | 骚版生产层：蕾丝胸罩/内裤/比基尼/透丝袜/性感运动装等，吸引男用户 | 公开直播，骚版动作 | Idle / Gift / Action / Typing / Preview |
| `private_live` | 1v1私密直播间 | 点击 private 直接脱衣切换（未来功能，未上线） | 更私密、沉浸 | 未上线，暂不出素材 |

## 状态范围

| 状态 | public_live（当前唯一活跃模式） | private_live | 时长 | 首尾要求 |
| --- | --- | --- | --- | --- |
| `Idle` | 骚版服装下的微动作 | 未上线 | 4-15 秒（默认 8 秒） | 首尾一致 |
| `Gift` | AR虚拟礼物，骚版服装反应 | 未上线 | 4-15 秒（默认 8 秒） | 特效消散后首尾一致 |
| `Action` | 骚版简单动作按钮 | 未上线 | 4-15 秒（默认 8 秒） | 动作完成后回到锚点 |
| `Typing` | 等待 AI 回复时的输入状态 | 未上线 | 4-15 秒（默认 5 或 8 秒） | 输入动作完成后回到锚点 |
| `Preview` | 首页/角色卡片吸引点击 | 未上线 | 4-15 秒（默认 8 秒） | 吸引动作完成后回到锚点 |
| `Complex Action` | — | 脱衣→性行为多段视频（从公屏骚版首帧出发，视频内脱衣） | 4 秒单段-80 秒四段 | 双路线：Wan 链式四段拼接（K1 同锚点首帧，K5 回休息态）或 H3 单段/逐段 |

## 前端消费逻辑

一个直播角色不是一条视频，而是**一组状态素材**，前端按用户行为切换：

| 用户行为 | 播放素材 | 提示词重点 | 验收重点 |
| --- | --- | --- | --- |
| 无人操作/等待互动 | idle | 低幅度微动作，不说话 | 首尾一致，镜头固定，有生命感 |
| 用户送礼物 | gift | 虚拟 AR 礼物 + 一句短感谢口播 | 反应真实，特效不挡脸，结尾消散 |
| 首页/角色卡片展示 | preview | 最能代表角色气质的轻动作 | 一眼看懂人设，画面干净 |
| 用户发消息等回复 | typing | 看屏幕、轻微打字或接过手机 | 像真实主播回复，镜头不变 |
| 点击动作按钮 | action | 一个单段动作，不加礼物 | 动作自然，结尾回到首帧 |
| 剧情/长互动 | complex action | 分段设计，多模型配合 | 先小样验证，避免长视频漂移 |

## 统一规则

1. **成人虚构角色**：prompt 内统一写成人虚构角色，不把真实姓名送进模型。
2. **H3 FL2VA**：每条视频都按首尾帧画布生成，Picture 1 对齐 0.00 秒，Picture 2 对齐目标时长结尾。
3. **同图首尾**：当前批量生产使用同一张参考图作为首帧和尾帧，动作必须回到参考图姿态。
4. **首帧先行**：生成视频前先按 `01_role/first_frame.md` 生产首帧/定妆照，并按 `01_role/first_frame_standard.md` 通过验收。
5. **时长弹性**：Idle / Gift / Action / Preview 时长 4-15 秒（默认 8 秒）；Typing 时长 4-15 秒（默认 5 或 8 秒）。敲键盘段只持续约 3-4 秒。
6. **首尾一致**：每条 prompt 必须写明首帧和尾帧回到同一参考图姿态。
7. **固定机位**：镜头锁定写成正向构图锚定：lens focal length、subject size、background landmarks remain unchanged。
8. **三段结构**：最终英文 prompt 使用 `integrated_multimodal_description`、`overall_soundscape`、`non_diegetic_music`。
9. **真实直播感**：真实高清手机直播画面、自然曝光、深景深、人物和背景同样清晰，不做电影感、影楼感、人像虚化。
10. **人格驱动**：先定义 AI 女友直播角色的人物底色，再按性格写 Idle / Gift / Action / Typing / Preview。
11. **Gift 虚拟化**：只用直播平台 AR overlay、粒子、贴纸或轻量 3D 礼物效果；不使用男性手部或实物交接。
12. **Gift 可口播**：Gift 允许一句很短的中文感谢台词，例如“谢谢爸爸。”“谢谢老公，爱你哟。”，必须使用 H3 原生音频链路。
13. **Action 简单化**：Action 是一个 8 秒简单单段动作，必须在结尾回到当前模式锚点。
14. **Typing 回复态**：Typing 是“主播正在处理输入/回复消息”的等待状态，优先使用画面外键盘声或低位手机交接，不写对白。
15. **Preview 预览态**：Preview 是首页/角色卡片吸引点击的视频，按人物底色做差异化吸引动作。真实明星、网红、素人或任何可识别真人只能做非露骨 Preview；成人向强吸引 Preview 只用于原创虚构成年人或授权成人素材。
16. **Preview 首帧编辑**：Preview 先做更强的角色卡 anchor。明星/演员类优先用本地部署 Krea 图像编辑，但仅限非露骨角色预览；网红、素人或普通人可用百炼 Qwen 3.0 Pro 或同等级图像编辑模型；输入图建议 1080P 以上，输出仍要满足真实高清直播首帧标准。原创虚构成人角色可使用本地 MiniMax 高清帧流程或 Wan 3.0 生成更成人向的首页吸引预览。
17. **候选生产**：Idle / Gift / Action / Preview 每个动作同一个 prompt 跑三次，分别进入 `candidate`、`candidate2`、`candidate3`。批量生产可按吞吐降为每 prompt 2 候选（验收靠候选兜底，见 `02_prompt/action.md` 推近拉远专项）。Typing 当前每个角色跑 2 条：01 为 5 秒短版，02 为 8 秒长版，供人工挑选。

## 首帧标准

所有角色进入视频生产前，必须先生成并验收首帧图：

- 竖版 9:16，人物居中，从头到膝盖完整可见。
- 身体正对镜头，脸完全正对镜头，双眼直视镜头。
- 镜头高度与眼睛平齐，平视，不俯拍、不仰拍、不侧拍。
- 人物与背景都清晰，前景、中景、远景全部清楚；不使用浅景深、人像模式或背景虚化。
- 场景真实、整洁、简约，3-5 个关键背景物即可；墙面、桌子、家具和主要物体与镜头平行。
- 画面明亮均匀，曝光正常偏亮但不过曝，像普通高清直播摄像头。

详细母提示词和示例见 `01_role/first_frame_standard.md`。
Prompt 质量经验和稳定写法见 `appendix/prompt_quality_notes.md`。

## Prompt 结尾

```text
She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2.

overall_soundscape: N/A

non_diegetic_music: N/A
```

Gift 有口播时，`overall_soundscape` 使用：

```text
overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.
```

## 目录约定

每个角色统一使用：

```text
generated/
  idle/{candidate,candidate2,candidate3,accepted,rejected,qc,notes,raw_runs}
  action/{candidate,candidate2,candidate3,accepted,rejected,qc,notes,raw_runs}
  gift/{candidate,candidate2,candidate3,accepted,rejected,qc,notes,raw_runs}
  typing/{candidate,candidate2,candidate3,accepted,rejected,qc,notes,raw_runs,prompts}
  preview/{candidate,candidate2,candidate3,accepted,rejected,qc,notes,raw_runs}
  complex action/{代号}/keyframes,segments/seg1-4/candidate*3+accepted,concat_output,rejected,qc,notes,raw_runs
```

坏视频如果对提示词迭代有价值，放 `rejected/videos`，对应 prompt 放 `rejected/prompts`。

## 角色交接交付物

每个角色交接必须交付五件：首帧三件套（图+提示词+LoRA/模型）、各状态中英文提示词、每动作 1-3 候选视频、人工筛选记录（合格+坏例原因）、上线资料（角色名/年龄/职业/描述/选中视频地址）。

完整表格与检查方法见 `03_video/qc_delivery.md`；上线资料字段可参照角色预览站（`https://xcamshow.xyz/interactive`）的角色卡片。

## 生成检查

- [ ] 当前是否只生产 `public_live` 素材（骚版服装）？
- [ ] 是否已按 `01_role/first_frame.md` 生产、并按 `01_role/first_frame_standard.md` 验收首帧？
- [ ] 是否只生成 `Idle`、`Gift`、`Action`、`Typing`、`Preview`（H3 FL2VA）或 `Complex Action`（双路线）？
- [ ] 如果是 Complex Action，是否按 `02_prompt/complex_action/SKILL.md` 选对路线（四段长视频走 Wan 链式；单段/多图参考走 H3），四段骨架脱衣→就位→高潮→收尾，K1 用已有锚点首帧？
- [ ] 如果走批量生产，是否按 `03_video/h3_pipeline.md` 执行（先 dry-run、选卡、断点续跑、收割器 salvage、autopilot+pullback 回收）？
- [ ] 如果是画布手工生成，是否按 `03_video/canvas_manual.md` 选对模式（直播 loop 用 FL2V/FL2VA）和首尾帧工作流？
- [ ] 角色交付物是否齐：首帧三件套、各状态提示词、候选+筛选记录、上线资料？
- [ ] Idle / Gift / Action / Preview 是否 4-15 秒（默认 8 秒），Typing 是否 4-15 秒（默认 5 或 8 秒）？
- [ ] 首帧和尾帧是否回到同一模式锚点？
- [ ] Gift 特效是否完全消散？
- [ ] Gift 如果有口播，是否只有一句短中文？
- [ ] Action 是否只是一个简单单段动作？
- [ ] Typing 是否没有对白、字幕和 UI，敲键盘段是否约 3-4 秒，并且结尾回到首帧姿态？
- [ ] Preview 是否按人物性格差异化，暧昧但不露骨，并且没有字幕/UI/新道具？
- [ ] 是否避免真实人物冒充、未成年人、非自愿、暴力或违法内容？
