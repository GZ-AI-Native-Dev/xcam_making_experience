---
name: spicycam-complex-action-latest
description: >
  Complex action (1-3 min long interaction videos) dual-route skill.
  Route A: Wan 3.0 i2v chained segments via mulerouter (4 segments, K1-K5
  keyframe chain). Route B: MiniMax H3 full modes — I2VA / FL2VA / L2VA /
  Ref2VA multi-reference, single-shot or H3-chained. Six standard positions:
  handjob, blowjob, paizuri, cowgirl, doggystyle, selftouch.
---

# Complex Action Skill（双路线）

> Complex action 是 1-3 分钟的长互动视频，从角色已有的 private_live 锚点首帧出发，视频内脱衣变裸体后进行性行为。
> 六大位姿：handjob / blowjob / paizuri / cowgirl / doggystyle / selftouch。
> **两条路线可选**：
> - **路线 A**：mulerouter Wan 3.0 **i2v 链式**（分段 + 提取尾帧接下一段）——60-80 秒四段结构的主力。
> - **路线 B**：MiniMax H3 全模式（T2VA / I2VA / FL2VA / L2VA / Ref2VA 多图参考）——4-15 秒单段，或 H3 逐段链式。
> 官方 H3 提示词完整写法（镜头词汇表、对白格式、音景规则）见 `../references/h3_official_base_modes.md` 和 `../references/h3_official_ref_mode.md`。

## 路线选择

| 场景 | 走哪条 |
|---|---|
| 60-80 秒完整四段（脱衣→就位→高潮→收尾），关键帧全控 | **路线 A** Wan 链式 |
| 4-15 秒单段复杂动作（一个位姿的一段动作） | **路线 B** MiniMax（I2VA/FL2VA） |
| 只有收尾态图、想倒推出开头 | **路线 B** MiniMax L2VA |
| 有多张参考图（锚点 + 脸特写 + 位姿参考 + 音色参考），要强一致性 | **路线 B** MiniMax Ref2VA |
| 无图、纯文字探索动作方案（试验用，不上线） | **路线 B** MiniMax T2VA |
| 想要 H3 原生音频（喘/音景随动作同步） | **路线 B**（H3 带 A 的模式都出声） |

混合也允许：四段里个别段用 H3 FL2VA 重跑，其余段保留 Wan。

## 通用规则（两路线都适用）

1. **同首帧**：每条 complex action 的 K1 就是角色已有的 private_live 锚点首帧（穿内衣/比基尼那张），不另出。
2. **脱衣先行**：第一段（seg1）是脱衣过渡，从穿衣锚点脱到全裸；第二段起才是性行为。
3. **关键帧链**：5 个关键帧 K1-K5，相邻段共享端点帧（K2 既是 seg1 尾帧也是 seg2 首帧）。
4. **关键帧先行**：K1 用已有锚点；K2-K5 用图像模型生成（Flux + NSFW LoRA 本地，或图像 API），过脸一致性验收再跑视频。
5. **小样铁律**：每段先跑 2-3 秒低步数/低时长验证 K→K 路径不崩，再放全量。
6. **三候选**：每段跑 candidate / candidate2 / candidate3，段过审才 concat。
7. **脸锁**：每段输出过一遍 FaceDetailer / ReActor 对齐锚点首帧。
8. **伴侣可见度**：涉及伴侣的位姿，伴侣只出现髋部或手，脸永不入画。
9. **镜头固定**：所有段写 `Static shot` / `the camera holds a static shot`，不做推近、拉远、平移、跟拍。
10. **成人虚构角色**：prompt 内统一写 adult fictional woman，不把真实姓名送进模型。

## 四段结构（两路线共用的骨架）

| 段 | 定位 | 内容 |
| --- | --- | --- |
| seg1 脱衣 | K1(穿衣锚点) → K2(全裸) | 脱掉内衣/比基尼，变裸体 |
| seg2 就位 | K2(全裸) → K3(行为就位) | 摆好体位，开始前戏 |
| seg3 高潮 | K3 → K4(行为峰值) | 加速，高潮表情，体位峰值 |
| seg4 收尾 | K4 → K5(收尾态) | 减速，撤出/松手，回到休息态 |

