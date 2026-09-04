# 直播在场感

## 目标

当前 latest 版本的视频靠真实摄像头画面、稳定锚点、轻微自然动作、清晰背景和人物性格建立直播在场感。

- Idle：静默等待状态。
- Action：静默按钮动作。
- Gift：虚拟礼物响应，可以有一句短中文感谢口播。
- Typing：等待回复/处理输入状态，不说话，只保留输入相关音效。
- Preview：首页/角色卡片吸引点击，静默，动作按人物底色差异化。

## 通用硬禁项

- 不凭空增加参考图里看不到的实体物体。
- 不生成字幕、画面文字、歌词或 UI 界面。
- 不用分镜、阶段、时间轴或按秒拆动作。
- 不改变场景、服装、背景和人物构图。
- 不让镜头推近、拉远、平移、摇晃、跟拍或重新构图。

## Idle

Idle 不说话，不写台词，不写环境声或音效。可用动作：

- 轻微呼吸
- 一两次自然眨眼
- 很小幅度微笑变化
- 轻微点头后回到原姿态
- 眼神短暂偏向镜头旁边再回到镜头
- 手部保持在原位置，只允许极小幅度自然松弛

## Action

Action 不说话，不写台词，不写环境声或音效。可用动作：

- 一个简单、连续、低风险的动作
- 低位小挥手
- 紧凑比心
- wink
- 侧看再回镜头
- 手指在原手位轻微调整

## Typing

Typing 不说话，不写台词，不写字幕、评论 UI、屏幕文字或直播界面。
Typing 建议 5 秒或 8 秒；敲键盘段只持续约 3-4 秒，其余时间用于看向镜头下方、表情反应和回到首帧姿态。

优先使用两种稳定结构：

- 画面外键盘打字声：人物看向镜头下方，自己向前走近一小步，在画面下方附近做小幅打字动作；键盘始终在画面外，画面里不出现键盘、手机、平板或电脑。
- 侧边递手机：画面侧边只出现一只手和远峰蓝 iPhone 13 Pro；人物低位接过、轻点几下、还给同一只手，不出现对方脸、身体或其他部位。

Typing 的声音只写在 `overall_soundscape`，例如：

```text
overall_soundscape: A short, clear, non-harsh keyboard typing sound is heard from the offscreen area below the camera, synchronized with her finger typing motions. No human voice, no music, and no other sound.
```

关键稳定句：

```text
the composition change comes only from her body moving closer, while the camera stays completely fixed
```

## Preview

Preview 不说话，不写台词，不写字幕、评论 UI、屏幕文字、直播界面或礼物特效。

Preview 的吸引力来自真实直播中的镜头状态，而不是影楼摆拍或露骨动作：

- 眼神慢慢变柔或变亮
- 闭唇微笑、轻微挑眉
- 低位小挥手或低位小比心
- 轻整理发尾或耳侧发丝
- 短暂侧看再回到镜头
- 轻微歪头或姿态自然微调

避免：摸胸、亲吻镜头、舌头动作、明显性行为暗示、突然贴近镜头、身体大幅前倾、字幕/UI/新道具。

## Gift

Gift 可以有一句短中文口播。口播必须短、明确，并放在 `integrated_multimodal_description` 内的 `<d>[Chinese] ...</d>` 中。

可用结构：

```text
The adult fictional woman (S1), with a young adult East Asian female voice, says one short Chinese line: <d>[Chinese] 谢谢老公，爱你哟。</d> Her lips move naturally only for this single line, then close.
```

Gift 不写字幕，不写其他人声，不写连续聊天。`overall_soundscape` 只承认这一句口播：

```text
overall_soundscape: The only intended vocal event is her single short Chinese thank-you line. No added room ambience, no gift sound effect, and no other voices are intended.
```

## 写作原则

只描述参考图中已经确定存在的内容。无法确认的物体不要写名称，统一写：

```text
the visible background from the reference image remains unchanged and in focus
```
