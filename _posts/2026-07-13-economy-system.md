---
layout: post
title: "经济系统"
date: 2026-07-13
categories: [systems-dynamics]
---

## 资源供需曲线

资源价值随游戏进程动态变化：

```typescript
interface ResourceValue {
  item_id: string;
  base_value: number;
  supply_factor: number;
  demand_factor: number;
  // current_value = base_value * (1 / supply_factor) * demand_factor
}
```

**稀缺性曲线**（按游戏天数）：

```
Day 1-3:   基础资源丰富，玩家学习基本操作
Day 4-7:   基础资源开始紧缺，需要探索更远区域
Day 8-14:  中级资源成为瓶颈，需要工具和策略
Day 15-21: 高级资源稀缺，需要Boss掉落或遗迹获取
Day 22+:   极度稀缺，考验玩家的所有积累
```

## 交易机制（流浪商人）

每隔 5-8 天，一个流浪商人 NPC 会在玩家营地附近出现：

```typescript
interface WanderingMerchant {
  spawn_interval: { min: 5, max: 8 }; // 天
  stay_duration: 1;                     // 天
  inventory_size: 5;
  price_multiplier: { min: 1.2, max: 2.5 };
  currency: 'gold_nuggets';
}
```
