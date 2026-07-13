---
layout: post
title: "存档与数据管理"
date: 2026-07-13
categories: [technical]
---

## 存档结构

```typescript
interface SaveData {
  version: string;
  save_time: number;
  play_time: number;
  seed: number;
  player: {
    position: { x: number; y: number };
    stats: { hp: number; hunger: number; sanity: number; stamina: number };
    inventory: InventoryItem[];
    hotbar: (string | null)[];
    equipment: { weapon: string | null; armor_head: string | null; armor_body: string | null; armor_legs: string | null };
  };
  world: {
    time: number; day: number; season: string; weather: string;
    modified_chunks: Record<string, ChunkModification>;
    buildings: BuildingInstance[];
    defeated_bosses: Array<{ id: string; defeat_time: number }>;
    discovered_fragments: string[];
  };
  stats: { creatures_killed: number; items_collected: number; distance_traveled: number; death_count: number };
}
```

## 自动存档策略

| 触发条件 | 类型 | 说明 |
|---------|------|------|
| 每 5 分钟 | 自动存档 | 定期保存 |
| 进入/离开区域 | 自动存档 | 区域切换时保存 |
| 建造/合成 | 自动存档 | 重要操作后保存 |
| Boss战前/后 | 自动存档 | 关键战斗保存 |
| 死亡时 | 特殊存档 | 保存统计后删除世界存档 |

## 遗产数据存储（永久）

```typescript
interface LegacyData {
  total_legacy_points: number;
  spent_legacy_points: number;
  unlocked_legacies: string[];
  death_history: DeathReport[];
  total_deaths: number;
  best_survival_days: number;
  discovered_items: string[];
  discovered_creatures: string[];
  discovered_biomes: string[];
  discovered_fragments: string[];
}
```
