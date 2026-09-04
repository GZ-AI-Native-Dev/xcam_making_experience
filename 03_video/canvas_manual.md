---
name: spicycam-canvas-video-latest
description: >
  画布手工视频生成 skill：在画布平台用图片节点接视频节点，选 MiniMax 模式
  （I2V/FL2V/Ref2VA）和首尾帧工作流，手工生成并挑选直播角色视频。
  与 h3_pipeline 互补：小批量、试产、交接演示走画布手工；大规模量产走脚本管线。
---

# 画布手工视频生成 Skill

> 定位：直播角色视频可以在画布里手工操作（本 skill），也可以脚本批量调用（`03_video/h3_pipeline.md`）。
> 手工路径适合：新角色试产、单条补拍、交接演示、非技术同事操作。
> 画布平台入口（SOP 默认 `https://ic.xshow.live/image`，环境不同以实际部署为准）。
> 核心原则：**固定镜头、固定人物、固定首尾帧，动作只在中间发生。**

## 1. 从首帧进入视频生成

1. 角色画布里已有首帧图片节点（首帧生产见 `01_role/first_frame.md`）。
2. 鼠标悬浮图片区域 → 进入白框操作菜单：视频生成 / 编辑 / 下载。
3. 图片节点连接到下游视频节点；视频节点里选视频模型、首尾帧输入和参数。

## 2. MiniMax 模式选择

| 模式 | 输入 | 适合场景 | 说明 |
|---|---|---|---|
| **I2V / I2VA** | 1 张首帧图 | 首帧向后自然发展 | 自由度更高，但结尾不一定回到原姿态 |
| **FL2V / FL2VA** | 2 张首尾帧图 | idle、gift、action、typing loop | **直播角色优先使用**，可以强制结尾回到尾帧 |
| **Ref2VA** | 多参考素材 | 复杂动作和长视频 | 需要更多参考定义，适合后续专项 |

- FL2V 需要正好两张图：图1 = 首帧，图2 = 尾帧。
- 直播 loop 首尾使用同一张图（Picture 1 = Picture 2）。
- 带音频的状态（gift 口播、action 骚音景）走 **FL2VA**（A = audio，H3 原生音频链路）。

## 3. 工作流选择

- 本地部署包含 MiniMax H3 官方工作流 + 带 spicy LoRA 的扩展工作流。
- 官方模型、工作流和提示词结构参考：MiniMax H3 官方仓库 `https://github.com/MiniMax-AI/MiniMax-H3`；交接包另附 `minimax-h3-official-prompt-writing`（官方 FL2VA 提示词格式）。
- **idle / action / typing / preview 优先选择首尾帧（FL2V）工作流**——它们必须首尾一致。
- I2V 工作流只用于不需要回位的试验性素材。

## 4. 生成控制点

- 只写单镜头 `[Shot 1]`，不写分镜、转场、剪切。
- 固定镜头用正向锚定：`static shot`、`same lens focal length`、`subject size remains unchanged`、`background landmarks remain unchanged`（不写否定式 `no zoom`）。
- Idle 和普通 action 默认静默；gift 可以一句短中文感谢；骚版 action 用非人声音景（见 `02_prompt/action.md`）。
- 不新增参考图里没有的实体道具、字幕、直播 UI 或屏幕文字。
- 同一个提示词建议跑 **3 个候选**（批量吞吐场景可降 2，见主 `SKILL.md` 规则17），因为同一提示词也可能随机生成推近、拉远或动作异常。
- 提示词三段结构（`integrated_multimodal_description` / `overall_soundscape` / `non_diegetic_music`）和首尾帧对齐行，按各状态 skill 的模板写。

## 5. 两条操作路径

| 路径 | 谁用 | 做法 |
|---|---|---|
| **画布 Agent** | 非技术同事 | 画布右上角 Agent（Qwen 模型）生成动作/神态提示词，人工复核后贴进视频节点 |
| **项目提示词资料** | 技术同事 | 按交接包里的 `spicycam-prompt-latest` 各 skill 模板写 prompt，批量跑则走 `h3_pipeline` |

画布 Agent 产出的 prompt 必须过一遍 §4 控制点复核（Agent 容易写分镜和镜头运动词）。

## 6. 生成后处理

1. 候选视频下载后抽三帧（约 0.5s / 中 / 尾）：看人物大小、背景地标、是否回位、是否推近拉远。
2. 合格进 `accepted/`，不合格进 `rejected/videos/` + `rejected/prompts/` 并记坏例原因（目录约定见 `appendix/prompt_quality_notes.md`）。
3. 画布出现节点异常、模型不可用，走平台问题反馈入口记录。

## 检查清单

- [ ] 是否选了正确的模式（直播 loop 用 FL2V/FL2VA，不用 I2V）？
- [ ] FL2V 是否正好两张图，首尾是否同一张锚点图？
- [ ] 是否选了首尾帧工作流（不是 I2V 工作流）？
- [ ] prompt 是否单镜头、正向锚定固定镜头、无分镜？
- [ ] 是否跑了候选并做三帧 QC？
- [ ] 画布 Agent 的产出是否过了 §4 复核？
- [ ] 素材是否按目录约定归档（accepted / rejected）？
