---
name: spicycam-preview-latest
description: >
  Generate or audit Preview assets and MiniMax H3 FL2VA Preview prompts for
  spicycam AI livestream role-card videos. Preview is the homepage/role-card
  attract video and should use a stronger image-edited anchor before video.
---

# Spicycam Preview 提示词 Skill

Preview 是首页/角色卡片视频。它不是进入直播间后的 loop，而是用户在首页看到的第一眼吸引素材；点击进入后才播放 Idle / Typing / Action / Gift 等 loop。

Preview 的商业目标是提高点击率，因此画面可以比 Idle 更有成人向吸引力；但必须区分“真实明星/可识别真人”和“原创虚构成人角色”。真实明星、网红、素人或任何可识别真人素材只能做非露骨角色预览；成人向强暗示或更私密的 Preview 只适用于原创、授权、不可识别真实身份的成人虚构角色。

## 核心定位

1. Preview 目标是让用户快速理解角色气质并愿意点击进入。
2. Preview 可以比 Idle 更吸引人，但仍然保持真实手机直播画面，不做影楼写真、MV、短剧或广告片。
3. Preview 先做更强的角色卡首帧/锚点图，再用 MiniMax H3 FL2VA 做 8 秒首尾一致视频。
4. Prompt 不写真实姓名，统一写 `the adult fictional woman in the reference pictures`。
5. 对真实明星、网红、素人或任何可识别真人，不写露骨性行为、裸体性行为、真实身份成人化或规避平台限制的内容。
6. 对原创虚构成人角色，可以做更私密、更成人向的吸引预览，但仍应以镜头眼神、姿态、亲密氛围和暗示性互动为主，不把露骨动作模板写成通用生产规则。

## Preview 首帧图像编辑

Preview 不建议直接拿普通 Idle 首帧开跑。先用图像编辑模型做一张更适合首页展示的 Preview anchor：

- 明星/演员类角色：优先使用本地部署的 Krea 图像编辑流程，结合本地明星 LoRA 或已有明星首帧，生成更有首页吸引力但仍真实的直播角色图；仅限非露骨 Preview，不用于明确性行为画面。
- 网红、素人或普通人：可以使用百炼 Qwen 3.0 Pro 图像编辑，或其他质量稳定的图像编辑模型；输入应是清晰、1080P 以上、脸部和身体信息足够的照片；仅限已授权或可合规使用的非露骨角色图。
- 成人虚构角色：如果要做更私密、更成人向的 Preview，应使用原创虚构身份和授权素材；可选本地 MiniMax 高清帧流程或 Wan 3.0 等视频模型生成更强吸引力的首页预览。
- 如果云端图像编辑平台因为明星、网红、真实人物或成人内容限制拒绝生成，不应把本地模型写成“绕过限制”的手段；应改用原创虚构成人角色、授权素材或降低为非露骨 Preview。
- 图像编辑只允许优化角色气质、光线、服装状态、背景整洁度和直播感；不能把人物改成不像本人，不能生成不符合直播场景的写真、海报、电影构图。
- Preview anchor 仍然必须满足 `01_role/first_frame_standard.md`：正面、平视、竖版 9:16、人物居中、人物和背景同样清晰、真实高清直播感。

## 视频规则

1. 使用 MiniMax H3 FL2VA，Picture 1 和 Picture 2 使用同一张 Preview anchor。
2. 固定 8 秒，单镜头 `[Shot 1]`，首尾回到同一姿态、手位、视线、人物大小、背景和构图。
3. 镜头必须完全固定：不推近、不拉远、不缩放、不平移、不摇晃、不跟拍、不重新构图、不切镜头。
4. 动作按人物性格差异化，重点是眼神、微笑、发丝整理、低位小手势、姿态微调。
5. Preview 可以暧昧、有吸引力。真实明星/可识别真人版本不写裸露、性行为、明显挑逗身体部位、摸胸、亲吻镜头或过度擦边动作。
6. 原创虚构成人角色版本可以更私密、更成人向，但仍要避免把具体露骨性行为姿势写成通用模板；优先使用成人氛围、眼神、贴近感、姿态张力和不露脸伴侣的存在感来吸引点击。
7. 不说话，不写对白，不生成字幕、屏幕文字、评论 UI、直播平台界面、礼物特效或新道具。

