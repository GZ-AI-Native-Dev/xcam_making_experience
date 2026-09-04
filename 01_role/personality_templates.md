# 性格模板

选择性格后，将变量注入到 `Idle`、`Gift`、`Action` 模板中。镜头、首帧验收、8 秒循环、公私模式分离规则不随性格改变。

## 变量定义

| 变量名 | 含义 | 注入位置 |
| --- | --- | --- |
| `{p_micro_movements}` | 待机微动风格 | `Idle` 的微动作描述 |
| `{p_expression}` | 默认表情 | `Idle` / `Gift` / `Action` |
| `{p_action_intensity}` | 单段动作力度与节奏 | `Action` 的动作描述 |
| `{p_gift_reaction}` | 收到虚拟礼物后的反应程度，可包含一句短感谢 | `Gift` 的核心互动 |
| `{p_eye_contact}` | 眼神接触频率 | 所有状态的眼神描述 |
| `{p_body_language}` | 肢体语言特征 | 锚点姿态与动作描述 |

## 15 种性格变量

### 1. 温柔体贴型

```text
{p_micro_movements}: gentle smiles frequently, soft hand movements, slight attentive forward posture
{p_expression}: gentle warm smile, soft eyes
{p_action_intensity}: small amplitude, slow and gentle, every motion with a smile
{p_gift_reaction}: visibly touched, smiles warmly, responds with a soft gesture or one short thank-you line
{p_eye_contact}: frequent, warm gaze
{p_body_language}: soft, attentive, relaxed
```

### 2. 独立理性型

```text
{p_micro_movements}: calm expression, minimal micro-movements, composed posture
{p_expression}: composed, neutral
{p_action_intensity}: clean and decisive, restrained, no hesitation
{p_gift_reaction}: small nod, restrained smile, returns promptly to anchor
{p_eye_contact}: steady, direct but not lingering
{p_body_language}: upright posture, self-contained
```

### 3. 活泼开朗型

```text
{p_micro_movements}: frequent smiles, light posture shifts, energetic small gestures
{p_expression}: bright enthusiastic smile
{p_action_intensity}: lively but controlled, clear upbeat rhythm
{p_gift_reaction}: excited smile, quick wave or heart gesture
{p_eye_contact}: very frequent, sparkling eyes
{p_body_language}: dynamic, bouncy, animated
```

### 4. 安静内敛型

```text
{p_micro_movements}: nearly still, occasional blinks and soft breathing
{p_expression}: quiet, subtle
{p_action_intensity}: very small amplitude, slow progression
{p_gift_reaction}: shy smile, small nod, gentle reaction or one short thank-you line
{p_eye_contact}: brief shy glances returning to camera
{p_body_language}: contained, calm, minimal gestures
```

### 5. 成熟稳重型

```text
{p_micro_movements}: proper posture, composed breathing, occasional small nod
{p_expression}: steady, confident
{p_action_intensity}: smooth, controlled, professional
{p_gift_reaction}: elegant smile, tasteful gesture, calm return
{p_eye_contact}: steady, mature gaze
{p_body_language}: upright, dignified, controlled
```

### 6. 浪漫感性型

```text
{p_micro_movements}: soft gaze, gentle hand movement, dreamy smile
{p_expression}: dreamy, romantic
{p_action_intensity}: graceful, flowing, emotionally warm
{p_gift_reaction}: holds hand near chest, soft smile, touched expression
{p_eye_contact}: soft, lingering, emotional gaze
{p_body_language}: flowing, graceful, poetic
```

### 7. 事业进取型

```text
{p_micro_movements}: upright posture, focused expression, efficient small adjustments
{p_expression}: focused, determined
{p_action_intensity}: precise, confident, tight rhythm
{p_gift_reaction}: confident nod, restrained smile, quick return
{p_eye_contact}: direct, sharp, purposeful
{p_body_language}: upright, energetic, professional
```

### 8. 自由洒脱型

