---
layout: post
title: "难度与平衡"
date: 2026-07-13
categories: [systems-dynamics]
---

## 区域危险等级

| 等级 | 颜色 | 距出生点 | 敌人数 | 敌人等级 | 资源品质 |
|------|------|---------|--------|---------|---------|
| 1 安全 | 🟢 绿 | 0-50格 | 极少 | Lv.1-3 | 普通 |
| 2 低危 | 🟡 黄 | 50-120格 | 少量 | Lv.3-6 | 普通+ |
| 3 中危 | 🟠 橙 | 120-200格 | 中等 | Lv.6-10 | 优良 |
| 4 高危 | 🔴 红 | 200-300格 | 大量 | Lv.10-15 | 稀有 |
| 5 致命 | ⚫ 黑 | 300格+ | 密集 | Lv.15+ | 史诗 |

## 动态难度调整（DDA）

```typescript
interface DynamicDifficulty {
  recent_deaths: number;
  recent_kills: number;
  time_since_last_hit: number;
  hp_average: number;
  
  adjustments: {
    mercy_scaling: { threshold: 3, reduction: 0.1, max_reduction: 0.3 },
    challenge_scaling: { threshold: 10, increase: 0.05, max_increase: 0.2 },
    mercy_drops: { hp_threshold: 25, drop_boost: 0.3 },
  };
}
```

## 敌人等级与属性缩放

```typescript
interface EnemyScaling {
  level: number;
  hp_multiplier: 1.0 + (level - 1) * 0.15;
  atk_multiplier: 1.0 + (level - 1) * 0.12;
  def_multiplier: 1.0 + (level - 1) * 0.10;
  xp_reward: level * 10;
  loot_quality: level;
}
```
