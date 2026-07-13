---
layout: post
title: "微信小程序方案"
date: 2026-07-13
categories: [technical]
---

## 小游戏框架适配

```typescript
import { GameManager } from './scripts/core/GameManager';

wx.onLaunch(() => {
  const canvas = wx.createCanvas();
  const game = new GameManager();
  game.init(canvas, {
    platform: 'wechat',
    touch_enabled: true,
    performance_mode: 'balanced',
  });
});

interface WechatAdapter {
  saveGame(data: string): Promise<void>;
  loadGame(): Promise<string>;
  shareToFriend(title: string, imageUrl: string): void;
  getLeaderboard(): Promise<any[]>;
  vibrateShort(): void;
  vibrateLong(): void;
}
```

## 包体优化策略

| 策略 | 说明 | 目标 |
|------|------|------|
| 代码分包 | 主包 < 4MB，子包按需加载 | 首包 < 4MB |
| 纹理压缩 | 使用 ETC2/ASTC 格式 | 减少 60% 体积 |
| 音频压缩 | MP3/OGG，降低采样率 | 音频 < 5MB |
| 资源CDN | 非首屏资源放 CDN | 首包最小化 |
| 图集合并 | 同类纹理合并为图集 | 减少 DrawCall |

## 性能分级

```typescript
interface PerformanceProfile {
  low: { frame_rate: 30, render_scale: 0.75, max_chunks_loaded: 4, lighting_enabled: false };
  balanced: { frame_rate: 30, render_scale: 1.0, max_chunks_loaded: 6, lighting_enabled: true };
  high: { frame_rate: 60, render_scale: 1.0, max_chunks_loaded: 9, lighting_enabled: true };
}
```

## 触屏适配

| 屏幕 | 策略 |
|------|------|
| < 5 英寸 | 紧凑UI布局，按钮放大 |
| 5-6.5 英寸 | 标准UI布局 |
| > 6.5 英寸 | 扩展UI布局 |
| 平板横屏 | 双栏布局 |
