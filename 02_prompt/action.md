---
name: spicycam-action-latest
description: >
  Generate or audit MiniMax H3 FL2VA ordinary Action prompts for AI-girlfriend
  livestream characters. 8-second single-action loops, same first/last frame,
  骚版生产层（吸引男用户），推近拉远为概率事件、靠候选兜底。
---

# Action Skill

> Action 是用户点击动作按钮后的普通单段响应，不是 Gift，也不是复杂剧情。
> 当前版本用于 MiniMax H3 FL2VA 首尾帧视频：Picture 1 和 Picture 2 使用同一张参考图。
> 本 skill 已经过 15 角色 × 8 动作 × 2 候选的批量生产验证（多卡 H3 管线，见 `03_video/h3_pipeline.md`）。

## 核心规则

1. 每条 Action 固定 8 秒。
2. 只写一个普通直播动作，不拆分镜，不按秒写阶段。
3. Action 不写口播台词（不用 `<d>`），但**允许非人声骚音景**：鼻音嗯~、喘气、湿吻声、布料/丝袜摩擦、肩带弹回声。Gift 才用人声台词。
4. 不新增参考图里没有的实体道具，不做礼物特效。
5. 镜头必须固定：写明 `the camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout`。
6. 结尾必须回到 Picture 2 的同一姿态、手位、视线、表情和构图——**包括锚点的特征手位**（见锚点回归）。
7. 骚版生产层默认开启（见下），人设只影响台词语气和微表情风格，不影响骚层。

## 官方 H3 FL2VA 格式（基线模板）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman in the reference pictures begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. She performs one ordinary livestream action that matches her personality: [one simple visible action]. The motion stays compact, continuous, and close to the original pose. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2.

overall_soundscape: N/A

non_diegetic_music: N/A
```

## 生产模板（骚版，批量验证通过）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 8.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic phone livestream realism, a single vertical front-facing static shot. The adult fictional woman with [发色发型+碎发细节], in [内衣/服装具体颜色款式 + with nipples pressing the lace/fabric], [锚点坐姿/站姿 + 双手确切位置], begins in the exact pose and framing established by Picture 1. The camera holds a static shot for the full 8 seconds, preserving the same lens focal length, full-body subject size, head position, body position, hand position, stable background landmarks, lighting, outfit, hairstyle, and background layout. [动作句：起手位→骚动作→回手位]. She returns to the exact pose, gaze, expression, hand position, subject size, and framing established by Picture 2 at the end of the shot.

overall_soundscape: [非人声骚音景：moan/breath/friction，2-4 项具体声音]

non_diegetic_music: N/A
```

要点：
- 开头人物描述必须**逐字对应首帧图**（发色、发型、内衣颜色款式、坐姿、手位、背景地标），模型同时看图和文字，文字越准漂移越小。
- 动作句必须**从锚点手位起手、回到锚点手位收尾**，中间才做骚动作。
- 开头用 `Live-action, authentic phone livestream realism` 比 `Authentic phone livestream realism` 更稳（批量采用前者）。

## 8 动作目录（生产按钮）

| key | 动作 | 骚版编排要点 |
|-----|------|-------------|
| waveHello | 单手打招呼 | 胸口位开掌挥2次 + wink/吐舌/咬唇，回手位 |
| kissOneHand | 单手飞吻 | 手到嘴、指尖压湿唇、胸口位送出吻（send the kiss，不写 blow kiss），回手位 |
| kissTwoHands | 双手飞吻 | 双手捂嘴闷哼、掌心向外送吻、唇角唾液丝，回手位 |
| hairPlay | 摆弄头发 | 发尾绕指、**发梢咬齿轻拽**、滑落到锁骨，回手位 |
| catTease | 猫式挑逗 | 拱背挺胸、乳尖顶布、大腿内磨、长喘滑成 moan，回正 |
| sexyTease | 性感挑逗 | 肩带慢拉到肩头弹回、指沿乳沟线/袜带描、指甲勾布边，回手位 |
| turnShake | 转身摆臀 | 起身→转半圈背对→屈膝摆臀3次→手滑自己髋→回眸吐舌→转回坐下回锚点 |
| breastSqueeze | 双手挤胸 | 侧托挤压、**乳肉溢出指缝**、头微仰闷哼、放开弹一下，回手位 |

## 锚点回归原则

首帧姿势分 7 族，写动作前先认族，结尾必须重建该族的确切手位：

