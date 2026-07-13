---
layout: post
title: "新手引导设计"
date: 2026-07-13
categories: [ui-ux]
---

## 设计原则

**核心理念**：没有教程弹窗，没有任务指引。通过精心设计的初始环境让玩家自然学会。

## 渐进式教学流程

```
第一步：苏醒（移动）
  → 角色在一片空地醒来
  → 周围有几棵明显的树和石头
  → 玩家自然尝试移动

第二步：饥饿驱动（采集）
  → 饥饿值从一开始就在消耗
  → 附近有明显的浆果丛
  → 靠近时出现交互提示 "E"

第三步：工具需求（合成）
  → 附近的树木和岩石需要工具才能有效采集
  → 徒手可以捡起树枝和石头
  → 收集到足够材料后，石斧配方高亮

第四步：夜晚恐惧（光源）
  → 第一天故意缩短（约5分钟）
  → 夜晚到来时，黑暗和声音制造恐惧
  → 火把配方在合成界面中明显

第五步：第一次遭遇（战斗）
  → 第二天，一只低级暗影狼出现在营地附近
  → 玩家已有基础武器
  → 通过战斗学会攻击、闪避、格挡
```

## 环境提示系统

```typescript
interface ContextualHint {
  id: string;
  trigger: (state: GameState) => boolean;
  display: {
    type: 'icon_glow' | 'sound_cue' | 'visual_pulse';
    target: string;
    duration: number;
  };
  shown_once: boolean;
}

const HINTS = [
  { id: 'hint_first_gather',
    trigger: (s) => s.time < 60 && s.inventory.length === 0,
    display: { type: 'visual_pulse', target: 'nearest_tree', duration: 3 },
    shown_once: true },
  { id: 'hint_first_night',
    trigger: (s) => s.time_to_night < 60 && !s.has_light_source,
    display: { type: 'icon_glow', target: 'craft_torch_recipe', duration: 5 },
    shown_once: true },
];
```
