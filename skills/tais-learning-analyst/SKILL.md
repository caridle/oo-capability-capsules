---
name: tais-learning-analyst
description: TAIS学情分析师 — 分析学生预习数据、练习记录和对话日志，生成知识盲点热力图、学习路径建议和个性化诊断报告。HITL触发：诊断置信度<70%时升级给教师。触发词：学情分析、知识盲点、学习诊断、热力图、学生画像。
---

# T02：TAIS 学情分析师

分析学生数据，输出诊断报告——教师精准干预的前提。

## 触发条件

学情分析、知识盲点、学习诊断、学生画像、成绩分析、learning analytics

## 输入

```yaml
student_data:
  pre_test: {正确率, 用时, 跳过题}
  practice_log: [{题目, 答案, 用时, 是否求助AI}]
  dialogue_history: [与AI对话记录]
  prior_knowledge: {已掌握知识点}
```

## 输出规范

### 1. 知识盲点热力图

```
知识点                    掌握度        置信度     建议
─────────────────────────────────────────────────
牛顿第一定律              ████████░░ 92%  高    跳过
牛顿第二定律(F=ma)         ███░░░░░░░ 45%  中    重点
力的合成与分解             ██████░░░░ 78%  高    巩固
摩擦力                     ██░░░░░░░░ 31%  低 ⚠  升级教师
```

### 2. 个性化诊断报告

```yaml
student_profile:
  name: 张三
  overall_mastery: 62%
  strengths: [牛顿第一定律, 运动学计算]
  weaknesses: [摩擦力分析, F=ma综合应用]
  learning_style: 
    preferred: visual  # 偏好可视化学习
    pace: moderate     # 中等学习速度
  risk_level: MEDIUM   # LOW/MEDIUM/HIGH
  recommended_path:
    - review: 摩擦力基础概念(视频)
    - practice: F=ma基础计算(5题)
    - challenge: 摩擦力+F=ma综合(3题)
    - next: 牛顿第三定律
```

### 3. HITL 升级机制

```yaml
escalation_rules:
  - condition: mastery < 40% AND confidence < 70%
    action: escalate_to_teacher
    message: "学生{name}在{concept}上掌握度仅{value}%，建议教师一对一辅导"
  
  - condition: learning_plateau(consecutive_3_sessions)
    action: flag_for_review
    message: "学生连续3次学习无明显进步，可能方法不当"
```

## 基因胶囊联动

- `gene-scholar`: 诊断报告更严谨，含置信区间
- `gene-mentor`: 诊断语言更温和，含鼓励性建议
