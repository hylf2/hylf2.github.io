---
layout: post
title: "时间与天气系统"
date: 2026-07-13
categories: [core-mechanics]
---

## 时间系统

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `DAY_LENGTH_REAL` | 480 | 一天的真实秒数（8分钟） |
| `DAY_PHASE_DAWN` | 0.20 | 黎明占比 |
| `DAY_PHASE_DAY` | 0.35 | 白天占比 |
| `DAY_PHASE_DUSK` | 0.10 | 黄昏占比 |
| `DAY_PHASE_NIGHT` | 0.35 | 夜晚占比 |

**时间流逝效果**：

| 时段 | 光照 | 温度修正 | Sanity 消耗 | 敌人生成 |
|------|------|---------|------------|---------|
| 黎明 | 渐亮 | +5°C | 正常 | 减少 |
| 白天 | 最亮 | 基准 | 正常 | 最少 |
| 黄昏 | 渐暗 | -3°C | 正常 | 增加 |
| 夜晚 | 最暗 | -10°C | +0.8/s | 最多 + 强化 |

## 季节系统

| 季节 | 持续天数 | 温度修正 | 特征 |
|------|---------|---------|------|
| 春 Spring | 20 天 | +5°C | 植物生长加速，花朵盛开 |
| 夏 Summer | 25 天 | +15°C | 食物充足，雷暴频繁 |
| 秋 Autumn | 20 天 | +0°C | 落叶，动物储食，准备冬眠 |
| 冬 Winter | 30 天 | -20°C | 食物稀缺，降雪，部分生物冬眠 |

## 天气系统

```typescript
interface Weather {
  id: string;
  name: string;
  seasons: string[];
  biomes: string[];
  weight: number;
  temperature_mod: number;
  visibility_mod: number;
  sanity_mod: number;
  movement_mod: number;
  effects: string[];
  particle_type?: string;
}
```

**原型天气**：

| 天气 | 季节 | 温度修正 | 能见度 | 特殊效果 |
|------|------|---------|--------|---------|
| 晴朗 | 四季 | 0 | 1.0 | 无 |
| 下雨 | 春/夏/秋 | -5 | 0.7 | 地面湿滑 |
| 降雪 | 冬 | -10 | 0.6 | 寒冷暴露 |
| 浓雾 | 秋/冬 | -3 | 0.3 | 迷失方向 |
| 雷暴 | 夏 | -8 | 0.5 | 雷击、地面湿滑 |
