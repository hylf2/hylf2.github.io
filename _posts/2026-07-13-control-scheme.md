---
layout: post
title: "操控方案"
date: 2026-07-13
categories: [ui-ux]
---

## PC 键鼠操控

| 操作 | 按键 | 说明 |
|------|------|------|
| 移动 | WASD | 8方向移动 |
| 奔跑 | Shift（按住） | 消耗耐力 |
| 蹲伏 | C | 降低被发现概率 |
| 普通攻击 | 鼠标左键 | 向鼠标方向攻击 |
| 重攻击 | 鼠标右键 | 蓄力攻击 |
| 闪避 | Space | 向移动方向翻滚 |
| 格挡 | Q（按住） | 减少受伤 |
| 交互/拾取 | E | 与物体交互 |
| 使用道具 | 1-5 | 快捷栏选择 |
| 背包 | Tab / B | 打开背包 |
| 暂停 | Esc | 暂停菜单 |

## 微信小程序触屏操控

| 操作 | 手势 | 说明 |
|------|------|------|
| 移动 | 左侧虚拟摇杆 | 8方向移动 |
| 奔跑 | 摇杆推到最外圈 | 消耗耐力 |
| 普通攻击 | 右侧攻击按钮 | 点击攻击 |
| 重攻击 | 长按攻击按钮 | 蓄力后释放 |
| 闪避 | 右侧闪避按钮 | 向移动方向翻滚 |
| 格挡 | 右侧格挡按钮（按住） | 减少受伤 |
| 交互 | 交互按钮 | 靠近可交互物时显示 |

## 虚拟摇杆规范（小程序）

```typescript
interface VirtualJoystick {
  position: 'bottom_left';
  outer_radius: 60;
  inner_radius: 25;
  opacity: 0.5;
  opacity_active: 0.8;
  dead_zone: 0.15;
  run_threshold: 0.85;
  auto_hide: true;
  touch_area: 'left_half';
}
```
