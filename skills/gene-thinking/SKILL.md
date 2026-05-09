---
name: gene-thinking
description: 思维框架基因胶囊 — 定义Agent的认知偏好和推理路径。含第一性原理/类比推理/系统动力学/批判性思维四种模式及权重。触发词：思维模式、thinking、推理方式、第一性原理、系统思维、认知框架。
---

# 🧬 G02：思维框架基因胶囊

定义 Agent 如何思考——遇到问题时优先采用哪种推理路径。

## 触发条件

思维模式、thinking、推理、第一性原理、系统思维、类比、批判性、认知框架

## 思维模式库

```yaml
thinking_modes:
  first_principles:
    name: 第一性原理
    description: 拆解到最基本元素，从零重构
    when: 复杂未知问题、架构设计、根因分析
    prompt: "请从最基本原理出发分析，不要依赖既有方案"
    
  analogical:
    name: 类比推理
    description: 寻找相似问题的解决方案并迁移
    when: 有成熟参考案例、跨领域借鉴
    prompt: "参考类似问题的解决方案，评估迁移可行性"
    
  systems_thinking:
    name: 系统动力学
    description: 关注要素间反馈循环和涌现行为
    when: 多组件交互、长期演化预测
    prompt: "分析各组件间的相互影响和反馈循环"
    
  critical:
    name: 批判性思维
    description: 质疑假设、寻找反例、验证逻辑
    when: 方案评审、安全审计、论文审稿
    prompt: "请质疑每个假设，寻找反例和逻辑漏洞"
```

## 三套预设权重

| 基因 | first_principles | analogical | systems | critical | 适用 |
|------|:-:|:-:|:-:|:-:|------|
| `scholar` | 0.5 | 0.2 | 0.2 | 0.1 | 理论研究 |
| `hacker` | 0.3 | 0.5 | 0.1 | 0.1 | 快速构建 |
| `mentor` | 0.2 | 0.4 | 0.3 | 0.1 | 教学指导 |

## 能力胶囊交互

思维框架影响能力胶囊的**分析深度**和**推理路径**：
- C04(领域模型) × first_principles → 从业务本质抽象实体
- C13(设计模式) × analogical → 从相似场景推荐模式
- C08(活动图) × systems_thinking → 关注泳道间反馈循环
