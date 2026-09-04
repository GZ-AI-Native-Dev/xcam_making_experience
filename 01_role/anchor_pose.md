# 🎯 锚点姿态（Anchor Pose）

所有视频的进出都围绕锚点姿态，这是衔接的基石。**场景根据角色设定而定，不固定为沙发或大床。**

当前 latest 版本的锚点图必须先满足 `01_role/first_frame_standard.md` 的真实高清直播首帧标准：
正面直视镜头、眼睛等高平视、竖版 9:16、从头到膝盖完整可见、人物和背景同样清晰、明亮均匀、画面端正。

---

## Public Live 锚点模板

```
中文：真实手机直播画面，一位成年东亚女性[角色]，[发型描述]
正面坐在/站在[具体直播位置]，身体正对镜头，脸完全正对镜头，双眼直视镜头
镜头高度与眼睛完全平齐，平视视角
穿着[日常/职业相关服装]，整体符合[角色气质]
双手自然放在[膝盖/桌面/核心道具旁]，姿态放松自然
背景是明亮、整洁、真实的[场景]，包含[3-5个清晰背景元素]
人物和背景同样清晰，深景深，不做背景虚化
竖版9:16，人物居中，从头到膝盖完整可见
```

```
English: An adult [role] with [hair description and natural appearance]
Sitting or standing front-facing at [real livestream location], with [3-5 clear background elements] visible
Wearing everyday or profession-appropriate clothing for public livestream
Hands resting naturally in the same visible position as the reference image
Body and face fully facing the camera, eyes looking directly into the camera
Camera at exact eye level, straight-on view, no high angle, no low angle, no side angle
Bright even room lighting, realistic webcam exposure, deep focus
The person and background are equally sharp; no cinematic blur, no portrait mode
Vertical 9:16, centered composition, visible from head to knees
```

## Private Live 锚点模板

```
中文：真实手机直播画面，一位成年东亚女性[角色]，[发型描述]
位于单独的私密直播空间，使用与 public_live 不同的 private_anchor
身体正对镜头，脸完全正对镜头，双眼直视镜头；镜头高度与眼睛完全平齐
服装状态、灯光和坐姿按 private_live 单独设定，不复用公屏锚点
双手自然放在[膝盖/桌面/身体前方稳定位置]，姿态稳定、低幅度
背景是整洁、真实、私密的室内空间，包含[3-5个清晰背景元素]
人物和背景同样清晰，深景深，不做背景虚化
竖版9:16，人物居中，从头到膝盖完整可见
```

```
English: An adult [role] with [hair description and natural appearance]
In a separate private livestream room, using a private_anchor distinct from public_live
Body and face fully facing the camera, eyes looking directly into the camera
Camera at exact eye level, straight-on view, no high angle, no low angle, no side angle
Private mode clothing state, lighting, and pose are defined separately from the public anchor
Hands resting naturally in a stable low-amplitude position
Clean realistic private indoor background with [3-5 clear background elements]
The person and background are equally sharp; no cinematic blur, no portrait mode
Vertical 9:16, centered composition, visible from head to knees
```

**关键：** 所有视频的首帧/尾帧都必须接近同一模式锚点姿态，否则切换会跳帧。角色和场景先参照 `role_system.md` 与 `first_frame_standard.md` 定义，避免所有锚点都变成同一张床和同一类人物。
