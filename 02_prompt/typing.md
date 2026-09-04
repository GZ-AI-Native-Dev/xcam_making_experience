---
name: spicycam-typing-latest
description: >
  Generate or audit MiniMax H3 FL2VA Typing prompts for spicycam AI livestream
  characters. Typing is the waiting/reply state, using one fixed livestream
  shot, same first and last frame, and no dialogue.
---

# Typing Skill

> Typing 是 AI 直播女友等待回复时的状态视频。当前稳定版本用于 MiniMax H3 FL2VA 首尾帧视频。
> Picture 1 和 Picture 2 使用同一张参考图。Typing 使用 5 秒短版或 8 秒长版，避免 10 秒输入动作显得拖沓。

## 核心规则

1. 每条 Typing 都是一个 5 秒或 8 秒单镜头 FL2VA loop。
2. 第一行必须写 H3 首尾帧对齐说明，Picture 1 对齐 0.00 秒，Picture 2 对齐 5.00 秒或 8.00 秒。
3. 正文必须使用三段结构：`integrated_multimodal_description`、`overall_soundscape`、`non_diegetic_music`。
4. 镜头必须完全固定：不推近、不拉远、不缩放、不平移、不摇晃、不跟拍、不重新构图、不切镜头。
5. Typing 不说话，不写对白，不生成字幕、屏幕文字、评论 UI、直播界面或平台弹幕。
6. 动作要像真实主播临时处理输入/回复消息，动作结束后必须回到 Picture 2 的同一姿态、手位、视线、人物大小、背景和构图。
7. 每个角色的 Typing 当前默认跑 2 条候选：1 条 5 秒短版、1 条 8 秒长版；挑选时优先看镜头是否固定、人物比例是否稳定、动作是否自然。

## 时长策略

- 5 秒短版：适合快速等待状态。动作包括短暂看向镜头下方、走近或准备输入、约 3 秒画面外敲键盘、抬眼回镜头并回到首帧姿态。
- 8 秒长版：适合稍长等待状态。动作包括更自然的停顿和表情变化，但实际敲键盘段仍只持续约 3-4 秒，其余时间用于看向镜头下方、微笑、回位。
- 不建议在最终 prompt 里按秒拆成分镜。只写 “briefly”、“for about 3-4 seconds”、“then calmly returns”，避免模型生成分镜、转场或切镜。

## 当前稳定动作

### Typing 01 - 走近画面外键盘打字声

这个动作是目前实测质量较高的 typing prompt。

适用场景：

- 主播距离镜头稍远，需要表现“走近处理消息”。
- 希望有打字反馈，但不想让键盘出现在画面里。
- 等待 AI 回复时播放，强调真实直播感和画面外键盘声。

稳定原因：

- 明确写“画面变化只来自人物走近，摄像机完全不动”，能区分人物移动和镜头推近。
- 明确写“键盘始终在画面外”，减少凭空生成键盘、桌子或电脑。
- 声音写在 `overall_soundscape`，只保留画面外键盘敲击声，不让人物说话。
- 结尾写回到 Picture 2 的同一人物大小和构图，能压住首尾一致。

推荐英文结构：

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 5.00-second or 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman in the reference pictures begins in the exact same pose, hand position, expression, subject position, and visible background as Picture 1. The camera remains completely locked for the full target duration: no push-in, no pull-back, no zoom, no pan, no tilt, no shake, no tracking, no reframing, and no cuts. She first glances slightly below the lens as if handling a livestream message, then steps forward by herself only one small step toward the lower edge of the frame; the composition change comes only from her body moving closer, while the camera stays completely fixed. Near the lower edge of the frame, she makes small, realistic, quick typing motions with her fingers for about 3-4 seconds, but the keyboard stays fully offscreen. No keyboard, phone, tablet, laptop, monitor, or other visible device appears in the frame. Her expression and rhythm follow the character personality: natural, attentive, and like a real livestream host briefly replying to a message, not mechanical and not exaggerated. After typing, she raises her eyes back to the lens with a natural personality-matched smile, then calmly steps back to the original position. Her hands, body, gaze, subject size, background, and framing return to the exact same state established by Picture 2, forming a seamless first-last-frame loop. No dialogue, no subtitles, no screen text, no new props, no gift effect.

overall_soundscape: A short, clear, non-harsh keyboard typing sound is heard from the offscreen area below the camera, synchronized with her finger typing motions. No human voice, no music, and no other sound.