---

# 路线 A：Wan 3.0 i2v 链式（60-80 秒主力）

> 所有段走 mulerouter Wan 3.0 **FLF2V API**（首帧图 + 尾帧图 + 自然语言文本）。

## Wan 段级提示词格式

- 纯自然语言，不使用 H3 三段式字段名。
- 现在时态描述动作路径。
- 每段开头锁一句 `Static shot`。
- 输入 = K{i} 首帧图 + K{i+1} 尾帧图 + 自然语言文本。

格式模板：

```text
Static shot. [动作起点]. [动作过程]. [动作收尾]. [构图约束].
```

## 六大位姿

详见 `six_poses.md`。

| 代号 | 中文 | 成品时长 | 涉及伴侣 | 脱衣重点 |
| --- | --- | --- | --- | --- |
| handjob | 手交 | ~65s | 是（手部） | 全脱 |
| blowjob | 口交 | ~65s | 是（髋部） | 全脱 |
| paizuri | 乳交 | ~65s | 是（髋部） | 全脱，重点上身 |
| cowgirl | 女上位 | ~80s | 是（平躺） | 全脱 |
| doggystyle | 后入 | ~70s | 是（后方） | 全脱，转身同步 |
| selftouch | 自摸 | ~65s | 否 | 全脱，后仰同步 |

## Wan 关键帧工作流

1. K1 直接用角色已有的 private_live 锚点首帧，不重新生成。
2. K2-K5 用图像模型生成（Flux + NSFW LoRA 本地部署，或 mulerouter 图像 API）。
3. K2 必须是全裸态，与 K1 同脸同发型同场景。
4. K3 是行为就位帧（裸体 + 体位摆好）。
5. K4 是行为峰值帧（表情 + 体位最高点）。
6. K5 是收尾帧（行为结束，休息态，可切回 idle）。
7. K2-K5 过脸一致性验收后才能进入视频段生成。

## 拼接与 QC

- 拼接：ffmpeg concat，段间加 0.5s crossfade 过渡。
- 段级 QC：每段抽 0.5s / 中 / 尾三帧，比人物大小、头位、手位、背景地标、脸一致性。
- 成品 QC：抽首 / 中 / 尾三帧，比角色一致性（脸、发型、场景不漂移）。
- 坏视频如果有学习价值，保留到 rejected/videos 并保存对应 prompt 到 rejected/prompts。

---

# 路线 B：MiniMax H3 模式

> 官方完整写法（首尾帧对齐行、镜头运动词汇表、`(S1)` 说话人、`<d>` 对白、音景规则）见 `../references/h3_official_base_modes.md`（T2VA/I2VA/FL2VA/L2VA）和 `../references/h3_official_ref_mode.md`（Ref2VA 六段结构）。
> H3 单条时长 4-15 秒。复杂动作走 H3 时：**单段短动作**直接用一条；**长视频**按四段骨架逐段链式（seg n 用 FL2VA 输入 K_n + K_{n+1}，和路线 A 相同的 K 链，只是每段换成一次 H3 调用）。

## H3 模式矩阵（复杂动作场景）

| 模式 | 输入 | 复杂动作场景 | 对齐行格式 |
|---|---|---|---|
| T2VA | 无图 | 纯文字试验动作方案，不出成品 | 无对齐行，直接三段 |
| I2VA | K1 一张 | 从锚点向后自然脱衣/动作；自由度高，结尾不回位 | `For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.` |
| FL2VA | K1+K5 两张（或段级 K_n+K_{n+1}） | **loop 回位主力**：强制落回收尾态 | `Picture 1 ... aligns with the 0.00-second mark ... Picture 2 ... aligns with the S.SS-second mark` |
| L2VA | 尾帧一张 | 只有休息态图，倒推开头逐渐收敛到图 | `<Picture 1> (from [Shot N]) aligns with the S.SS-second mark` |
| Ref2VA | 多参考（锚点+脸特写+位姿图+音色） | 长视频强一致性；六段结构输出 | 六段：subject_definitions / summary / retention_analysis / detailed_description / overall_soundscape / non_diegetic_music |

