---
name: spicycam-gift-latest
description: >
  Generate or audit MiniMax H3 FL2VA Gift prompts for AI-girlfriend livestream
  characters. Gifts are virtual livestream AR effects. Each clip is an
  8-second first-last-frame loop and may include one short Chinese thank-you line.
---

# Gift Skill

> Gift 是送礼响应状态。当前版本用于 MiniMax H3 FL2VA 首尾帧视频。
> Picture 1 和 Picture 2 使用同一张参考图，礼物特效在尾帧前完全消散，人物回到原姿态。

## 核心规则

1. 每条 Gift 是一个 4-15 秒单镜头 FL2VA loop（默认 8 秒）。
2. 第一行必须写 H3 首尾帧对齐说明。
3. 正文必须使用三段结构：`integrated_multimodal_description`、`overall_soundscape`、`non_diegetic_music`。
4. 礼物只能是直播平台虚拟 AR overlay、粒子、贴纸、平台礼物动画；不能是实体礼物。
5. 不出现男性手部、实物递入、交接、归还动作。
6. 礼物在画面边缘、人物旁边或人物后方的开放区域出现，不能挡脸、手、身体和关键背景。
7. 人物反应要比 idle 明显：先真实看到礼物，再惊喜或开心回应镜头，最后回到参考图姿态。
8. Gift 可以有一句短中文口播，但只能一句，不能连续聊天，不能出现字幕或屏幕文字。
9. 有口播时必须使用 H3 原生音频链路：`minimax_h3_audio_vae_fp32.safetensors`、`VAEDecodeAudio`，并把 audio 接入 `VHS_VideoCombine`。

## 人设驱动

先给角色定义 AI 女友直播人格，再写反应。不要所有角色都用同一个表情模板。

- 温柔型：眼神柔、笑得慢，感谢时像被打动。
- 明艳型：反应明亮、自信，感谢时有营业感但不机械。
- 甜妹型：惊喜更明显，小幅挥手、比心、轻微点头。
- 清冷型：先克制，眼神慢慢变软，再给一个小笑。
- 健身/职业型：反应干净、直接、健康，不要过度撒娇。
- 文艺型：笑容轻，眼神停留更久，动作更优雅。

## 可用口播

口播只放在 `integrated_multimodal_description` 里，使用官方 H3 写法：

```text
The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢爸爸。</d> Her lips move naturally only for this single line, then close.
```

可选短句：

- `谢谢爸爸。`
- `谢谢老公。`
- `谢谢老公，爱你哟。`
- `谢谢大哥。`
- `谢谢老板。`
- `谢谢你的城堡。`
- `谢谢你的火箭。`
- `谢谢你的皇冠。`

## 礼物特效库

当前批量生产优先使用这 15 个，和已跑过的景甜 gift 对齐：

1. Small Hearts / 小心心
2. Cake / 蛋糕
3. Butterfly Group / 蝴蝶群
4. Cherry Blossom / 樱花
5. Fireworks / 烟花
6. Qixi Magpie Bridge / 七夕鹊桥
7. Castle / 城堡
8. Rocket / 火箭
9. Ferris Wheel / 摩天轮
10. Starry Galaxy / 星空银河
11. Flower Sea / 花海
12. Sports Car / 跑车
13. Carnival / 嘉年华
14. Crown / 皇冠
15. Paper Plane / 纸飞机

## 稳定写法

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. A livestream virtual [gift effect] appears as a platform AR overlay near the outer frame area, never becoming a physical object and never covering her face, hands, body, clothing, or important room details. She notices the virtual gift and reacts according to her personality: [personality-matched surprise and thank-you reaction]. The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] [one short thank-you line]</d> Her lips move naturally only for this single line, then close. The AR gift effect dissolves completely before the final frame. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2.

overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.