non_diegetic_music: N/A
```

推荐中文结构：

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 5.00-second or 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] 真实手机直播画面，单个竖屏正面固定镜头。参考图中的成年女性从 Picture 1 的同一姿态、手位、表情、人物位置和原始背景开始。镜头全程固定：不推近、不拉远、不缩放、不平移、不摇晃、不跟拍、不重新构图、不切镜头。她先短暂看向镜头下方，像在处理直播间消息，然后自己向前走近一小步，来到更靠近画面下方的位置；画面变化只来自人物走近，摄像机完全不动。她在画面下方附近做几下小幅、真实、快速的敲键盘动作，但键盘始终在画面外。画面中不出现键盘、不出现手机、不出现平板、不出现任何可见设备。她的表情和节奏要符合这个角色底色，动作自然像真实主播在直播间临时回复消息，不要机械、不要夸张。敲完后她抬眼看回镜头，露出符合人物性格的自然微笑，再平稳退回原来的位置，双手和身体恢复到 Picture 2 的同一姿态、同一手位、同一视线、同一人物大小、同一背景和同一构图，形成首尾一致循环。没有对白，没有字幕，没有屏幕文字，没有新道具，没有礼物特效。

overall_soundscape: 画面外传来短暂、清晰但不刺耳的键盘敲击声，声音来自镜头下方的画面外区域，并与她的手指敲击动作同步。没有人声，没有音乐，没有其他声音。

non_diegetic_music: N/A
```

### Typing 02 - 旁边递远峰蓝 iPhone 13 Pro

这个动作适合表现“旁边工作人员/用户递手机给主播处理消息”，但风险比画面外键盘更高。

适用场景：

- 需要手机进入画面，明确展示远峰蓝 iPhone 13 Pro。
- 人物不向前走，画面稳定性优先。
- 只允许出现侧边一只手和手机，不出现对方脸、身体或其他部位。

稳定写法：

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 5.00-second or 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman in the reference pictures begins in the exact same pose, hand position, expression, subject position, and visible background as Picture 1. The camera remains completely locked for the full target duration: no push-in, no pull-back, no zoom, no pan, no tilt, no shake, no tracking, no reframing, and no cuts. She stays in place and does not walk toward the camera. From one side of the frame, a single hand briefly enters holding a Sierra Blue iPhone 13 Pro; only this hand and the phone are visible, with no other person's face, body, arm, or extra body parts. She naturally takes the phone and keeps it low in the lower half of the frame. The Sierra Blue color and triple rear camera module are briefly recognizable, but the phone never covers her face, main body, hands, or important background. She looks down at the phone once and taps it a few times with her thumb as if quickly replying to a livestream message. Her expression and rhythm follow the character personality: natural, focused, and like a real livestream host handling a quick reply. Then she returns the phone to the same side hand, and the hand exits the frame with the phone. She looks back to the lens with a natural personality-matched smile. Her hands, body, gaze, subject size, background, and framing return to the exact same state established by Picture 2, forming a seamless first-last-frame loop. No dialogue, no subtitles, no screen text, no keyboard, no gift effect, no shot cuts, no transitions.

overall_soundscape: Very light fabric rustle and small finger-contact sounds are heard as the phone is passed, followed by brief touchscreen tapping synchronized with her thumb motion. No human voice, no music, and no other sound.

non_diegetic_music: N/A
```

## 写作注意

- Typing 不是普通 action，不要写成表演动作；它是“等待 AI 回复时的真实输入状态”。
- 如果参考图里没有桌子，不要写桌子；如果参考图里没有键盘，就让键盘保持画面外。
- 不要写 `typing on a visible keyboard`，除非首帧中已经有键盘并且用户明确要显示键盘。
- 不要写 `looking at chat window`、`live interface`、`screen comments`，容易生成字幕、UI 和平台界面。
- 对于画面外键盘，重点写声音来源：`offscreen area below the camera`。
- 对于手机版，手机必须保持低位，不能靠近脸，不能挡住身体。
- 动作幅度宁可小一点；走近只允许人物走近，不允许镜头运动。

## 检查清单

- [ ] 是否是 5 秒或 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有 `[Shot 1]`，没有分镜、时间轴、切镜和转场？
- [ ] 是否固定镜头，没有推近、拉远、缩放、平移、摇晃、跟拍？
- [ ] 是否没有对白、字幕、屏幕文字、评论 UI？
- [ ] 键盘版是否没有可见键盘/手机/设备，只有画面外键盘声？
- [ ] 手机版是否只出现一只侧边手和远峰蓝 iPhone 13 Pro？
- [ ] 结尾是否回到同一姿态、手位、视线、人物大小、背景和构图？
