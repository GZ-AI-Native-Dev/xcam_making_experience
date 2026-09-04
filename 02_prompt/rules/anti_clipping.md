# ⚠️ 防穿透规则 (Anti-Clipping Protocol)

**所有视频生成必须遵守物理边界，防止 AI 视频生成中的常见穿模 bug：**

1. **接触即停止 (Surface Stop)** — 描述手部或物体接触脸部/身体时，必须写出 `stopping at the surface`。例如："Palms pressed flat against cheeks, fingers NOT passing through face."
2. **肢体不交叉 (No Intersecting Limbs)** — 尽量避免手肘穿过胸前，或大腿互相穿插。若必须出现，强调 `distinct volume`。
3. **物品实体感 (Solid Props)** — 礼物或道具接触身体时，强调 `collision`（碰撞），不能穿透身体模型。
