---
layout: post
title: "合成与建造系统"
date: 2026-07-13
categories: [core-mechanics]
---

## 合成系统

```typescript
interface CraftingRecipe {
  id: string;
  name: string;
  station: 'hand' | 'workbench' | 'forge' | 'alchemy' | 'cooking';
  ingredients: Array<{ item_id: string; count: number }>;
  output: { item_id: string; count: number };
  craft_time: number;
  unlock_condition?: string;
}
```

### 合成站层级

| 合成站 | 解锁方式 | 可合成类型 |
|--------|---------|-----------|
| 徒手合成 | 初始可用 | 基础工具（石斧、火把）、绳索 |
| 工作台 | 合成工作台放置 | 武器、防具、高级工具 |
| 锻造炉 | 合成锻造炉放置 | 金属武器、金属防具 |
| 炼金台 | 探索遗迹获取 | 药水、特殊消耗品 |
| 烹饪锅 | 合成烹饪锅放置 | 熟食、特殊料理 |

## 建造系统

```typescript
interface Building {
  id: string;
  name: string;
  type: 'shelter' | 'storage' | 'production' | 'defense' | 'decoration';
  build_time: number;
  materials: Array<{ item_id: string; count: number }>;
  requires_station?: string;
  max_hp: number;
  footprint: { w: number; h: number };
  effects: BuildingEffect[];
}
```

### 原型建筑列表

| 建筑 | 材料 | HP | 效果 |
|------|------|-----|------|
| 篝火 | 木材x5, 石头x3 | 50 | 光源(5格), 温暖(3格), Sanity+2/s(3格) |
| 简易帐篷 | 木材x10, 布料x5 | 100 | 睡眠点, 轻度遮蔽 |
| 储物箱 | 木材x8 | 80 | 额外20格存储 |
| 工作台 | 木材x15, 石头x5 | 150 | 解锁工作台合成 |
| 木栅栏 | 木材x3 | 60 | 阻挡移动，可被破坏 |
| 石墙 | 石头x8 | 200 | 高级防御 |
| 火把架 | 木材x2, 树脂x1 | 30 | 光源(4格) |
| 烹饪锅 | 石头x10, 铁矿x3 | 120 | 解锁烹饪合成 |
