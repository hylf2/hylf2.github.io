---
layout: post
title: "世界生成规则"
date: 2026-07-13
categories: [core-mechanics]
---

## 地图参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `MAP_WIDTH` | 512 | 地图宽度(格) |
| `MAP_HEIGHT` | 512 | 地图高度(格) |
| `TILE_SIZE` | 32 | 每格像素 |
| `CHUNK_SIZE` | 16 | 区块大小(格) |
| `SEA_LEVEL` | 0.4 | 海平面阈值 (Noise 0-1) |

## 地形生成算法

使用多层 Perlin Noise 叠加生成地形：

```typescript
interface WorldGenConfig {
  seed: number;
  terrain: {
    noise_scale: 0.02;
    octaves: 6;
    persistence: 0.5;
    lacunarity: 2.0;
  };
  temperature: {
    noise_scale: 0.005;
    base_gradient: true;
  };
  moisture: {
    noise_scale: 0.01;
  };
  caves: {
    noise_scale: 0.05;
    threshold: 0.3;
  };
}
```

## 生态群落（Biome）分配

基于温度和湿度的组合决定生态群落类型：

| 温度 \ 湿度 | 干燥 (0-0.3) | 中等 (0.3-0.6) | 潮湿 (0.6-1.0) |
|-------------|-------------|---------------|---------------|
| **寒冷 (0-0.3)** | 冻原 Tundra | 针叶林 Taiga | 冰封沼泽 Frozen Swamp |
| **温和 (0.3-0.7)** | 草原 Plains | 森林 Forest | 湿地 Wetland |
| **炎热 (0.7-1.0)** | 荒漠 Desert | 稀树草原 Savanna | 热带丛林 Jungle |

## 特殊区域

| 区域 | 生成条件 | 特征 |
|------|---------|------|
| 暗影裂谷 | 世界生成时强制 1-2 个 | 暗元素高浓度，强力怪物 |
| 远古遗迹 | 每个群落 1-3 个 | 内含宝箱、谜题、叙事碎片 |
| 地下洞穴 | 噪声洞穴层 | 稀有矿石、地下生物 |
| 世界之树 | 世界中心 | 最终 Boss 区域 |

## 资源分布规则

```typescript
interface ResourceDistribution {
  resource_id: string;
  biomes: string[];
  spawn_density: number;
  cluster_size: { min: number; max: number };
  depth_range?: { min: number; max: number };
  respawn_time: number;
}
```