non_diegetic_music: N/A
```

## 实测经验

- 景甜 `gift_prompt_v3_official.md` 比早期版本更稳定，原因是用了官方 H3 三段结构、单镜头、清晰 AR overlay、人物反应和回锚点。
- Gift 反应可以比 idle 大，但不要写连续动作链。推荐：眼睛睁大、眉毛抬起、捂嘴、双手抬到胸前、低位比心、小幅点头。
- 大礼物可以明显惊喜，小礼物可以轻微甜笑。不要所有礼物都同样反应。
- 不要写 `comments`、`app interface`、`screen overlay text`，容易生成奇怪 UI 或字幕。
- 礼物效果要“出现-发光/粒子-消散”，不能变成实体，也不能留到尾帧。

## 完整示例

以下四条均为批量生产验证通过的完整 Gift prompt，AR overlay 格式，覆盖不同礼物类型和反应风格。

### 示例 1 · 玫瑰花（AR overlay · 御姐型 · 闻花反应）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman with long black hair over her shoulders, in a black lace bra and sheer black thigh-high stockings, kneeling on the dark gray floor mat before the wooden bookshelf, begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. A livestream virtual rose gift appears as a platform AR overlay, a cluster of glowing red roses blooming open at the outer frame edge beside her, never becoming physical objects and never covering her face, hands, or body. She leans her head slightly toward the blooms, eyes closing for one slow pretend inhale, then opens them half-lidded at the camera with a small satisfied smile. The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢你的玫瑰。</d> Her lips move naturally only for this single line, then close. The AR roses dissolve completely before the final frame. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2 at the end of the shot.

overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.

non_diegetic_music: N/A
```

### 示例 2 · 棒棒糖（AR overlay · 御姐型 · 舔嘴反应）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman with long black hair over her shoulders, in a black lace bra and sheer black thigh-high stockings, kneeling on the dark gray floor mat before the wooden bookshelf, begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. A livestream virtual lollipop gift appears as a small platform AR prop at her right hand, a glossy round candy on a stick glowing softly; she lifts it, brings it to her mouth, and draws one slow deliberate lick along the edge with half-lidded eyes held on the camera, then parts her lips around it for a brief suck before lowering the hand, the candy dissolving into soft particles. The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢老公的糖。</d> Her lips move naturally only for this single line, then close. The AR lollipop dissolves completely before the final frame. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2 at the end of the shot.

overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.

non_diegetic_music: N/A
```

### 示例 3 · 飞吻卡片（AR overlay · 御姐型 · 回飞吻反应）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman with long black hair over her shoulders, in a black lace bra and sheer black thigh-high stockings, kneeling on the dark gray floor mat before the wooden bookshelf, begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. A livestream virtual kiss-card gift appears as a platform AR overlay, a small glowing card with a lipstick kiss mark floating at the outer frame side, never becoming a physical object and never covering her face, hands, or body. She watches it, presses one hand flat over her heart as if catching the kiss, then extends the same hand forward at chest height to send a kiss back with a slow flick of the wrist, half-lidded. The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢老公，爱你哟。</d> Her lips move naturally only for this single line, then close. The AR card dissolves completely before the final frame. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2 at the end of the shot.

overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.

non_diegetic_music: N/A
```

### 示例 4 · 香槟（AR overlay · 御姐型 · 举杯反应）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman with long black hair over her shoulders, in a black lace bra and sheer black thigh-high stockings, kneeling on the dark gray floor mat before the wooden bookshelf, begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. A livestream virtual champagne gift appears as a platform AR overlay, a glowing bottle at the outer frame edge popping with a spray of golden sparkle particles that drift and fade, never becoming a physical object and never covering her face, hands, or body. She lifts one hand in a small elegant toast gesture toward the camera, chin raised a fraction, eyes bright and half-lidded, holding the toast a beat before lowering the hand back to her thigh. The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢老板的香槟。</d> Her lips move naturally only for this single line, then close. The AR bottle and sparkles dissolve completely before the final frame. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2 at the end of the shot.

overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.

non_diegetic_music: N/A
```

## 检查清单

- [ ] 是否是 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有 `[Shot 1]`，没有分镜、时间轴、转场？
- [ ] 是否固定镜头，没有推近、拉远、平移、摇镜、跟拍？
- [ ] 礼物是否是虚拟 AR overlay，不是实体礼物？
- [ ] 礼物是否完全消散，人物是否回到尾帧原姿态？
- [ ] 口播是否只有一句，是否用 `<d>[Chinese] ...</d>`？
- [ ] 是否没有字幕、屏幕文字、其他人声或额外音乐？
