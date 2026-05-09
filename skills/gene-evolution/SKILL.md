---
name: gene-evolution
description: 进化记忆基因胶囊 — 存储Agent的历史经验：成功模式库、教训库、场景适应记录。支持增量更新和代际遗传。触发词：进化、记忆、经验、教训、成功模式、学习、adaptation。
---

# 🧬 G06：进化记忆基因胶囊

Agent 的"海马体"——存储学到的经验，支持持续进化。

## 触发条件

进化、记忆、经验、教训、成功模式、学习、adaptation、evolution

## 记忆结构

```yaml
evolution_memory:
  success_patterns:
    - id: SP-001
      scenario: "PRD_生成"
      pattern: "先确认MoSCoW分级→再拆解→最后验收标准"
      effectiveness: 0.92
      uses: 15
      
    - id: SP-002
      scenario: "ClassDiagram_设计"
      pattern: "从领域模型出发→定义接口→继承关系→SOLID检查"
      effectiveness: 0.88
      uses: 12
      
  lessons:
    - id: LE-001
      scenario: "跳过领域模型直接画类图"
      consequence: "类图反复返工3次"
      fix: "严格走 C04→C05 链路"
      severity: HIGH
      
    - id: LE-002
      scenario: "User_Story_拆解过粗"
      consequence: "估点不准确，Sprint延期"
      fix: "Story > 8点必须进一步拆分"
      severity: MEDIUM

  adaptations:
    - field: "AI婚恋平台"
      tweak: "支付流程需要额外的幂等校验"
      reason: "金融场景对一致性要求极高"
```

## 进化机制

### 增量学习
```
每次任务完成后:
  成功 → 记录到 success_patterns，effectiveness += 0.02
  失败 → 记录到 lessons，标记 severity
         → 更新 adaptations
```

### 基因参数微调
```
用户反馈 φ ∈ [-1, +1] →
  ρ(t+1) = clip(ρ(t) + η·φ·sign(ρ_expected - ρ(t)))
  τ(t+1) = clip(τ(t) - η·φ·0.1)
```

## 能力胶囊交互

```
C02(UserStory) 遇到"拆解过粗"场景
  → 基因检索 LE-002
  → 自动建议："根据历史经验，建议将 >8点的Story进一步拆分"
  
C05(ClassDiagram) 在新项目首次使用
  → 基因检索 SP-002
  → 自动应用成功模式
```
