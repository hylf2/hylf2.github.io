---
layout: post
title: "网络与社交"
date: 2026-07-13
categories: [technical]
---

## 排行榜（微信小程序）

```typescript
interface LeaderboardEntry {
  player_name: string;
  avatar_url: string;
  survival_days: number;
  bosses_defeated: number;
  total_deaths: number;
  board_type: 'survival' | 'boss' | 'exploration';
}
```

## 死亡分享卡片

```typescript
interface DeathShareCard {
  title: string;               // "我在荒野永夜中存活了 15 天"
  image: string;               // 自动生成的截图 + 统计数据
  epitaph: string;             // 墓志铭
  stats_summary: {
    days: number;
    kills: number;
    boss_defeated: number;
    cause_of_death: string;
  };
  share_to: 'wechat_friend' | 'wechat_moments';
}
```

## PC 端社交功能

| 功能 | 实现 |
|------|------|
| 死亡记录导出 | JSON/图片格式，可手动分享 |
| 存档导出 | 可导出存档文件 |
| 统计面板 | 本地详细统计（无在线排行） |