- A 跪坐（双手搭大腿）：佟丽娅、刘浩存、虞书星
- B 坐姿双手搭膝：姜慧元、李沁、林允儿、王楚然、祝绪丹
- C 桌/床边双手撑面：张嘉倪、杨幂
- D 单手举起挥手：张天爱（**结尾必须把手举回挥手位**）
- E 前倾双手托胸：张婧仪（**结尾必须回到托胸位**）
- F 站立双手腹前交叠：景甜
- G 地毯坐双手撑后/侧：迪丽热巴、郑爽

规则：锚点有什么特征手位，结尾就还什么手位。turnShake 允许起身离座，但必须坐回并重建手位。

## 骚版风骚层（产品方向：吸引男用户，已验收）

每条 prompt 默认叠加：
- 视觉：`chest rising and falling visibly`、`nipples pressing the lace/fabric`、挤压时 `flesh bulges over her fingers`、放开 `bounce once`、`thighs pressing together and rubbing`、`lower lip caught between her teeth` / 吐舌、`flushed face`、`eyes going glassy / half-lidded`。
- 音景（action 非人声）：`a soft nasal moan`、`a shaky exhale sliding into a moan`、`wet kissy exhale`、`lace/stockings friction`、`a soft strap snap`。
- 人设只改微表情语气：甜妹=眨眼嘟嘴鼻音；御姐=慢抬眼掌控感；清冷=港风烟视媚行；怯羞=脸红眼神躲；教练=大方亮笑。

## 高风险词（触发推近/拉远/局部近景，必须降级）

- `one hand rises to shoulder height` → 手不超过胸口位（`at chest height`）
- `near her lips` → `to her mouth`
- `upper chest` → `collarbone / cleavage line`
- `lean closer` → 删除，改写身体内部动作
- `blow kiss` → `sends the kiss away`
- `hair along the side of her face` → `hair end near her shoulder`
- `front line of her outfit` → `hands loosen slightly at the original hand area`
- `read comments` / `screen` / `phone` / `small tool` / `hand-off` → 不写
- 不写 `no close-up` / `no zoom` 这类否定刺激词，改正向锚定：`full-body subject size remains unchanged`（已含在生产模板的 preserving 句里）

## 推近拉远 = 概率事件（专项说明）

**实测结论：即使 prompt 完全合规，仍有小概率出现推近/拉远。这是模型采样概率事件，不是措辞错误。**

降低概率的手段（已内置于生产模板）：
1. preserving 长句锁焦距+人物大小+背景地标。
2. 结尾回归句重复 `subject size, and framing`。
3. 高风险词降级。
4. 不用否定式镜头词。

兜底策略（验收流程）：
- 批量每条 prompt 跑 **2 候选**（吞吐优先），单条精验跑 3 候选。
- QC 抽 0.5s / 4s / 7.5s 三帧，看人物大小和背景位置是否一致。
- 两候选都推近拉远 → 重跑一次；仍坏 → 标记 rejected 保留 prompt。
- 好版本进 `results/{角色}/{cat}_{key}/candN.mp4`，坏版本不删、留 prompt 可复跑。

## 生成经验（批量管线）

- 管线：多卡 ComfyUI（H3 FL2VA + audio），driver 每卡一线程派单，runner 单条提交+轮询+下载；环境配置见 `03_video/h3_pipeline.md`。
- runner 曾有 `set -u` 坑：`local a="$1" b="${a}.x"` 同行自引用 → unbound 退出。已修（声明与赋值分两行）。
- ComfyUI `/history`、`/queue` 需 cookie-jar 登录（`-c -b` 同一 jar）；裸 curl 或过期 cookie 会返回登录页 HTML，收割器必须检测"非 JSON → 重新 login → 重试"。
- 同 prompt 有概率波动，候选之间差异主要来自采样，不来自 prompt 微调。

## 检查清单

- [ ] 是否 8 秒 H3 FL2VA，Picture 1 和 Picture 2 同图？
- [ ] 是否只有一个 `[Shot 1]`，开头 `Live-action, authentic phone livestream realism`？
- [ ] 人物描述是否逐字对应首帧（发色/内衣/坐姿/手位/背景）？
- [ ] 是否只有一个动作，且从锚点手位起手、回锚点手位收尾？
- [ ] 是否无口播（action 不用 `<d>`），音景只有非人声骚音？
- [ ] 是否叠了骚层（胸口起伏/乳尖顶布/咬唇/喘）？
- [ ] 是否无高风险词、无否定式镜头词？
- [ ] 结尾是否回到同一姿态、手位、视线、表情、subject size、framing？
- [ ] 是否跑了 ≥2 候选并做了三帧 QC？
