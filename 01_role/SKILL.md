---
name: spicycam-role-latest
description: >
  阶段一「角色与锚点」总览：角色定义 → 首帧生产与验收 → 锚点建立 →
  模式选择 → 性格选择。产出物是后续所有视频的首尾帧锚点图。
---

# 阶段一：角色与锚点

> 上游：角色概念 ｜ 下游：→ `02_prompt/`（所有 prompt 依赖本阶段的锚点首帧）
> 对齐平台的「主播角色视觉生成」阶段。

## 本阶段文件导航

| 文件 | 职责 |
|---|---|
| `role_system.md` | 角色定义系统：身份、直播地点、习惯、职业痕迹、外观与场景多样性 |
| `first_frame.md` | 首帧生产 skill：三种制作方式（明星 LoRA / 角色画布 / Krea-edit）+ 模板 + 流程 |
| `first_frame_standard.md` | 首帧验收标准：母提示词细节、4 个完整示例、背景控制、易翻车词 |
| `anchor_pose.md` | 锚点姿态模板：public_live / private_live 两套，中英双语 |
| `mode_system.md` | 公屏/私屏模式系统：服装状态、灯光、互动尺度对照 |
| `personality_templates.md` | 15 种性格变量及注入位置 |

## 生产流程

1. **定义角色**：按 `role_system.md` 写身份 + 直播地点 + 直播习惯 + 职业痕迹（职业痕迹只用参考图里已可见的物体）。
2. **选模式**：按 `mode_system.md` 决定 public_live / private_live；两个模式的首帧、锚点、服装状态分开做，不混用。
3. **选性格**：从 `personality_templates.md` 选 1 种，后续注入 idle / gift / action。
4. **生产首帧**：按 `first_frame.md` 三种方式之一出图，9:16 多张生成人工挑选。
5. **验收首帧**：过 `first_frame_standard.md` 验收标准（直视镜头、平视、背景同清晰、明亮端正）。
6. **建锚点**：按 `anchor_pose.md` 模板固化该角色的锚点描述，这张图就是后续所有视频的 Picture 1 + Picture 2。

## 检查清单

- [ ] 角色是否有身份、地点、习惯、职业痕迹四件套？
- [ ] 是否明确了 public_live / private_live，公私首帧分开？
- [ ] 首帧是否按 `first_frame.md` 流程生产并过了 `first_frame_standard.md` 验收？
- [ ] 锚点是否按 `anchor_pose.md` 固化（手位、坐姿、背景地标明确）？
- [ ] 性格是否选定，变量可注入三个状态？
- [ ] 交付物是否齐：首帧图 + 首帧提示词 + 使用的 LoRA/edit 模型？
