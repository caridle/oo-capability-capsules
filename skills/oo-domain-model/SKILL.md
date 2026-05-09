---
name: oo-domain-model
description: 从 Use Case 提炼领域模型（DDD 战术设计）。输入 Use Case 规约，输出概念类图：实体、值对象、聚合根、关联（1:1/1:N/M:N）、继承/实现关系。PlantUML 格式输出。触发词：领域模型、Domain Model、概念类图、DDD、实体、值对象、聚合根、业务建模。
---

# OO 能力胶囊 C04：领域模型构建器

从 Use Case 规约中提取领域概念，构建 DDD 战术设计模型。

## 触发条件

用户提到：领域模型、Domain Model、概念模型、DDD、实体、值对象、聚合、业务对象、名词分析

## 输入格式

- Use Case 规约列表（来自 C03）
- 或 PRD 中的领域概念表（来自 C01）
- 或业务描述文本

## 方法论：名词分析法

扫描 Use Case 文本，提取名词 → 分类为实体/值对象：

```
扫描 UC 文本
  ↓
提取所有名词
  ↓
筛选核心概念（删除 Actor、UI 元素、技术术语）
  ↓
分类：Entity / Value Object / Aggregate Root
  ↓
标注属性和关联
```

## 输出规范

### 1. 概念分类表

```
| 概念 | 类型 | 关键属性 | 唯一标识 | 可变? |
|------|------|---------|---------|------|
| Exam | Aggregate Root | id, title, startTime, duration | ExamId | ✓ |
| Question | Entity | id, content, type, answer | QuestionId | ✓ |
| Score | Value Object | value, grade | - | ✗ |
| Student | Entity | id, name, class | StudentId | ✓ |
| ExamResult | Aggregate Root | id, score, submittedAt | ResultId | ✓ |
```

### 2. PlantUML 类图（概念级）

```plantuml
@startuml
skinparam classAttributeIconSize 0

class Exam {
  - examId: ExamId
  - title: String
  - startTime: DateTime
  - duration: Minutes
  - status: ExamStatus
}

class Question {
  - questionId: QuestionId
  - content: String
  - type: QuestionType
  - answer: String
  - score: Score
}

class ExamResult {
  - resultId: ResultId
  - submittedAt: DateTime
  - totalScore: Score
  - status: ResultStatus
}

class Student {
  - studentId: StudentId
  - name: String
  - className: String
}

enum ExamStatus { DRAFT, PUBLISHED, IN_PROGRESS, FINISHED }
enum QuestionType { SINGLE, MULTIPLE, ESSAY }
enum ResultStatus { SUBMITTED, GRADED, PUBLISHED }

Exam "1" --> "*" Question : contains
Exam "1" --> "*" ExamResult : generates
Student "1" --> "*" ExamResult : submits
@enduml
```

### 3. 关联详细说明

```
| A → B | 类型 | 多重性 | 方向 | 说明 |
|-------|------|--------|------|------|
| Exam → Question | composition | 1:* | A→B | 考试包含题目，考试删除则题目级联 |
| Exam → ExamResult | association | 1:* | A→B | 一次考试有多个结果 |
| Student → ExamResult | association | 1:* | A→B | 一个学生可参加多次考试 |
```

### 4. 聚合边界

```
┌─ Exam Aggregate ─────────────┐
│ Exam (root)                  │
│   ├── Question[*]            │
│   └── ExamResult[*]          │
│       └── Score (VO)         │
└──────────────────────────────┘
```

## OO 原则检查

- [ ] 聚合根是否合理？（修改聚合必须通过根）
- [ ] 值对象是否不可变？
- [ ] 关联是否正确（composition vs aggregation vs association）？
- [ ] 是否避免了"上帝类"（一个类做太多事）？
- [ ] 枚举类型是否完整？

## 衔接 C05

领域模型是 Class Diagram (C05) 的输入。C05 会在概念模型基础上添加：
- 方法签名
- 可见性
- 接口/抽象类
- 设计模式应用
