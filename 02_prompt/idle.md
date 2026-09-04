---
name: spicycam-idle-latest
description: >
  Generate or audit MiniMax H3 FL2VA Idle prompts for AI-girlfriend livestream
  characters. All Idle clips are silent 8-second seamless loops with identical
  first and last frames.
---

# Idle Skill

> Idle 是直播间等待状态。当前版本用于 MiniMax H3 FL2VA 首尾帧视频。
> Picture 1 和 Picture 2 使用同一张参考图，人物只做低幅度生活微动。

## 核心规则

1. 每条 Idle 固定 8 秒。
2. 只写低幅度微动作，不能写成 Action、Gift、剧情或分镜。
3. Idle 必须静默：不说话、不张口念台词、不写声音、不加音乐。
4. 不新增参考图里没有的物体、道具、UI、字幕或直播间界面。
5. 已有参考图时，prompt 重点写动作、表情、人格和稳定约束；不要长篇复述模型能从图里识别的衣服和背景。
6. 结尾回到 Picture 2 的同一姿态、手位、视线、表情和构图。

## 官方 H3 FL2VA 格式

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static idle shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. She only makes tiny idle micro-movements that match her personality: [one small idle action]. The movement amplitude stays very small, her posture remains almost unchanged, and her gaze returns to the camera. She settles back into the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2.

overall_soundscape: N/A

non_diegetic_music: N/A
```

## 人设驱动

先定义 AI 女友直播角色的人物底色，再选择微动作。

- 温柔型：轻呼吸、慢微笑、柔和眼神。
- 明艳型：眼神更亮、微笑更有存在感，但动作克制。
- 甜妹型：眨眼、轻微歪头、小幅害羞笑。
- 清冷型：表情少，眼神短暂变软。
- 健身/职业型：坐姿稳定、轻微点头、干净自然。
- 文艺型：眼神停顿、轻触发尾、安静微笑。

## 稳定动作库

- natural blink and soft smile
- tiny nod then return
- eyes glance slightly beside the lens, then return
- fingers relax slightly at the original hand position
- hands loosen and overlap again
- subtle breathing and posture settle
- very small head tilt and return
- low fingertip wave that stays near the original hand area

## 高风险动作与不稳定写法

### 动作层面

- 大幅撩头发、摸脸、靠近嘴唇、吹吻。
- 离开画面再回来。
- 前倾读弹幕。
- 看手机、看屏幕、读 comments。
- 小工具、手部细节、实体交接。
- "全身"和"头到膝盖"混写。

### 写法层面（反例沉淀）

- **动作链太长**：递入、接过、使用、展示、归还、退出 → 模型容易切镜。
- **小物件太具体**：指甲钳、手机、口红、耳机等，诱导手部近景。
- **堆否定词**：反复写 `no close-up`、`no zoom`，反而把注意力拉到近景概念。改正向锚定：`full-body subject size remains unchanged`、`background landmarks remain fixed`。
- **污染词**：`close-up`、`hand close-up`、`cutaway`，即使前面有 `no`，也可能污染注意力。
- **UI 诱导词**：`comments`、`screen`、`livestream composition` 容易让 H3 生成整屏直播 UI 或弹幕。
- **手部细节词**：`sleeve`、`wrist`、详细手指动作，容易把构图拉向手部或半身局部。

如果生成出现推近拉远，优先把动作降级为眼神、微笑、点头、原手位手指微动，而不是堆否定词。

## 复用说明

- **替换背景锚点词**：把示例中的 `flower shelf, wooden cabinet, wall, floor, and warm room-light positions` 换成对应角色首帧里实际可见的 2-4 个稳定背景地标。
- **替换性格描述**：把 `a poised, bright, teasing AI-girlfriend livestream personality; elegant, self-aware, and camera-fluent` 换成角色对应性格底色，但保持单条只描述一个动作。
- **首尾同图**：Picture 1 和 Picture 2 必须用同一张参考图，确保首尾帧无缝循环。
- **H3 模式**：idle 用 FL2VA 无音频（`overall_soundscape: N/A`）。

## 完整示例

以下 4 条均为批量生产验证通过的完整 Idle prompt（15 角色复用模板）。直接照结构替换背景地标和性格描述即可。

### 示例 1 · Soft Blink And Smile（眨眼微笑 · 纯表情）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static idle shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1, sitting front-facing in the same flower-room pose, with both hands resting naturally near the original side or lap-level position. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, the same flower shelf, wooden cabinet, wall, floor, and warm room-light positions, lighting, outfit, hairstyle, and background layout. She only makes tiny idle micro-movements that match a poised, bright, teasing AI-girlfriend livestream personality; elegant, self-aware, and camera-fluent: she gives one or two natural blinks, her eyes soften, and a small personality-matched smile appears before returning to the original calm expression. The movement amplitude stays very small, the action stays close to the original pose, her posture remains almost unchanged, and her gaze returns to the camera. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2. No speaking, no dialogue, no subtitles, no on-screen text, no new objects, no shot cuts, no transitions, no camera movement.

overall_soundscape: N/A

non_diegetic_music: N/A
```

