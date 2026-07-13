---
layout: post
title: "生态与AI系统"
date: 2026-07-13
categories: [systems-dynamics]
---

## 生物行为树架构

所有生物使用统一的行为树（Behavior Tree）框架驱动：

```typescript
interface BehaviorTree {
  root: BehaviorNode;
  blackboard: Record<string, any>;
}

type BehaviorNode = 
  | { type: 'selector'; children: BehaviorNode[] }
  | { type: 'sequence'; children: BehaviorNode[] }
  | { type: 'condition'; check: string }
  | { type: 'action'; action: string; params?: any }
  | { type: 'decorator'; child: BehaviorNode; mod: string };
```

## 生物AI模板

**被动生物（食草动物）**：
```
Root [Selector]
  ├─ [Sequence] 逃跑
  │   ├─ [Condition] 感知到威胁（距离 < 感知范围）
  │   └─ [Action] 远离威胁（速度 = 最大速度）
  ├─ [Sequence] 觅食
  │   ├─ [Condition] 饥饿值 < 阈值
  │   ├─ [Action] 搜索食物（范围内）
  │   └─ [Action] 进食
  └─ [Action] 闲逛/休息
```

**攻击性生物（掠食者）**：
```
Root [Selector]
  ├─ [Sequence] 战斗
  │   ├─ [Condition] 目标在攻击范围内
  │   ├─ [Condition] 自身 HP > 逃跑阈值
  │   └─ [Action] 攻击
  ├─ [Sequence] 追击
  │   ├─ [Condition] 感知到猎物
  │   └─ [Action] 追击目标
  ├─ [Sequence] 巡逻领地
  └─ [Action] 休息/闲逛
```

## 生物感知系统

| 感知类型 | 基础范围 | 说明 |
|---------|---------|------|
| 视觉 | 8 格 | 正面锥形 120°，受光照/天气影响 |
| 听觉 | 12 格 | 全方向，受噪音等级影响 |
| 嗅觉 | 16 格 | 追踪血迹/食物气味，受风向影响 |

## 食物链与生态系统

```
顶级掠食者 (Boss级)
    ↑ 捕食
大型掠食者 (暗影狼, 黑熊)
    ↑ 捕食
小型掠食者 (狐狸, 蛇)
    ↑ 捕食
食草动物 (兔子, 鹿, 野牛)
    ↑ 食用
植物 (草, 浆果, 蘑菇)
```

## 生物群落分布

| 群落 | 被动生物 | 攻击性生物 | 特殊生物 |
|------|---------|-----------|---------|
| 森林 | 兔子, 鹿, 松鼠 | 暗影狼, 蜘蛛 | 古树精 |
| 草原 | 野牛, 羚羊, 田鼠 | 猎豹, 鬣狗群 | 草原之王 |
| 荒漠 | 蜥蜴, 沙鼠 | 沙蝎, 沙虫 | 沙漠巨蝎 |
| 冻原 | 雪兔, 驯鹿 | 冰狼, 雪怪 | 冰霜巨人 |
| 丛林 | 猴子, 鹦鹉 | 毒蛇, 猎豹 | 丛林巨蟒 |
| 湿地 | 青蛙, 水鸟 | 鳄鱼, 水蛭 | 沼泽女巫 |
| 暗影裂谷 | 无 | 影魔, 暗影蝙蝠 | 暗影领主 |
