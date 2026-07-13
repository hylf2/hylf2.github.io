---
layout: post
title: "HUD设计"
date: 2026-07-13
categories: [ui-ux]
---

## HUD 布局（PC 端）

```
┌─────────────────────────────────────────────────┐
│ [HP条] [饥饿条] [精神条]              [小地图] │
│ [耐力条]                                        │
│                                                 │
│                  游戏画面                        │
│                                                 │
│ [快捷栏 1][2][3][4][5]      [装备图标][灯笼状态]│
└─────────────────────────────────────────────────┘
```

## 属性条设计

| 属性条 | 位置 | 尺寸 | 颜色 | 动画效果 |
|--------|------|------|------|---------|
| HP | 左上 #1 | 150×12px | 红 #e94560 | 受伤时闪白 |
| 饥饿 | 左上 #2 | 150×12px | 橙 #ff9f43 | 低值时闪烁 |
| 精神 | 左上 #3 | 150×12px | 紫 #a55eea | 低值时扭曲波纹 |
| 耐力 | 左上 #4 | 120×8px | 绿 #4ecca3 | 耗尽时变灰 |

## 快捷栏设计

```typescript
interface HotbarConfig {
  slot_count: 5;
  position: 'bottom_center';
  select_method: { pc: 'number_keys_1_5'; mobile: 'tap_slot' };
  allowed_types: ['consumable', 'tool', 'weapon'];
  slot_size: { w: 48, h: 48 };
  selected_highlight: '#ffd700';
  show_count: true;
  show_durability: true;
  show_cooldown: true;
}
```

## 小地图设计

**设计理念**：没有传统小地图（符合"未知恐惧"设计支柱），取而代之的是：

| 功能 | 实现方式 |
|------|---------|
| 罗盘 | 右上角简易罗盘，显示东南西北 |
| 灯笼状态 | 灯笼图标显示当前光照强度 |
| 时间指示 | 日月图标显示当前时间 |
| 天气指示 | 简易天气图标 |

## HUD 动态调整

```typescript
interface HUDState {
  in_combat: { show_stamina: true; show_enemy_hp: true; show_quickbar: true; };
  crafting: { show_recipe_list: true; show_ingredient_status: true; dim_background: true; };
  low_sanity: { distort_hud: true; false_warnings: true; flicker: true; };
}
```
