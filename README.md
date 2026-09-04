# Spicycam 主播角色制作经验 · 官方知识库

> **AI 直播女友角色全链路生产 Skill Pack**
> 从一个角色概念，到上线交付的完整视频素材组——每个环节都有对应的 skill 文档。

**当前版本：`v1.0-20260904`** ｜ 维护：[@1aziness6](https://github.com/1aziness6)

---

## ✦ 这套东西解决什么问题

一个 AI 直播角色不是一条视频，而是**一组状态素材**：待机、送礼、动作、打字、预览、长互动。
前端按用户行为切换播放。这套知识库把整条生产链沉淀成可执行的 skill：

| 用户行为 | 播放素材 | 对应 Skill |
| --- | --- | --- |
| 无人操作 / 等待互动 | idle | `02_prompt/idle.md` |
| 用户送礼物 | gift | `02_prompt/gift.md` |
| 点击动作按钮 | action | `02_prompt/action.md` |
| 用户发消息等回复 | typing | `02_prompt/typing.md` |
| 首页 / 角色卡片 | preview | `02_prompt/preview.md` |
| 剧情 / 长互动 | complex action | `02_prompt/complex_action/` |

## ✦ 三阶段生产管线

目录结构与创作平台的三个阶段一一对应，接手者按顺序读即可：

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  01_role/   │ ──► │  02_prompt/ │ ──► │  03_video/  │
│  角色与锚点  │     │  提示词写作  │     │  出片与交付  │
│ 角色视觉生成 │     │ 声音内容编排 │     │ 镜头直播创作 │
└─────────────┘     └─────────────┘     └─────────────┘
```

**阶段一 `01_role/`**：角色定义 → 首帧生产（明星 LoRA / 角色画布 / Krea-edit 三方式）→ 首帧验收 → 锚点建立 → 公屏/私屏模式 → 15 种性格变量。

**阶段二 `02_prompt/`**：MiniMax H3 FL2VA 首尾帧视频的全部状态提示词，附四条写作铁律（镜头锁定 / 防穿透 / 时长幅度 / 直播在场感）与 MiniMax 官方提示词指南。

**阶段三 `03_video/`**：两条出片路径——画布手工（小批量试产）与多卡 ComfyUI 脚本管线（大规模量产），以及三帧 QC 法和角色交接交付物清单。

## ✦ 目录结构

```
├── SKILL.md                          总纲：导航 + 统一规则 + 前端消费逻辑
├── 01_role/                          阶段一：角色与锚点
│   ├── SKILL.md                        生产总览
│   ├── role_system.md                  角色定义系统
│   ├── first_frame.md                  首帧生产（三种方式）
│   ├── first_frame_standard.md         首帧验收标准（含 4 个完整示例）
│   ├── anchor_pose.md                  锚点姿态模板（公/私）
│   ├── mode_system.md                  公屏 / 私屏模式
│   └── personality_templates.md        15 种性格变量
├── 02_prompt/                        阶段二：提示词写作
│   ├── SKILL.md                        写作总纲 + 官方参考导航
│   ├── idle.md / gift.md / action.md / typing.md / preview.md
│   ├── complex_action/                 复杂动作（双路线）
│   │   ├── SKILL.md                      Wan 3.0 链式 + MiniMax H3 全模式
│   │   └── six_poses.md                  六大位姿分镜
│   ├── rules/                          四条写作铁律
│   └── references/                     MiniMax 官方提示词指南（已入包）
│       ├── h3_official_base_modes.md     T2VA/I2VA/FL2VA/L2VA
│       └── h3_official_ref_mode.md       Ref2VA 多图参考六段结构
├── 03_video/                         阶段三：出片与交付
│   ├── SKILL.md                        两条路径怎么选
│   ├── canvas_manual.md                画布手工生成
│   ├── h3_pipeline.md                  多卡批量管线（环境无关）
│   └── qc_delivery.md                  三帧 QC + 交接交付物清单
├── appendix/                         生产实测经验笔记
└── docs/                             附件文档（SOP 等）
```

## ✦ 接手者阅读路线

1. 读本 `README` + 顶层 `SKILL.md`（10 分钟建立全局认知）
2. `01_role/SKILL.md` → 出第一张首帧并过验收
3. `02_prompt/SKILL.md` → 写第一条 idle，跑 3 候选，三帧 QC
4. `03_video/SKILL.md` → 选路径出片（手工 or 批量）
5. 按 `03_video/qc_delivery.md` 组齐角色交接交付物

## ✦ 核心生成引擎

| 引擎 | 用途 | 模式 |
| --- | --- | --- |
| **MiniMax H3 FL2VA** | 8 秒首尾一致直播 loop（idle/gift/action/typing/preview） | T2VA / I2VA / FL2VA / L2VA / Ref2VA |
| **Wan 3.0 i2v 链式** | 60-80 秒复杂动作四段拼接 | 首帧图+尾帧图+自然语言 |
| **MiniMax H3（复杂动作）** | 单段复杂动作 / 多图参考强一致 | I2VA / FL2VA / L2VA / Ref2VA |

## ✦ 附件文档

- [`docs/AI直播角色生产SOP_首帧画布与MiniMax视频生成指南_20260902.pdf`](docs/) —— 带操作截图的平台级 SOP（首帧制作 / 画布视频生成 / 视频分类 / 交接清单）。

## ✦ 参考资料

- MiniMax H3 官方仓库：<https://github.com/MiniMax-AI/MiniMax-H3>
- 画布平台 / 生图工作台：`ic.xshow.live`
- 角色预览站（上线字段参考）：`https://xcamshow.xyz/interactive`

## ✦ 版本记录

| 版本 | 日期 | 内容 |
| --- | --- | --- |
| **v1.0** | 2026-09-04 | 三阶段架构首发：01_role / 02_prompt / 03_video + appendix；complex_action 双路线（Wan 3.0 链式 + MiniMax H3 全模式）；MiniMax 官方提示词指南入包；附平台操作 SOP |

---

> 本知识库为生产经验沉淀，随产线迭代持续更新。
