---
layout: post
title: "色彩与光影设计"
date: 2026-07-13
categories: [aesthetics]
---

## 主色调板

| 用途 | 色值 | 说明 |
|------|------|------|
| 主色-暗影 | `#1a1a2e` | 背景/黑暗基调 |
| 主色-深蓝 | `#16213e` | 夜空/深海 |
| 主色-靛青 | `#0f3460` | 环境色 |
| 强调色-赤红 | `#e94560` | 危险/HP/警告 |
| 强调色-金 | `#ffd700` | 稀有/成就/UI高亮 |
| 强调色-翠绿 | `#4ecca3` | 安全/恢复/正面 |

## 生态群落色彩

| 群落 | 地面主色 | 植被色 | 天空色 | 氛围色 |
|------|---------|--------|--------|--------|
| 森林 | #3d5c3a | #2d6e2d | #4a6741 | 神秘绿 |
| 草原 | #7a8c4e | #5e8c3a | #87a96b | 温暖黄绿 |
| 荒漠 | #c4a35a | #8b7d3c | #d4a843 | 燥热橙 |
| 冻原 | #b8c6d4 | #6b8f9e | #8faabe | 寒冷蓝白 |
| 丛林 | #2d4a2d | #1a6e1a | #3d6b3d | 浓郁深绿 |
| 湿地 | #4a5c4a | #3d5e3d | #5a7a6a | 潮湿灰绿 |
| 暗影裂谷 | #1a0a2e | #2d1a4e | #0a0a1e | 诡异紫黑 |

## 昼夜光影系统

```typescript
interface LightingSystem {
  ambient_light: {
    dawn:  { color: '#ff9966', intensity: 0.4 };
    day:   { color: '#fff5e6', intensity: 1.0 };
    dusk:  { color: '#ff6633', intensity: 0.5 };
    night: { color: '#1a1a3e', intensity: 0.15 };
  };
  point_light: {
    falloff_type: 'quadratic';
    max_radius: 5;
    flicker: true;
    flicker_intensity: 0.1;
  };
  darkness: {
    visibility_radius: 2;
    fog_of_war: true;
    vignette_intensity: 0.6;
  };
}
```

## 光源层级

| 光源 | 半径(格) | 颜色 | 精神值效果 |
|------|---------|------|-----------|
| 灯笼(基础) | 3 | 暖黄 #FFE4B5 | +0.5/s |
| 灯笼(升级) | 5 | 明亮黄 #FFD700 | +1.0/s |
| 火把 | 4 | 橙红 #FF8C00 | +0.8/s |
| 篝火 | 5 | 暖橙 #FF6600 | +2.0/s |
| 月光(满月) | 全局 | 冷蓝 #6688BB | 0 |
| 发光蘑菇 | 2 | 荧光绿 #66FF66 | 0 |
| 暗影裂谷 | -2(吸收) | 深紫 #220044 | -3.0/s |
