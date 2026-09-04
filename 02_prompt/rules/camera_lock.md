# 🔒 镜头锁定规范（最高优先级）

AI视频生成器容易忽略此指令，**每个prompt必须遵守**：

## 英文版（放在 `integrated_multimodal_description` 的第一句）

```
Locked-off camera, tripod-mounted, completely static shot, zero camera movement,
no zoom, no pan, no tilt, no dolly, no tracking, no handheld, fixed framing
throughout, camera does not move at all
```

## 中文版（放在中文动作描述的第一句）

```
固定机位，三脚架拍摄，完全静止镜头，零机位移动，
不推近不拉远不摇移不跟拍不手持，画框全程固定，镜头完全不动
```

## 铁律

1. H3 FL2VA prompt 的第一行必须先写 Picture 1 / Picture 2 对齐说明。
2. 镜头锁定指令放在 `integrated_multimodal_description` 的第一句。
3. 结尾回到 Picture 2 时再次写明 subject size 和 framing 不变。
4. 禁止任何形式的镜头运动。
5. 只有角色和虚拟礼物特效在动，镜头不动。
