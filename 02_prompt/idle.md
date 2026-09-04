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

## 高风险动作

- 大幅撩头发、摸脸、靠近嘴唇、吹吻。
- 离开画面再回来。
- 前倾读弹幕。
- 看手机、看屏幕、读 comments。
- 小工具、手部细节、实体交接。
- “全身”和“头到膝盖”混写。

如果生成出现推近拉远，优先把动作降级为眼神、微笑、点头、原手位手指微动。

## 检查清单

- [ ] 是否是 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有 `[Shot 1]`，没有分镜、时间轴、转场？
- [ ] 是否静默无口播？
- [ ] 是否只有微动作，没有变成 Action 或 Gift？
- [ ] 是否没有新增道具、UI、字幕、屏幕文字？
- [ ] 是否固定镜头，人物大小和背景位置不变？
- [ ] 是否回到参考图同一姿态、手位、视线和构图？
