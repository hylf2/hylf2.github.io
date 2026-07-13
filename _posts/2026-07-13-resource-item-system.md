---
layout: post
title: "资源与物品系统"
date: 2026-07-13
categories: [core-mechanics]
---

## 物品分类

```typescript
enum ItemCategory {
  RESOURCE = 'resource',     // 原材料：木材、石头、铁矿
  FOOD = 'food',             // 食物：浆果、生肉、熟肉
  TOOL = 'tool',             // 工具：斧头、镐、铲子
  WEAPON = 'weapon',         // 武器：剑、矛、弓
  ARMOR = 'armor',           // 防具：头盔、胸甲、腿甲
  CONSUMABLE = 'consumable', // 消耗品：绷带、药水、火把
  MATERIAL = 'material',     // 合成材料：皮革、布料、绳索
  QUEST = 'quest',           // 关键物品：遗迹碎片、Boss信物
}

enum ItemRarity {
  COMMON = 'common',       // 白色 - 60% 基础掉落率
  UNCOMMON = 'uncommon',   // 绿色 - 25%
  RARE = 'rare',           // 蓝色 - 10%
  EPIC = 'epic',           // 紫色 - 4%
  LEGENDARY = 'legendary', // 金色 - 1%
}
```

## 背包系统

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `INVENTORY_SLOTS` | 20 | 初始背包格数 |
| `INVENTORY_MAX_SLOTS` | 40 | 最大背包格数 |
| `STACK_SIZE_RESOURCE` | 99 | 原材料堆叠上限 |
| `STACK_SIZE_FOOD` | 20 | 食物堆叠上限 |
| `STACK_SIZE_CONSUMABLE` | 10 | 消耗品堆叠上限 |
| `STACK_SIZE_TOOL` | 1 | 工具不可堆叠 |
| `STACK_SIZE_WEAPON` | 1 | 武器不可堆叠 |

## 食物保鲜系统

```typescript
interface FoodItem {
  id: string;
  name: string;
  category: ItemCategory.FOOD;
  hunger_restore: number;
  hp_restore: number;
  sanity_restore: number;
  spoil_time: number;         // 腐烂时间(游戏秒)，-1 表示永不腐烂
  current_freshness: number;  // 当前新鲜度 0-1
  raw_effects?: StatusEffect[];
  cooked_id?: string;
  is_spoiled: boolean;
}
```

**腐烂规则**：
- 生肉：2 天（游戏时间）后腐烂
- 浆果：3 天后腐烂
- 熟食：5 天后腐烂
- 干肉/腌制品：15 天后腐烂
- 腐烂食物食用后：Hunger +10，Sanity -15，概率获得「食物中毒」
