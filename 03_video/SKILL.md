---
name: spicycam-video-latest
description: >
  阶段三「出片与交付」总览：两条生成路径（画布手工小批量 / 脚本批量量产）
  的选择，以及 QC 与角色交接交付物。
---

# 阶段三：出片与交付

> 上游：← `01_role/` 首帧 + `02_prompt/` 提示词 ｜ 下游：→ 上线交付
> 对齐平台的「镜头与直播持续创作」阶段。

## 两条路径怎么选

| 场景 | 走哪条 | 文档 |
|---|---|---|
| 新角色试产、单条补拍、交接演示、非技术同事操作 | **画布手工** | `canvas_manual.md` |
| 几十上百条批量生产（多卡并行、断点续跑、无人值守） | **脚本管线** | `h3_pipeline.md` |

两条路径共用同一套 prompt 和首帧；产物都是 webm→mp4，都进同一套目录约定（见 `appendix/prompt_quality_notes.md` Output Convention）。

## 画布手工要点（详见 canvas_manual.md）

- 直播 loop 一律 **FL2V / FL2VA**（首尾两图），不用 I2V。
- idle / action / typing / preview 必选首尾帧工作流。
- 画布 Agent（Qwen）产出的 prompt 必须过 `02_prompt/SKILL.md` 的控制点复核。

## 脚本管线要点（详见 h3_pipeline.md）

- 环境无关：服务器、ComfyUI 账号、域名、路径由接手者按 §0 配置表填写。
- 流程：同步 → dry-run → 选卡 → driver 派单 → 收割器补收 → autopilot 转码 → pullback 回收。
- 死任务用收割器（`collect_v2.sh`）补，**不要重跑 driver**（会重复提交）。

## 出片之后

无论哪条路径，产出都过 `qc_delivery.md`：三帧 QC → 人工筛选 → 交接交付物清单。

## 检查清单

- [ ] 是否选对了路径（小批量手工 / 大批量脚本）？
- [ ] 模式是否选对（直播 loop 用 FL2V/FL2VA）？
- [ ] 是否跑了候选？
- [ ] 出片后是否过了 `qc_delivery.md` 的三帧 QC？
- [ ] 交接交付物是否齐？
