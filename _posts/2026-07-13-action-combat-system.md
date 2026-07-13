---
layout: post
title: "动作战斗系统"
date: 2026-07-13
categories: [core-mechanics]
---

## 基础战斗动作

| 动作 | PC操作 | 小程序操作 | 冷却时间 | 说明 |
|------|--------|-----------|---------|------|
| 普通攻击 | 鼠标左键 | 点击敌人方向 | 0.4s | 基础伤害，可连击（最多3段） |
| 重攻击 | 鼠标右键 | 长按攻击 | 1.2s | 高伤害，有蓄力时间 |
| 闪避/翻滚 | Space | 滑动 | 0.8s | 无敌帧 0.3s，消耗耐力 |
| 格挡 | Q（按住） | 双指按住 | 持续 | 减少 70% 伤害，消耗耐力 |
| 使用道具 | 1-5 数字键 | 快捷栏点击 | 视道具 | 使用背包中的道具 |

## 耐力系统（Stamina）

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `MAX_STAMINA` | 100 | 耐力上限 |
| `STAMINA_REGEN` | 15/s | 静止时回复率 |
| `STAMINA_REGEN_MOVING` | 8/s | 移动时回复率 |
| `STAMINA_COST_ATTACK` | 10 | 普通攻击消耗 |
| `STAMINA_COST_HEAVY` | 25 | 重攻击消耗 |
| `STAMINA_COST_DODGE` | 20 | 闪避消耗 |
| `STAMINA_COST_BLOCK` | 5/次 | 格挡受击消耗 |
| `STAMINA_COST_RUN` | 8/s | 奔跑消耗 |

**耐力耗尽**：无法攻击、闪避、格挡，移动速度降低 30%，持续 2 秒后才开始回复。

## 伤害计算公式

```typescript
// 基础伤害
baseDamage = weapon.base_atk * weapon.multiplier + player.bonus_atk;

// 防御减伤
damageReduction = target.defense / (target.defense + 100); // 0~1 之间

// 暴击
isCrit = Math.random() < player.crit_rate; // 默认 5%
critMultiplier = isCrit ? player.crit_mult : 1.0; // 默认 1.5x

// 最终伤害
finalDamage = baseDamage * (1 - damageReduction) * critMultiplier * elementBonus;
```

## 元素克制系统

| 攻击\目标 | 火🔥 | 冰❄️ | 雷⚡ | 毒☠️ | 暗🌑 |
|-----------|------|------|------|------|------|
| 火🔥 | 1.0 | 1.5 | 0.8 | 1.0 | 1.2 |
| 冰❄️ | 0.8 | 1.0 | 1.0 | 1.2 | 1.0 |
| 雷⚡ | 1.2 | 1.0 | 1.0 | 0.8 | 1.0 |
| 毒☠️ | 1.0 | 0.8 | 1.2 | 1.0 | 1.5 |
| 暗🌑 | 0.8 | 1.0 | 1.0 | 1.0 | 1.0 |

## 武器系统

```typescript
interface Weapon {
  id: string;
  name: string;
  type: 'melee' | 'ranged' | 'magic';
  element: 'fire' | 'ice' | 'thunder' | 'poison' | 'dark' | 'none';
  base_atk: number;
  multiplier: number;
  attack_speed: number;
  range: number;
  max_durability: number;
  current_durability: number;
  durability_loss_per_hit: number;
  on_hit_effects?: StatusEffect[];
  combo_sequence: Array<{
    damage_mult: number;
    anim_id: string;
    duration: number;
  }>;
}
```

## 敌人受击反馈

| 效果 | 触发条件 | 表现 |
|------|---------|------|
| 击退 | 普通攻击 | 敌人后退 0.5 格 |
| 硬直 | 重攻击 | 敌人 0.5s 无法行动 |
| 击飞 | 特定武器/技能 | 敌人被击飞 1 秒 |
| 元素异常 | 元素武器命中 | 附加对应元素状态 |

**元素异常状态**：

| 元素 | 异常状态 | 效果 |
|------|---------|------|
| 火🔥 | 燃烧 | 持续 5s，每秒 3 点伤害 |
| 冰❄️ | 冰冻 | 减速 50%，持续 3s |
| 雷⚡ | 麻痹 | 无法行动 1.5s |
| 毒☠️ | 中毒 | 持续 8s，每秒 2 点伤害，减速 20% |
| 暗🌑 | 恐惧 | 目标混乱 2s |
