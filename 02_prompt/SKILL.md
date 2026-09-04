---
name: spicycam-prompt-writing-latest
description: >
  阶段二「提示词写作」总览：六个状态的子技能（idle/gift/action/typing/
  preview/complex_action）+ 四条共享写作铁律（rules/）。MiniMax H3 FL2VA
  三段结构、单镜头、固定机位、首尾一致。
---

# 阶段二：提示词写作

> 上游：← `01_role/` 锚点首帧 ｜ 下游：→ `03_video/` 生成
> 对齐平台的「声音与内容无限编排」阶段。

## 子技能导航

| 状态 | 文件 | 时长 | 音频 | 要点 |
|---|---|---|---|---|
| idle | `idle.md` | 8s | 静默 | 低幅度微动作 |
| gift | `gift.md` | 8s | 可一句短中文口播 | 虚拟 AR 礼物，尾帧前消散 |
| action | `action.md` | 8s | 非人声骚音景（骚版） | 单段动作，锚点起手回位 |
| typing | `typing.md` | 5s / 8s | 画面外键盘声 | 等待回复态，无对白 |
| preview | `preview.md` | 8s | 静默 | 首页吸引，暧昧不露骨 |
| complex_action | `complex_action/SKILL.md` | 4s-80s | 分段/原生音景 | 双路线：Wan 3.0 四段链式 或 MiniMax H3 全模式 |

## 共享写作铁律（写任何状态前先读）

| 文件 | 管什么 |
|---|---|
| `rules/camera_lock.md` | 镜头锁定：零机位移动，正向锚定写法 |
| `rules/anti_clipping.md` | 防穿透：接触即停止、肢体不交叉、物品实体感 |
| `rules/dynamic_timing.md` | 时长规则 + 各状态动作幅度边界 |
| `rules/live_presence.md` | 直播在场感：各状态的可用动作与硬禁项 |

## 通用 H3 FL2VA 骨架

所有单镜头状态（idle/gift/action/typing/preview）都用这个结构：

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the [8.00/5.00]-second mark of the target video.

integrated_multimodal_description: [Shot 1] ...

overall_soundscape: ...

non_diegetic_music: N/A
```

写作要点（全状态通用）：

1. 第一行必须写 Picture 1 / Picture 2 对齐说明。
2. 只写一个 `[Shot 1]`，不分镜、不按秒拆阶段、不写转场。
3. 镜头锁定用正向锚定（lens focal length / subject size / background landmarks remain unchanged），不写 `no zoom` 这类否定式。
4. 人物描述逐字对应首帧图（发色、服装、坐姿、手位、背景地标）。
5. 结尾必须回到 Picture 2 的同一姿态、手位、视线、表情、人物大小和构图。
6. 不新增参考图里没有的实体道具、字幕、UI、屏幕文字。
7. 高风险词降级：`near her lips`→`to her mouth`、`upper chest`→`collarbone`、`blow kiss`→`sends the kiss`、`lean closer`→删除、`shoulder height`→`chest height`。完整表见 `action.md` 和 `appendix/prompt_quality_notes.md`。

## 官方 H3 提示词参考（MiniMax 官方，交接包自带）

- `references/h3_official_base_modes.md`：T2VA / I2VA / FL2VA / L2VA 完整写法——对齐行格式、镜头运动词汇表（motion type + amplitude + speed）、说话人 `(S1)`、对白 `<d>`、音景两段规则、4 个完整案例。
- `references/h3_official_ref_mode.md`：Ref2VA 多图参考六段结构——`subject_definitions` / `summary` / `retention_analysis` / `detailed_description` / 两段音景，参考标签规则（`<Subject N>` / `<Picture N>` / `<Video N>` / `<Audio N>`）和完整案例。
- 日常单镜头状态用本文件的通用骨架即可；写 complex_action 路线 B 或需要镜头/对白细节时查官方参考。

## 候选规则

- 默认每 prompt 跑 **3 候选**（H3 有采样概率波动）。
- 批量吞吐场景可降 **2 候选**，靠候选兜底验收（三帧 QC，见 `03_video/qc_delivery.md`）。
- 两候选都坏 → 重跑一次；仍坏 → rejected 并保留 prompt。

## 检查清单

- [ ] 是否先读了四条共享铁律？
- [ ] 是否选对了子技能（状态对口）？
- [ ] 三段结构 + 首尾对齐行是否完整？
- [ ] 是否单镜头、正向锚定固定镜头？
- [ ] 人物描述是否逐字对应首帧？
- [ ] 结尾是否回 Picture 2？
- [ ] 高风险词是否降级？
- [ ] 候选数是否按规则设定？