## 稳定动作

- 清冷温婉型：低眸、眼神变柔、轻整理发丝、安静微笑。
- 阳光甜妹型：明亮微笑、低位小挥手、轻微歪头、短暂俏皮眨眼。
- 明艳轻熟型：稳定直视、细微挑眉、侧眸回镜头、克制营业微笑。
- 文艺治愈型：慢眨眼、侧看再回、轻整理发尾、温柔点头。
- 冷艳型：高冷直视、眼神慢慢变软、极小低位手势、克制微笑。
- 甜软撒娇型：可爱眨眼、低位小比心、轻微歪头、黏人式回看镜头。
- 健康运动型：自信微笑、清爽小点头、低位招呼、自然姿态调整。

## 成人向 Preview 边界

- 仅适用于原创虚构成年人、授权成人素材或不可识别真实身份的合成角色。
- 不用于真实明星、演员、网红、素人或任何可识别真人的露骨性行为预览。
- 如果画面需要暗示伴侣存在，只允许不露脸、不可识别身份的局部存在感；不要把真实人物身份、脸部或可识别特征带入。
- 重点写“首页吸引力”：眼神看镜头、呼吸节奏、姿态张力、亲密距离感、灯光氛围、短暂停顿和回到锚点。
- 仍然遵守 H3 FL2VA：单镜头、8 秒、固定机位、首尾一致、人物大小不变、背景不变。
- 使用本地 MiniMax 高清帧流程或 Wan 3.0 时，也必须先确认 Preview anchor 合规且满足真实高清直播首帧标准。

## 推荐结构

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static Preview shot for an AI-girlfriend homepage role card. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, subject size, head position, body position, hand position, background landmarks, lighting, outfit, hairstyle, and background layout. She performs one tasteful attractive preview action that matches her personality: [personality-matched action]. The action feels like a real livestream host using eye contact and subtle body language to attract attention, not like a photoshoot pose, not explicit, not nude, and not a sexual act. She does not step toward the lens and the subject size remains unchanged. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2. No speaking, no dialogue, no subtitles, no on-screen text, no AR gift, no physical gift, no new props, no shot cuts, no transitions, no camera movement.

overall_soundscape: N/A

non_diegetic_music: N/A
```

## 检查清单

- [ ] 是否先做了 Preview anchor，而不是直接复用普通 Idle 首帧？
- [ ] 明星/演员类是否优先使用本地 Krea 图像编辑，且只做非露骨 Preview？
- [ ] 网红/素人是否使用百炼 Qwen 3.0 Pro 或同等级图像编辑模型，且素材来源可合规使用？
- [ ] 成人向强吸引 Preview 是否只用于原创虚构成人角色或授权成人素材？
- [ ] Preview anchor 是否仍满足真实高清直播首帧标准？
- [ ] 是否是 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有 `[Shot 1]`，没有分镜、时间轴、切镜和转场？
- [ ] 是否固定镜头，没有推近、拉远、缩放、平移、摇晃、跟拍？
- [ ] 是否根据人物底色做了差异化动作，而不是每个人都同一个表情？
- [ ] 真实明星/可识别真人版本是否暧昧但不露骨，不摸胸、不亲吻镜头、不做性行为？
- [ ] 原创虚构成人版本是否保持固定镜头、首尾一致，并避免把露骨性行为姿势写成通用模板？
- [ ] 是否没有对白、字幕、屏幕文字、UI、礼物特效和新道具？
- [ ] 结尾是否回到同一姿态、手位、视线、人物大小、背景和构图？
