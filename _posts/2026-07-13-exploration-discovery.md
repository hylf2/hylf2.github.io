---
layout: post
title: "探索与发现系统"
date: 2026-07-13
categories: [systems-dynamics]
---

## 地图迷雾

- 初始地图完全被迷雾覆盖，无小地图
- 玩家移动时逐步揭示迷雾（揭示半径：5格）
- 迷雾中的区域只显示大致轮廓
- 迷雾已揭示区域在死亡后重置

## 地标系统

```typescript
interface Landmark {
  id: string;
  name: string;
  type: 'natural' | 'structure' | 'ruin' | 'camp' | 'boss';
  position: { x: number; y: number };
  biome: string;
  discover_radius: number;
  discovered: boolean;
  on_discover: {
    lore_entry?: string;
    map_marker?: boolean;
    rewards?: Array<{ item_id: string; count: number }>;
  };
}
```

## 随机事件系统

```typescript
interface RandomEvent {
  id: string;
  name: string;
  conditions: {
    biome?: string[];
    time_of_day?: string[];
    weather?: string[];
    min_day?: number;
    min_sanity?: number;
  };
  weight: number;
  cooldown: number;
  type: 'encounter' | 'discovery' | 'hazard' | 'blessing';
  content: any;
}
```

**原型随机事件**：

| 事件 | 类型 | 条件 | 描述 |
|------|------|------|------|
| 迷路旅人 | 遭遇 | 白天, 森林/草原 | 发现一个已死的旅人 |
| 灵光一闪 | 祝福 | 精神值>60 | 揭示附近资源点 |
| 暗影低语 | 危险 | 夜晚, 精神值<40 | 引导走向危险区域 |
| 地下涌泉 | 发现 | 任意 | 发现临时水源 |
| 兽群迁徙 | 遭遇 | 春季/秋季, 草原 | 大量食草动物经过 |
| 远古记忆 | 发现 | 遗迹附近 | 触发叙事过场 |