## 路线 B 实例

### 实例 1：I2VA 单图续写（脱衣段，10 秒）

```text
For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.

integrated_multimodal_description: [Shot 1] Live-action, authentic private livestream realism, a single vertical front-facing static shot. The adult fictional woman shown in <Picture 1> begins in the exact pose, framing, lingerie, and room established by the image, and the camera holds a static shot for the full duration, preserving the same lens focal length, subject size, background landmarks, lighting, and layout. She reaches behind her back and unclasps her top, letting it fall away to reveal her breasts, then hooks her thumbs into her bottoms and slides them down her hips, stepping out of them to become fully nude while holding eye contact with the camera. Her face, hair, skin tone, body proportions, and the room background stay consistent with <Picture 1>. She settles standing nude with hands relaxed at her sides.

overall_soundscape: Soft fabric slides as the lingerie drops to the floor, followed by a quiet exhale and faint bare-foot contact against the floor.

non_diegetic_music: N/A
```

### 实例 2：FL2VA 首尾回位（selftouch 单段 loop，10 秒）

```text
How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot 1) aligns with the 10.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic private livestream realism, a single vertical front-facing static shot. The adult fictional woman begins fully nude in the pose and framing established by Picture 1, seated back on her hands with knees slightly parted, and the camera holds a static shot for the full 10 seconds, preserving the same lens focal length, subject size, background landmarks, lighting, and layout. Her free hand slides down her stomach to between her thighs, fingertips pressing in slow circles as her head tilts back, lips parting, back arching off the support; the rhythm speeds until her thighs tremble, then her hand withdraws, she sits up slowly, closes her knees, brushes her hair back, and settles into the exact resting pose, gaze, hand position, subject size, and composition established by Picture 2.

overall_soundscape: Skin-on-skin contact sounds and fabric-free movement, a shaky exhale sliding into a soft moan, and ragged breathing that settles back to calm.

non_diegetic_music: N/A
```

### 实例 3：L2VA 尾帧倒推（只有收尾态图，6 秒）

```text
How the reference pictures align with the target video — <Picture 1> (from [Shot 1]) aligns with the 6.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, authentic private livestream realism, a single vertical front-facing static shot. The same adult fictional woman as in <Picture 1> begins nude and kneeling, breathing still heavy right after climax, one hand resting on her thigh and eyes half-closed. The camera holds a static shot for the full 6 seconds, preserving the same lens focal length, subject size, background landmarks, lighting, and layout. Her breathing gradually slows, she straightens her posture, wipes her fingers on her thigh, brushes her hair back, and her expression softens into a satisfied smile as her pose, hand position, gaze, and framing gradually converge into the exact arrangement established by <Picture 1>.

overall_soundscape: Ragged breathing slowly settles into calm, with faint skin contact as she shifts her weight on the carpet.

non_diegetic_music: N/A
```

### 实例 4：Ref2VA 多图参考（锚点 + 脸特写 + 位姿参考，12 秒）

