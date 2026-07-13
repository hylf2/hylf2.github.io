---
layout: post
title: "引擎与架构"
date: 2026-07-13
categories: [technical]
---

## 引擎选型

| 方案 | 优势 | 劣势 | 推荐度 |
|------|------|------|--------|
| **Cocos Creator 3.x** | 原生支持微信小游戏、2D/2.5D 支持良好 | 3D 能力有限 | ⭐⭐⭐⭐⭐ **推荐** |
| Phaser 3 | 纯 Web 技术、开源免费 | 需额外适配小游戏 | ⭐⭐⭐ |
| Unity | 功能强大、生态完善 | 小游戏包体大 | ⭐⭐⭐ |
| 自研引擎 | 完全可控 | 开发成本极高 | ⭐⭐ |

## 项目架构

```
wild-eternal-night/
├── assets/
│   ├── scenes/              # 场景文件
│   ├── scripts/
│   │   ├── core/            # 核心系统 (GameManager, EventBus, ConfigManager)
│   │   ├── player/          # 玩家系统 (Controller, Stats, Inventory, Combat)
│   │   ├── world/           # 世界系统 (WorldGenerator, ChunkManager, TimeSystem)
│   │   ├── entities/        # 实体系统 (Entity, Creature, Boss, Item)
│   │   ├── systems/         # 游戏系统 (Crafting, Building, Combat, Sanity, Ecology)
│   │   ├── ui/              # UI系统 (HUD, InventoryUI, CraftingUI, MenuUI)
│   │   └── utils/           # 工具类 (MathUtils, NoiseGenerator, PathFinding)
│   ├── prefabs/             # 预制体
│   ├── textures/            # 纹理资源
│   ├── audio/               # 音频资源
│   └── data/                # 配置数据（JSON）
├── build-templates/         # 平台构建模板
└── tsconfig.json
```

## 核心系统架构

```typescript
class EventBus {
  on(event: string, callback: Function): void;
  off(event: string, callback: Function): void;
  emit(event: string, data?: any): void;
}

class GameManager {
  state: 'menu' | 'playing' | 'paused' | 'crafting' | 'dead';
  world: WorldGenerator;
  time: TimeSystem;
  weather: WeatherSystem;
  combat: CombatSystem;
  crafting: CraftingSystem;
  sanity: SanitySystem;
  ecology: EcologySystem;
  legacy: LegacySystem;
  save: SaveManager;
  startNewGame(seed?: number): void;
  pause(): void;
  resume(): void;
  onPlayerDeath(): void;
}
```
