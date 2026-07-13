---
layout: post
title: "性能预算"
date: 2026-07-13
categories: [technical]
---

## 帧率目标

| 平台 | 目标帧率 | 最低帧率 | 说明 |
|------|---------|---------|------|
| PC (推荐配置) | 60 FPS | 45 FPS | 流畅体验 |
| PC (最低配置) | 30 FPS | 24 FPS | 可玩 |
| 微信小游戏(高端) | 60 FPS | 45 FPS | 流畅 |
| 微信小游戏(中端) | 30 FPS | 24 FPS | 可玩 |
| 微信小游戏(低端) | 30 FPS | 20 FPS | 基础可玩 |

## 内存预算

| 平台 | 总预算 | 纹理 | 音频 | 代码 | 运行时 |
|------|--------|------|------|------|--------|
| PC | 512 MB | 200 MB | 100 MB | 50 MB | 162 MB |
| 小游戏(高端) | 256 MB | 120 MB | 50 MB | 30 MB | 56 MB |
| 小游戏(低端) | 128 MB | 60 MB | 25 MB | 20 MB | 23 MB |

## 加载时间目标

| 场景 | PC | 小游戏 |
|------|-----|--------|
| 首次启动 | < 5s | < 8s |
| 新游戏开始 | < 3s | < 5s |
| 区域切换 | < 1s | < 2s |
| 存档加载 | < 2s | < 4s |

## 性能监控

```typescript
interface PerformanceMonitor {
  metrics: {
    fps: number;
    draw_calls: number;
    memory_used: number;
    chunk_count: number;
    entity_count: number;
  };
  auto_downgrade: {
    fps_threshold: 24;
    check_interval: 5;
    downgrade_steps: [
      'reduce_particles', 'disable_shadows',
      'reduce_lighting', 'reduce_render_scale', 'reduce_chunks',
    ];
  };
}
```