### 示例 2 · Small Head Tilt（轻微歪头 · 头部微动）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static idle shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1, sitting front-facing in the same flower-room pose, with both hands resting naturally near the original side or lap-level position. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, the same flower shelf, wooden cabinet, wall, floor, and warm room-light positions, lighting, outfit, hairstyle, and background layout. She only makes tiny idle micro-movements that match a poised, bright, teasing AI-girlfriend livestream personality; elegant, self-aware, and camera-fluent: her head tilts only a little, her eyes stay soft, and she returns her face squarely to the camera. The movement amplitude stays very small, the action stays close to the original pose, her posture remains almost unchanged, and her gaze returns to the camera. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2. No speaking, no dialogue, no subtitles, no on-screen text, no new objects, no shot cuts, no transitions, no camera movement.

overall_soundscape: N/A

non_diegetic_music: N/A
```

### 示例 3 · Hair-End Touch（轻碰发尾 · 手部微动但不出原位）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static idle shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1, sitting front-facing in the same flower-room pose, with both hands resting naturally near the original side or lap-level position. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, the same flower shelf, wooden cabinet, wall, floor, and warm room-light positions, lighting, outfit, hairstyle, and background layout. She only makes tiny idle micro-movements that match a poised, bright, teasing AI-girlfriend livestream personality; elegant, self-aware, and camera-fluent: one hand lifts only slightly from the original hand area to lightly settle a hair end near the shoulder, then returns to the original hand position. The movement amplitude stays very small, the action stays close to the original pose, her posture remains almost unchanged, and her gaze returns to the camera. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2. No speaking, no dialogue, no subtitles, no on-screen text, no new objects, no shot cuts, no transitions, no camera movement.

overall_soundscape: N/A

non_diegetic_music: N/A
```

### 示例 4 · 极简版（推近拉远高发角色用 · 张天爱反例沉淀）

> 当角色首帧构图不稳或 idle 反复推近拉远时，用此极简结构——只写表情，不写手部动作，压缩 prompt 长度，减少模型注意力分散。

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Locked-off camera, authentic phone livestream realism, one continuous front-facing idle shot. The adult woman begins in the exact Picture 1 seated pose. The composition, subject size, chair position, visible floor area, and background landmarks remain unchanged for the full 8 seconds. She performs one tiny expression-only idle action, then returns to the exact Picture 2 pose.

overall_soundscape: N/A

non_diegetic_music: N/A
```

## 检查清单

- [ ] 是否是 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有 `[Shot 1]`，没有分镜、时间轴、转场？
- [ ] 是否静默无口播？
- [ ] 是否只有微动作，没有变成 Action 或 Gift？
- [ ] 是否没有新增道具、UI、字幕、屏幕文字？
- [ ] 是否固定镜头，人物大小和背景位置不变？
- [ ] 是否回到参考图同一姿态、手位、视线和构图？
