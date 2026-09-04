# Prompt Quality Notes

## Current Production Target

当前项目只做 MiniMax H3 FL2VA 首尾帧视频：

- Picture 1 和 Picture 2 使用同一张参考图。
- Idle / Gift / Action / Preview 使用 8 秒；Typing 使用 5 秒或 8 秒。
- 只有 `[Shot 1]`，不写分镜、不写时间轴、不写多段剧情。
- Idle 和 Action 默认静默。
- Gift 可以一句短中文感谢口播，必须走 H3 原生音频链路。
- Typing 默认 5 秒短版或 8 秒长版，不说话，只保留输入相关音效。敲键盘段只持续约 3-4 秒。
- Preview 默认静默，用人物性格驱动的眼神、微笑、发丝整理和低位手势吸引点击。
- 同一个 prompt 至少跑 3 个候选，因为 H3 有概率波动。

## Good Prompt Pattern

Use short, positive, composition-anchored English prompts in official H3 format.

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. She performs one personality-matched action close to the original pose, then returns to the exact Picture 2 pose, gaze, hand position, subject size, and framing.

overall_soundscape: N/A

non_diegetic_music: N/A
```

## Why This Works

- The first line tells H3 this is FL2VA and exactly where the first and last frames land.
- The action is inside a single `[Shot 1]`.
- The camera lock is written as positive composition preservation: lens focal length, subject size, hand position, background landmarks.
- The action happens close to the original pose.
- The ending explicitly returns to Picture 2.

## Learned From Accepted Runs

- 景甜 gift v3 became more stable after switching to official H3 structure and writing the virtual gift as an AR overlay around the frame edge.
- 杨幂 idle outputs were stable when the prompt stayed close to the anchor pose and used low-amplitude streamer behavior.
- 景甜 action 03 and 06 improved when accepted from `candidate2`, showing that the same action can pass after rerun because generation is probabilistic.
- 景甜 action 05 improved after replacing outfit/body wording with original-hand-area finger adjustment.
- The real-star typing batch was stable when the keyboard stayed fully offscreen, the camera stayed locked, and the prompt explicitly said the visible composition change comes only from the subject stepping closer.
- Typing prompts became easier to review when each character used the same two actions: offscreen keyboard typing and low-position Sierra Blue iPhone 13 Pro handoff.
- The 10-second keyboard typing version was usable but too long for waiting/reply UI; current production uses two variants per character, one 5-second short version and one 8-second long version, with only 3-4 seconds of actual typing.

## Stable Idle Actions

- natural blink and soft smile
- tiny nod then return
- eyes glance slightly beside the lens, then return
- fingers relax slightly at the original hand position
- hands loosen and overlap again
- subtle breathing and posture settle
- very small head tilt and return
- low fingertip wave near the original hand area

## Stable Action Choices

- compact two-hand heart below chin
- playful wink while hands stay near the original hand position
- side glance and return
- small graceful bow-like nod
- fingertip wave below chest height
- hands loosen slightly, fingers adjust against each other, then return

## Stable Preview Choices

- Cool gentle characters: lowered glance, softened eyes, restrained smile, light hair-end touch.
- Sunny sweet characters: bright smile, tiny head tilt, low fingertip wave, compact low heart.
- Mature camera-aware characters: steady eye contact, subtle eyebrow lift, side glance return, polished closed-mouth smile.
- Soft healing characters: slow blink, shy lowered glance, gentle nod, quiet warm smile.
- Cool elegant characters: composed direct gaze, eyes soften slowly, minimal low hand gesture.
- Fitness/healthy characters: confident smile, compact greeting wave, small upbeat nod, natural posture settle.
- Preview should be attractive through gaze and rhythm, not explicit body contact.

## Stable Typing Choices

- Offscreen keyboard typing: she glances below the lens, steps forward by herself only one small step, makes small typing motions near the lower frame edge, then steps back to the anchor.
- Keep the actual keyboard tapping segment around 3-4 seconds; do not let the full video become continuous typing.
- Keep the keyboard fully offscreen unless the reference frame already contains a keyboard and the user explicitly wants it visible.
- Put keyboard sound only in `overall_soundscape`: short, clear, non-harsh typing sound from the offscreen area below the camera, synchronized with finger motion.
- Phone handoff: one side hand briefly enters with a Sierra Blue iPhone 13 Pro, she keeps the phone low, taps briefly, returns it to the same hand, then returns to anchor.
- For phone handoff, explicitly say only the hand and phone are visible; no other person's face, body, arm, or extra body parts.
- Typing should feel like a real host temporarily replying to a message, not a performance action.

## Gift Reaction Choices

- Small gifts: soft smile, tiny nod, low fingertip wave.
- Medium gifts: eyes brighten, eyebrows lift, compact two-hand heart.
- Big gifts: brief surprise pause, widened eyes, both hands rise to chest-level area, short thank-you line, then return.

Use different reactions for different personalities. Do not make every character or every gift use the same expression.

## Avoid

- Storyboards, time splits, shot cuts, scene changes, transitions.
- Camera words that invite motion: `push in`, `pull out`, `zoom`, `tracking`, `pan`, `tilt`.
- Action words that pulled Jing Tian into near-crop frames: `shoulder height`, `hair along the side of her face`, `near her lips`, `blow kiss`, `upper chest`, `front line of her outfit`, `lean closer`.
- Words that make H3 invent UI or subtitles: `comments`, `screen`, `interface`, `app overlay`, `screen text`.
- Physical hand-offs inside Gift prompts.
- Tiny tools or objects that invite hand close-ups.
- Repeating risky words such as `close-up`, even inside `no close-up`; prefer positive anchors like `full-body subject size remains unchanged`.
- For Typing, avoid `chat window`, `screen comments`, `visible keyboard`, `computer desk`, `typing on laptop`, or `live interface` unless those objects are already visible in the reference frame.
- For Typing, avoid saying the camera follows her; say the camera is fixed and the composition change comes only from her body movement.
- For Preview, avoid explicit sexual/body-part wording, kissing the lens, leaning close, close-up framing, or photoshoot language.

## Output Convention

Every character should use this structure:

```text
generated/
  idle/
    accepted/videos/
    candidate/videos/
    candidate2/videos/
    candidate3/videos/
    rejected/videos/
    rejected/prompts/
    qc/
    notes/
    raw_runs/
  action/
    accepted/videos/
    candidate/videos/
    candidate2/videos/
    candidate3/videos/
    rejected/videos/
    rejected/prompts/
    qc/
    notes/
    raw_runs/
  gift/
    accepted/videos/
    candidate/videos/
    candidate2/videos/
    candidate3/videos/
    rejected/videos/
    rejected/prompts/
    qc/
    notes/
    raw_runs/
  typing/
    accepted/videos/
    candidate/videos/
    candidate2/videos/
    candidate3/videos/
    rejected/videos/
    rejected/prompts/
    qc/
    notes/
    raw_runs/
    prompts/
  preview/
    accepted/videos/
    candidate/videos/
    candidate2/videos/
    candidate3/videos/
    rejected/videos/
    rejected/prompts/
    qc/
    notes/
    raw_runs/
```

## Rejection Notes

Bad videos should be kept only when useful for learning. Put the bad video in `rejected/videos/` and save the exact prompt in `rejected/prompts/` with the same base name. Notes should explain the visible failure:

- zoom / push-in / pull-out
- subject scale changed
- face or body cropped
- unwanted speech
- unwanted subtitle or UI
- gift became physical object
- action became multi-shot or multi-step
- preview became explicit, photoshoot-like, close-up, or too similar across characters
- typing generated visible keyboard/phone when it should be offscreen
- typing changed into dialogue, subtitles, chat UI, or platform interface
- subject walked too close and caused crop, or camera zoomed instead of subject movement

## QC Method

For every generated video, extract three frames around 0.5s, 4s, and 7.5s. Compare:

- subject size
- head position
- hand position
- background landmark position
- whether the gift fully disappears before the end
- whether idle/action remains silent
- whether gift has only the requested one-line speech
- whether typing has only the intended keyboard/touchscreen sound and no speech