```text
subject_definitions:
<Subject 1> is the adult fictional woman in <Picture 1>, with her hair, face, skin tone, and body proportions; <Picture 2> is a face close-up of the same woman used to lock facial identity; <Picture 3> is the cowgirl position reference defining the mounted pose and camera-side framing.

summary:
[reference generation + keyframe completion] The target video shows <Subject 1> fully nude, moving from a kneeling mount into a slow riding rhythm, using <Picture 1> as the opening frame anchor, <Picture 2> as facial identity reference, and <Picture 3> as pose reference; her partner is implied at the lower frame edge with only hips visible.

retention_analysis:
<Subject 1> (appears in [Shot 1]): fully_preserved - hair, face, skin tone, and body proportions from <Picture 1> are retained throughout.
<Picture 1> ([Shot 1] first frame): fully_preserved - the video opens exactly on this frame.
<Picture 2> (facial identity): fully_preserved - facial features stay locked to the close-up reference.
<Picture 3> (pose reference): partially_preserved - the mounted pose and framing are followed, adapted to the room from <Picture 1>.

detailed_description:
The target video is in a realistic private-livestream style with warm low lighting.
[Shot 1] The shot begins from <Picture 1>. The adult fictional woman <Subject 1>, fully nude with facial features locked to <Picture 2>, kneels over her partner whose hips are implied at the lower frame edge, face and torso fully visible to the camera. Following the mounted pose defined by <Picture 3>, she lowers her hips slowly until full contact, hands bracing on her own thighs, back arching slightly. She begins a slow riding rhythm, breasts bouncing with each descent, mouth opening, eyes locking with the camera, hips staying within the frame composition of <Picture 3>. The camera holds a static shot throughout, preserving the same lens focal length, subject size, background landmarks, lighting, and layout.

overall_soundscape: Skin-on-skin impact sounds in a slow rhythm, a shaky exhale sliding into a soft moan, and quiet room tone underneath.

non_diegetic_music: N/A
```

## 路线 B 写作要点

1. 单镜头 `[Shot 1]`，不分镜；镜头锁定写 `the camera holds a static shot`（正向锚定）。
2. 人物身份句逐字对应首帧图（脸、发、身体比例、房间），参考图标签（`<Picture 1>`）在对齐行和正文里保持一致。
3. 音景写非人声为主（skin-on-skin、breath、moan），人声台词才用 `<d>`。
4. 总时长必须和请求时长一致（4-15 秒），结尾句重复 `subject size, framing`。
5. 段级链式时：seg n 的 FL2VA 输入 = K_n + K_{n+1}，和路线 A 的 K 链完全相同，可逐段混跑。

---

## 目录约定

```text
complex action/{代号}/
  keyframes/          K1.png(复制自锚点) K2.png K3.png K4.png K5.png
  segments/
    seg1/
      candidate/  candidate2/  candidate3/  accepted/
    seg2/
      candidate/  candidate2/  candidate3/  accepted/
    seg3/
      candidate/  candidate2/  candidate3/  accepted/
    seg4/
      candidate/  candidate2/  candidate3/  accepted/
  concat_output/      {代号}_final.mp4   ← 线上消费对象
  rejected/
    videos/
    prompts/
  qc/
  notes/
  raw_runs/
```

OSS 最终交付路径：

```text
spicycam/{角色名}/complex action/{代号}/concat_output/{代号}_final.mp4
```

## 检查清单

- [ ] 是否选对了路线（四段长视频走 A，单段/多图参考走 B）？
- [ ] K1 是否直接用已有的 private_live 锚点首帧？
- [ ] seg1 是否是脱衣过渡（穿衣 → 全裸）？
- [ ] 路线 A 是否全部 Wan 3.0 via mulerouter；路线 B 是否用对 H3 模式（回位用 FL2VA，倒推用 L2VA，多图用 Ref2VA）？
- [ ] 路线 B 是否带首尾帧对齐行、单镜头、总时长匹配？
- [ ] K2-K5 是否过脸一致性验收后才跑视频？
- [ ] 是否每段先跑小样验证 K→K 路径不崩？
- [ ] 是否每段跑 3 个候选？
- [ ] 是否每段过 FaceDetailer 对齐锚点首帧？
- [ ] 伴侣是否只出现髋部/手，脸不入画？
- [ ] 是否所有段写 `Static shot`，无推近拉远？
- [ ] 段过审后才 concat？
- [ ] 成品 QC 是否抽首/中/尾三帧比角色一致性？
- [ ] 是否避免真实人物冒充、未成年人、非自愿、暴力或违法内容？
