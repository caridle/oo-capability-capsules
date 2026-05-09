---
name: gene-heredity
description: 遗传引擎基因胶囊 — 实现基因的复制、交叉、突变和代际选择。支持从父代基因产生子代、基因谱系追踪和适应度驱动的演化。触发词：遗传、交叉、突变、繁殖、基因演化、子代、population、crossover。
---

# 🧬 G07：遗传引擎基因胶囊

基因的"繁殖系统"——实现复制、交叉、突变和选择。

## 触发条件

遗传、交叉、突变、繁殖、基因演化、子代、population、crossover、heredity

## 遗传操作

### 1. 复制 (Clone)
```
clone(G) → G'  (G' 与 G 完全相同，θ.v += 0.0.1)
用途: 备份、分发、基线保存
```

### 2. 交叉 (Crossover)
```
crossover(G_a, G_b, λ=0.6) → G_c

连续参数（人格维度、决策参数）:
  p_c = λ·p_a + (1-λ)·p_b

离散参数（角色原型、思维模式）:
  均匀交叉，随机选择父代之一的值

示例:
  G_scholar(ρ=0.2) × G_hacker(ρ=0.8), λ=0.6
  → G_child(ρ=0.44)  ← 偏保守的新基因
```

### 3. 突变 (Mutation)
```
mutate(G, μ=0.05) → G'

对每个连续参数 x:
  if random() < μ:
    x' = x + N(0, 0.1)
    x' = clip(x', 0, 1)

离散参数:
  if random() < μ:
    随机替换为种群中其他值
```

### 4. 选择 (Selection)
```
tournament_select(population, k=3):
  随机选k个个体 → 返回F(G)最高的

fitness(G) = 0.4·Q + 0.3·U + 0.3·S
  Q: 任务完成质量
  U: 用户满意度
  S: 安全合规率
```

## 演化算法

```yaml
evolution_params:
  population_size: 10
  generations: 20
  crossover_rate: 0.7
  mutation_rate: 0.05
  elitism: 2  # 保留前2名直接进入下一代

流程:
  P ← 初始种群(10个随机/预设基因)
  for gen=1..20:
    评估所有 F(G)
    保留前2(elite)
    生成8个新个体(交叉+突变)
    P ← elite + new_offspring
  return argmax F(G)
```

## 基因谱系

```yaml
lineage:
  G_v1.0: [原始学者基因]
    ├── G_v1.1: [学者 + 微突变]
    ├── G_v2.0: [学者 × 黑客, λ=0.6]
    │   ├── G_v2.1: [v2.0 + 金融场景适应]
    │   └── G_v2.2: [v2.0 + 教学场景适应]
    └── G_v3.0: [学者 × 导师, λ=0.4]
```

## 能力胶囊交互

```
用户: "创建一个适合金融场景的新基因"
  → G07: 加载 scholar + 金融场景适应记录
  → G07: mutate(ρ→0.1, τ→0.9)  ← 更保守
  → G07: 添加风控规则 R06(金融合规)
  → 输出: G_finance_v1.0
```
