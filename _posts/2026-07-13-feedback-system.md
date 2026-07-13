---
layout: post
title: "反馈系统"
date: 2026-07-13
categories: [ui-ux]
---

## 视觉反馈

| 事件 | 视觉反馈 | 持续时间 |
|------|---------|---------|
| 受伤 | 屏幕边缘红色闪烁 | 0.3s |
| 暴击 | 放大金色伤害数字 + 屏幕微震 | 0.2s |
| 死亡 | 灰度滤镜 + 慢动作 | 2s → 死亡面板 |
| 拾取物品 | 物品飞向背包动画 + 名称飘字 | 0.5s |
| 合成成功 | 闪光效果 + 物品弹出 | 0.8s |
| 精神值低 | 屏幕边缘扭曲 + 色彩偏移 | 持续 |
| 发现地标 | 标题文字淡入 + 光效 | 3s |
| Boss出现 | 屏幕震动 + Boss名称显示 | 2s |

## 音效反馈

| 事件 | 音效 | 音量 |
|------|------|------|
| HP < 25% | 心跳声 | 随HP降低增大 |
| 饥饿 < 15% | 胃鸣声 | 中 |
| 精神值 < 25% | 低语声 | 随值降低增大 |
| 敌人在附近 | 紧张弦乐 | 随距离增大 |
| 夜晚降临 | 黄昏号角 | 中 |

## 触觉反馈（小程序）

```typescript
interface HapticFeedback {
  events: {
    attack_hit:    { type: 'short', intensity: 'medium' };
    player_hurt:   { type: 'short', intensity: 'heavy' };
    player_death:  { type: 'long',  intensity: 'heavy' };
    item_pickup:   { type: 'short', intensity: 'light' };
    craft_success: { type: 'medium', intensity: 'medium' };
    boss_appear:   { type: 'long',  intensity: 'heavy' };
    low_hp:        { type: 'short', intensity: 'light', interval: 1.0 };
  };
}
```
