---
layout: post
title: "音效与音乐设计"
date: 2026-07-13
categories: [aesthetics]
---

## 音乐风格

**整体风格**：氛围音乐 + 暗色调原声

| 场景 | 风格 | 乐器 | 节奏 |
|------|------|------|------|
| 主菜单 | 空灵、悠远 | 钢琴 + 弦乐 + 风声 | 极慢 |
| 白天探索 | 宁静但带忧郁 | 吉他 + 竖琴 + 自然音 | 慢 |
| 夜间探索 | 紧张、不安 | 低音提琴 + 不协和弦 | 中 |
| 战斗 | 激烈、紧迫 | 打击乐 + 电吉他 + 管弦 | 快 |
| Boss战 | 史诗、压迫 | 交响乐 + 合唱团 + 重金属 | 极快 |
| 低精神值 | 诡异、扭曲 | 反向音效 + 低频嗡鸣 | 不规则 |
| 死亡 | 悲伤、终结 | 大提琴独奏 | 极慢→停止 |

## 动态音乐系统

```typescript
interface DynamicMusic {
  layers: Array<{
    id: string;
    audio_id: string;
    condition: string;
    crossfade_time: number;
    volume: number;
  }>;
  scenes: {
    exploration: { base_layer: 'ambient_pad'; add_layers: ['melody_guitar', 'nature_sounds'] };
    combat: { base_layer: 'combat_drums'; add_layers: ['combat_melody', 'combat_bass'] };
    boss: { base_layer: 'boss_orchestra'; add_layers: ['boss_choir'] };
  };
}
```

## 音效清单

**环境音效**：

| ID | 名称 | 触发条件 | 循环 |
|----|------|---------|------|
| `amb_forest_day` | 森林日间 | 森林群落+白天 | 是 |
| `amb_forest_night` | 森林夜间 | 森林群落+夜晚 | 是 |
| `amb_wind` | 风声 | 冻原/高处 | 是 |
| `amb_water` | 水流声 | 靠近水源 | 是 |
| `amb_cave` | 洞穴回响 | 地下区域 | 是 |
| `amb_shadow` | 暗影低语 | 暗影裂谷 | 是 |

**动作音效**：

| ID | 名称 | 触发条件 |
|----|------|---------|
| `sfx_attack_melee` | 近战攻击 | 普通攻击 |
| `sfx_hit_enemy` | 命中敌人 | 攻击命中 |
| `sfx_hit_player` | 被击中 | 玩家受伤 |
| `sfx_death` | 死亡 | 玩家死亡 |
| `sfx_gather_wood` | 采集木材 | 砍树 |
| `sfx_craft` | 合成完成 | 合成物品 |