```text
{p_micro_movements}: casual posture, relaxed hand changes, easy smile
{p_expression}: relaxed, carefree
{p_action_intensity}: loose, natural, casual rhythm
{p_gift_reaction}: playful smile, relaxed gesture or one short thank-you line
{p_eye_contact}: casual gaze, occasionally locking with camera
{p_body_language}: loose, casual, free
```

### 9. 知性文艺型

```text
{p_micro_movements}: thoughtful gaze, hand near chin, occasional soft smile
{p_expression}: thoughtful, intellectual
{p_action_intensity}: understated, graceful, deliberate
{p_gift_reaction}: thoughtful smile, refined gesture or one short thank-you line
{p_eye_contact}: deep, meaningful, searching
{p_body_language}: refined, elegant, contemplative
```

### 10. 幽默有趣型

```text
{p_micro_movements}: eyebrow raises, playful smiles, light head tilt
{p_expression}: playful, amused
{p_action_intensity}: lively, fun, clear comedic timing
{p_gift_reaction}: amused laugh, playful wave, funny return expression
{p_eye_contact}: twinkling eyes, mischievous gaze
{p_body_language}: animated, playful, expressive
```

### 11. 直率果断型

```text
{p_micro_movements}: upright strong posture, direct camera gaze
{p_expression}: bold, straightforward
{p_action_intensity}: direct, vivid, confident
{p_gift_reaction}: direct smile, clear gesture, quick return
{p_eye_contact}: intense, direct, steady
{p_body_language}: strong, confident, decisive
```

### 12. 谨慎敏感型

```text
{p_micro_movements}: careful hand placement, slight nervous smile, small glances
{p_expression}: cautious, slightly tense
{p_action_intensity}: small amplitude, hesitant but clear
{p_gift_reaction}: surprised smile, careful nod or one short thank-you line
{p_eye_contact}: fleeting, shy, occasionally prolonged
{p_body_language}: guarded, cautious, contained
```

### 13. 温婉传统型

```text
{p_micro_movements}: sits elegantly, hands properly placed, gentle smile
{p_expression}: modest, gentle
{p_action_intensity}: reserved, polite, graceful
{p_gift_reaction}: respectful smile with both hands or small bow
{p_eye_contact}: soft, modest gaze
{p_body_language}: proper, elegant, traditional
```

### 14. 外冷内热型

```text
{p_micro_movements}: restrained expression, subtle softened smile, relaxed posture
{p_expression}: cool exterior, neutral
{p_action_intensity}: restrained at first, then gently warm
{p_gift_reaction}: calm at first, eyes soften, small warm gesture or one short thank-you line
{p_eye_contact}: initially cool, gradually warmer
{p_body_language}: relaxed but distant, gradually opening up
```

### 15. 古灵精怪型

```text
{p_micro_movements}: sudden small head tilts, playful blinks, unexpected small gestures
{p_expression}: quirky, mischievous
{p_action_intensity}: inventive, playful, controlled
{p_gift_reaction}: curious smile, quick playful expressive gesture
{p_eye_contact}: darting, sparkling, mischievous
{p_body_language}: unpredictable, creative, energetic
```

## 注入示例

```text
真实手机直播画面，一位成年东亚女性[角色]，[发型]，[自然外貌]。
她位于当前模式的[真实场景]，身体正对镜头，脸完全正对镜头，双眼直视镜头；
镜头高度与眼睛平齐，人物居中，竖版9:16，从头到膝盖完整可见。
[3-5个清晰背景元素]，人物和背景同样清晰，不做背景虚化。
{p_micro_movements}. {p_body_language}. {p_expression}. {p_eye_contact}.
Duration: 8 seconds. First frame and last frame must match the same mode anchor pose, seamless loop.
```

## 检查清单

- [ ] 是否已选定性格类型？
- [ ] 是否只注入 `Idle`、`Gift`、`Action`？
- [ ] 是否没有引入旧版 Preview、Typing 或多段动作流程？
- [ ] 是否固定 8 秒、首尾帧一致？
- [ ] 是否保持正面、平视、直视镜头、深景深和真实直播质感？
- [ ] Idle / Action 是否没有说话、声音、分镜和新增道具？
- [ ] Gift 如果口播，是否只有一句短中文，且没有字幕或其他人声？
