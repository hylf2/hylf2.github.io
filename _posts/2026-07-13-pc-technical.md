---
layout: post
title: "PC端技术方案"
date: 2026-07-13
categories: [technical]
---

## 渲染方案

```typescript
interface RenderConfig {
  resolution: 'auto';
  design_resolution: { w: 1280, h: 720 };
  fit_mode: 'fit_height';
  camera_type: 'orthographic';
  camera_angle: 45;
  tile_size: 64;
  batching: true;
  atlas_enabled: true;
  max_atlas_size: 2048;
  lighting_enabled: true;
  max_point_lights: 20;
  shadow_map_size: 512;
}
```

## 输入适配（PC）

```typescript
interface InputManager {
  keyboard: {
    key_map: Record<string, string>;
    default_map: {
      'KeyW': 'move_up', 'KeyS': 'move_down',
      'KeyA': 'move_left', 'KeyD': 'move_right',
      'Space': 'dodge', 'ShiftLeft': 'run',
      'KeyQ': 'block', 'KeyE': 'interact',
      'KeyB': 'inventory', 'Escape': 'pause',
    };
  };
  mouse: {
    left_click: 'attack';
    right_click: 'heavy_attack';
    scroll: 'zoom';
  };
}
```

## Windows 打包

| 参数 | 值 |
|------|-----|
| 目标平台 | Windows 10/11 x64 |
| 最低配置 | Intel i3, 4GB RAM, 集成显卡 |
| 推荐配置 | Intel i5, 8GB RAM, 独立显卡 |
| 安装方式 | 绿色免安装版 / 安装包 |
| 预计包体 | 200-500 MB |
