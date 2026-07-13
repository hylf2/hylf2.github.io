---
layout: post
title: "三大核心属性系统 (HP/Hunger/Sanity)"
date: 2026-07-13
categories: [core-mechanics]
---

玩家角色由三个核心属性驱动，它们是所有生存决策的基础。

## 生命值（Health Points, HP）

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `MAX_HP` | 100 | 生命值上限 |
| `HP_REGEN_RATE` | 0 | 自然回复率（不自动回复） |
| `HP_REGEN_WHEN_FED` | 0.5/s | 饥饿值 > 70 时的回复率 |
| `HP_DAMAGE_STARVE` | 2/s | 饥饿值为 0 时的持续伤害 |
| `HP_DAMAGE_INSANE` | 1/s | 精神值为 0 时的自残伤害 |

**消耗来源**：
- 敌人攻击（按伤害公式计算）
- 环境伤害（坠落、溺水、火焰、毒气）
- 饥饿归零后的持续掉血
- 精神归零后的自残

**恢复方式**：
- 食用熟食（需先烹饪）
- 使用绷带/草药等医疗物品
- 睡眠（需在安全区域，会加速时间）
- 饱食度充足时的缓慢自然回复

**状态阈值**：

```
HP > 75  ：正常状态，无视觉变化
HP 50-75 ：轻微受伤，角色行走姿态微变
HP 25-50 ：重伤状态，屏幕边缘泛红，移动速度 -15%
HP 0-25  ：濒死状态，屏幕严重泛红，视野模糊，移动速度 -40%
HP = 0   ：死亡
```

## 饥饿值（Hunger）

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `MAX_HUNGER` | 100 | 饥饿值上限 |
| `HUNGER_DECAY_RATE` | 1.0/60s | 基础消耗率（每秒） |
| `HUNGER_DECAY_RUNNING` | 2.5/60s | 奔跑时的消耗率 |
| `HUNGER_DECAY_COLD` | 1.5/60s | 寒冷环境下的额外消耗 |
| `HUNGER_DECAY_COMBAT` | 2.0/60s | 战斗中的额外消耗 |

**消耗规则**：
- 基础消耗恒定，按游戏内时间计算
- 奔跑、战斗、寒冷环境会叠加额外消耗
- 所有消耗源可叠加

**状态阈值**：

```
Hunger > 70  ：饱腹，HP 缓慢回复
Hunger 40-70 ：正常，无额外效果
Hunger 15-40 ：饥饿，屏幕出现饥饿提示，无法奔跑
Hunger 0-15  ：极度饥饿，视野变窄，无法奔跑
Hunger = 0   ：开始持续掉 HP
```

## 精神值（Sanity）

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `MAX_SANITY` | 100 | 精神值上限 |
| `SANITY_DECAY_NIGHT` | 0.8/60s | 夜间的消耗率 |
| `SANITY_DECAY_DARK` | 1.5/60s | 完全黑暗中的消耗率 |
| `SANITY_DECAY_MONSTER` | 3.0/60s | 靠近怪物时的消耗率 |
| `SANITY_REGEN_LIGHT` | 1.0/60s | 光源附近的回复率 |
| `SANITY_REGEN_FIRE` | 2.0/60s | 篝火旁的回复率 |

**消耗来源**：
- 夜间/黑暗环境
- 靠近敌对生物（越近消耗越快）
- 目击恐怖事件（发现尸体、Boss 出场）
- 食用生肉/腐烂食物
- 长时间不睡觉

**状态阈值与效果**：

```
Sanity > 75  ：清醒，无异常
Sanity 50-75 ：略有不安，偶尔出现虚假音效
Sanity 25-50 ：恐惧，出现幻觉敌人（不造成伤害但会消耗资源）
Sanity 0-25  ：濒临崩溃，幻觉频繁，视野扭曲，影怪开始实体化
Sanity = 0   ：崩溃状态，影怪实体攻击，持续自残掉 HP
```

**幻觉系统设计**：

```typescript
interface HallucinationSystem {
  level: 'none' | 'mild' | 'moderate' | 'severe' | 'critical';
  types: Array<{
    id: string;
    name: string;
    minSanity: number;
    probability: number;
    duration: number;
    isHarmful: boolean;
  }>;
  checkInterval: 5; // 秒
}
```
