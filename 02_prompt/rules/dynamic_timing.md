# 动态时机与上下文逻辑

当前 `spicycam-prompt-latest` 只服务 `public_live` 与 `private_live` 两种直播间模式，状态范围限定为 `Idle`、`Gift`、`Action`、`Typing`、`Preview`。

## 固定时长

当前版本时长：

- `Idle`: 8s
- `Gift`: 8s
- `Action`: 8s
- `Typing`: 5s or 8s
- `Preview`: 8s

Idle / Gift / Action / Preview 必须写明：

```text
Duration: 8 seconds. First frame and last frame must match the same mode anchor pose, seamless loop.
```

Typing 必须写明：

```text
Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 5.00-second or 8.00-second mark of the target video.
```

## 首尾帧规则

- 首帧必须来自当前模式已验收的 anchor。
- 尾帧必须回到同一 anchor 姿态，不能切到其他空间、其他服装状态或其他镜头角度。
- 镜头固定、眼平视角、人物居中，保持真实高清手机直播质感。
- 人物和背景都清晰，不使用电影感、浅景深、人像模式或背景虚化。

## 动作幅度

`Idle` 只允许微动作：

- 眨眼、呼吸、轻微微笑
- 小幅整理头发或衣袖
- 轻微看弹幕后回到镜头
- 手保持在膝盖、桌面或道具附近

`Gift` 是收到虚拟礼物后的即时反应：

- 看见直播 AR 特效
- 微笑、点头、挥手或双手比心
- 可以说一句很短的中文感谢，例如“谢谢爸爸。”或“谢谢老公，爱你哟。”
- AR 礼物特效在画面前景出现和淡出
- 结尾回到当前模式 anchor

`Action` 是单段动作按钮响应：

- 只做一个清晰动作，不拆成多段链路。
- 动作要适合当前模式和角色职业。
- 不写连续流程、强剧情或多段拼接。

`Preview` 是首页/角色卡片吸引点击状态：

- 8 秒首尾一致，静默。
- 按人物底色做差异化：清冷、甜妹、轻熟、治愈、冷艳、运动感等不能用同一套表情。
- 只用眼神、微笑、发丝整理、低位小手势和姿态微调建立吸引力。
- 不写裸露、性行为、摸胸、亲吻镜头、字幕、UI、礼物特效或新道具。

`Typing` 是等待回复/处理消息状态：

- 不说话，不生成字幕、评论 UI 或屏幕文字。
- 优先使用“画面外键盘打字声”：人物看向镜头下方，自己向前一小步，手指在画面下方附近做小幅打字动作，键盘始终在画面外。
- 也可使用“侧边递远峰蓝 iPhone 13 Pro”：只出现一只手和手机，人物低位接过、轻点、还回去。
- Typing 总时长为 5 秒或 8 秒；如果是敲键盘，实际敲击动作只持续约 3-4 秒。
- 声音只写画面外键盘声或轻微触屏声，不写环境声、音乐和人声。
- 结尾回到当前模式 anchor，人物大小和背景构图一致。

## 写作前检查

- [ ] 是否是 `public_live` 或 `private_live`？
- [ ] 是否只生成 `Idle`、`Gift`、单段 `Action`、`Typing` 或 `Preview`？
- [ ] Idle / Gift / Action / Preview 是否固定 8 秒，Typing 是否为 5 秒或 8 秒？
- [ ] 首尾帧是否一致并回到同一模式 anchor？
- [ ] 是否符合 `first_frame_standard.md` 的真实高清直播首帧标准？
- [ ] Idle / Action 是否没有说话、台词、字幕、环境声和凭空新增道具？
- [ ] Gift 如果有口播，是否只有一句短中文，且没有字幕和其他人声？
- [ ] Typing 是否没有对白和字幕，且只保留输入相关音效？
