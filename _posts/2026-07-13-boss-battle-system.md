---
layout: post
title: "Boss战系统"
date: 2026-07-13
categories: [systems-dynamics]
---

## Boss设计原则

1. **多阶段战斗**：每个 Boss 至少 2-3 个阶段
2. **环境互动**：战斗场地有可利用的环境元素
3. **可学习的模式**：Boss 攻击有可识别的前摇动画
4. **资源消耗战**：Boss 战设计为持久战
5. **唯一性**：每个 Boss 在世界中只存在一个

## Boss属性模板

```typescript
interface Boss {
  id: string;
  name: string;
  biome: string;
  level: number;
  max_hp: number;
  attack: number;
  defense: number;
  element: string;
  phases: BossPhase[];
  guaranteed_loot: Array<{ item_id: string; count: number }>;
  random_loot_pool: Array<{ item_id: string; weight: number; count: number }>;
  respawn_time: number;
  arena_id: string;
  lore_id: string;
}
```

## 原型Boss：暗影领主（Shadow Lord）

- 位置：暗影裂谷深处
- 等级：Lv.15
- 元素：暗🌑
- HP：2500

**阶段设计**：

| 阶段 | HP阈值 | 新增机制 | 环境变化 |
|------|--------|---------|---------|
| 1 (100-60%) | — | 爪击、暗影弹、瞬移 | 正常 |
| 2 (60-30%) | 60% | 召唤暗影爪牙、暗影冲击波 | 暗影迷雾笼罩 |
| 3 (30-0%) | 30% | 暗影吞噬(秒杀技)、狂暴化 | 场地缩小，落石 |

## 其他Boss概览

| Boss | 群落 | 等级 | 元素 | 核心机制 |
|------|------|------|------|---------|
| 冰霜巨人 | 冻原 | Lv.10 | 冰❄️ | 冰冻地面限制移动 |
| 丛林巨蟒 | 丛林 | Lv.8 | 毒☠️ | 毒雾扩散，缠绕 |
| 沙漠巨蝎 | 荒漠 | Lv.12 | 火🔥 | 沙暴遮蔽，钻地突袭 |
| 古树精 | 森林 | Lv.6 | — | 召唤树根束缚 |
| 世界之树守卫 | 世界之树 | Lv.20 | 全 | 最终Boss |
