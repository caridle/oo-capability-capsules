---
name: tais-feedback-collector
description: TAIS反馈采集器 — 全流程记录学生学习轨迹：预习数据、对话日志、练习记录、产出成果、AI交互。结构化为训练数据，驱动教学优化和AI进化。触发词：反馈采集、学习轨迹、数据收集、learning record、process data。
---

# T06：TAIS 反馈采集器

学习过程留痕——每一步都是进化燃料。

## 触发条件

反馈采集、学习轨迹、数据收集、learning record、过程数据、留痕

## 采集维度

```yaml
采集节点:
  课前:
    - pre_test_result: {题目, 答案, 正确率, 用时}
    - prior_knowledge_check: {已知知识点, 自评掌握度}
    
  课中:
    - dialogue_log: [{时间, 角色(学生/AI), 内容, 追问轮次}]
    - interaction_pattern: {求助频率, 回答质量, 停顿时长}
    - milestone: {关键认知突破点, 时间戳}
    
  课后:
    - assignment: {题目, 学生答案, AI反馈, 是否修改}
    - reflection: {学生自评, 困难点, 收获}
    - quiz_result: {正确率, 薄弱点分布}

  元数据:
    - session_id: 学习会话ID
    - student_id: 学生ID(脱敏)
    - concept: 知识点
    - timestamp: 时间戳
```

## 数据结构化输出

```yaml
feedback_record:
  session: S20260509-001
  student: STU_A1B2
  concept: newton_second_law
  
  pre:
    mastery_self: 3/10
    pre_test_score: 40%
    
  during:
    dialogue_rounds: 8
    ai_interventions: 5
    student_questions: 2
    breakthrough_point: "第6轮追问后理解F=ma"
    stuck_points: ["力的方向判断"]
    
  post:
    assignment_score: 85%
    improvement: +45%
    remaining_blindspots: ["斜面问题中的受力分析"]
```

## 聚合分析

```yaml
class_report:
  concept: newton_second_law
  total_students: 45
  avg_improvement: 38%
  common_stuck_points: ["力的分解", "摩擦力方向"]
  avg_dialogue_rounds: 6.2
  hitl_escalation_rate: 12%
  teacher_intervention_rate: 8%
```

## 能力胶囊联动

- `oo-db-schema`: 反馈数据表设计
- `tais-learning-analyst`: 分析聚合数据
