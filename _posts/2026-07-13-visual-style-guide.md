---
layout: post
title: "视觉风格指南"
date: 2026-07-13
categories: [aesthetics]
---

## 整体美术方向

**风格定位**：手绘卡通风 × 暗黑奇幻

**参考基调**：
- 线条清晰、形状夸张（卡通感）
- 色彩饱和度偏低（压抑感）
- 大量使用阴影和剪影效果（暗黑感）
- 角色和生物设计夸张变形（非写实）

## 2.5D 等轴测视角规范

```
视角参数：
- 摄像机角度: 45° 俯视角（标准等轴测）
- 缩放范围: 0.8x ~ 2.0x
- 默认缩放: 1.0x
- Tile 大小: 64×32 像素（菱形 tile）
```

**绘制层次**（从底到顶）：

```
Layer 7: 天气粒子（雨/雪/雾）
Layer 6: UI 叠加层（伤害数字、状态图标）
Layer 5: 高空物体（树冠、飞鸟）
Layer 4: 角色/生物（按 Y 轴排序）
Layer 3: 中层物体（灌木、石头、建筑）
Layer 2: 地面装饰（草地纹理、水洼）
Layer 1: 地形基底（地面 tile）
Layer 0: 地下层（洞穴、矿石）
```

## 角色美术规范

```typescript
interface CharacterArtSpec {
  sprite_size: { w: 64, h: 96 };
  facing: '4_direction';
  animations: Array<{
    name: string;
    frame_count: number;
    fps: number;
    loop: boolean;
  }>;
  outline_color: '#1a1a2e';
  outline_width: 2;
  shadow_type: 'blob';
}
```

## 原型占位符美术

| 元素 | 占位符方案 | 颜色 |
|------|-----------|------|
| 玩家角色 | 蓝色方块 + 白色三角形头 | #4488CC |
| 被动生物 | 绿色圆形 | #44AA44 |
| 攻击性生物 | 红色三角形 | #CC4444 |
| Boss | 大号紫色五角星 | #AA44CC |
| 树木 | 绿色三角形 + 棕色矩形 | #338833 |
| 石头 | 灰色圆形 | #888888 |
| 水源 | 蓝色半透明面 | #4488FF66 |
| 建筑 | 棕色矩形 + 屋顶三角形 | #886644 |
