---
layout: post
title: "死亡与Roguelike机制"
date: 2026-07-13
categories: [core-mechanics]
---

## 永久死亡规则

当玩家 HP 归零时：
1. 播放死亡动画
2. 显示死亡统计面板（存活时间、击杀数、探索区域、死亡原因）
3. 当前世界存档删除
4. 玩家回到主菜单

**死亡统计面板**：

```typescript
interface DeathReport {
  causeOfDeath: string;       // "被暗影狼撕咬致死"
  survivalTime: number;       // 存活秒数
  distanceTraveled: number;   // 移动总距离
  creaturesKilled: number;    // 击杀生物数
  itemsCollected: number;     // 采集物品数
  biomesExplored: string[];   // 探索过的生态群落
  bossesDefeated: string[];   // 击败的Boss
  furthestFromSpawn: number;  // 最远离出生点距离
  dayReached: number;         // 存活到的天数
  epitaph: string;            // 系统生成的墓志铭
}
```

## 遗产系统（Legacy）

虽然角色永久死亡，但玩家可积累「遗产点数」，用于下次游戏的微小优势。

| 遗产 | 解锁条件 | 效果 | 点数消耗 |
|------|---------|------|---------|
| 起始背包扩展 | 存活 3 天 | 初始背包 +5 格 | 50 |
| 基础地图知识 | 探索 3 个群落 | 初始显示周围 3x3 区域 | 80 |
| 生存直觉 | 存活 7 天 | 低精神值时幻觉出现概率 -20% | 120 |
| 铁胃 | 食用 50 种食物 | 生食负面效果减半 | 60 |
| 战斗记忆 | 击杀 100 个敌人 | 初始武器耐久 +20% | 100 |
| 暗影亲和 | 击败暗影领主 | 暗元素抗性 +15% | 200 |

**遗产点数获取**：

```
遗产点数 = floor(survivalTime / 60) * 2
         + creaturesKilled * 3
         + bossesDefeated * 50
         + biomesExplored * 20
         + itemsCollected
```

## 随机种子与难度递进

每次新游戏使用随机种子生成世界，但保留「难度递进」机制：

```typescript
interface DifficultyScaling {
  deathCount: number;
  enemyHpScale: 1.0 + (deathCount * 0.02);     // 每次 +2%，上限 1.5
  enemyDmgScale: 1.0 + (deathCount * 0.015);    // 每次 +1.5%，上限 1.3
  resourceScarcity: 1.0 - (deathCount * 0.01);   // 每次 -1%，下限 0.8
  sanityDecayScale: 1.0 + (deathCount * 0.01);   // 每次 +1%，上限 1.3
  maxScale: { hp: 1.5, dmg: 1.3, scarcity: 0.8, sanity: 1.3 };
}
```
