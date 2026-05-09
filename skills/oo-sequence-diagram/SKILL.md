---
name: oo-sequence-diagram
description: 从 Use Case 和 Class Diagram 生成 UML 时序图。输入 Use Case 规约，输出 PlantUML 时序图：生命线、消息（同步/异步/返回）、alt/loop/opt/par 片段、自调用。触发词：时序图、Sequence Diagram、UML时序、消息序列、交互图、生命线、调用链。
---

# OO 能力胶囊 C06：Sequence Diagram 生成器

将 Use Case 的主成功场景转化为对象协作时序图。

## 触发条件

用户提到：时序图、Sequence Diagram、消息序列、交互流程、调用链、生命线、UML 时序

## 输入格式

- Use Case 规约（来自 C03）
- 或 Class Diagram（来自 C05）
- 或一段业务流程描述

## 输出规范

### 1. PlantUML 时序图

```plantuml
@startuml
actor Student
participant "ExamController" as EC
participant "ExamService" as ES
participant "Exam" as E
participant "IExamRepository" as Repo
database "DB" as DB
participant "AutoGrader" as AG

== UC-02: 参加考试 ==

Student -> EC: GET /exam/{id}/start
activate EC

EC -> ES: startExam(examId, studentId)
activate ES

ES -> Repo: findById(examId)
activate Repo
Repo -> DB: SELECT * FROM exams WHERE id=?
DB --> Repo: Exam(row)
Repo --> ES: Optional<Exam>
deactivate Repo

alt Exam not found
  ES --> EC: throw ExamNotFoundException
  EC --> Student: 404 Not Found
else Exam not published
  ES --> EC: throw ExamNotAvailableException
  EC --> Student: 403 Forbidden
else OK
  ES -> E: start()
  activate E
  E --> ES: void
  deactivate E

  ES -> Repo: save(exam)
  Repo -> DB: UPDATE exams SET status='IN_PROGRESS'
  DB --> Repo: OK
  Repo --> ES: void

  ES --> EC: ExamStartedDTO
  deactivate ES

  EC --> Student: 200 + 第一题
end
deactivate EC

== 提交答案 ==

Student -> EC: POST /exam/{id}/submit {answers}
activate EC

EC -> ES: submitAnswers(examId, studentId, answers)
activate ES

ES -> AG: grade(answers)
activate AG

loop for each question
  AG -> AG: validate answer
end

AG --> ES: Score(85, B)
deactivate AG

ES -> E: recordResult(studentId, score)
activate E
E --> ES: ExamResult
deactivate E

ES -> Repo: saveResult(result)
Repo -> DB: INSERT INTO exam_results
DB --> Repo: OK
Repo --> ES: void

ES --> EC: ExamResultDTO{score:85, grade:B}
deactivate ES
EC --> Student: 200 OK {score: 85, grade: B}
deactivate EC

@enduml
```

### 2. 参与者映射

```
| 图中名称 | 类型 | 对应类 | 职责 |
|----------|------|--------|------|
| Student | Actor | - | 触发用例 |
| ExamController | Boundary | ExamController | HTTP 请求处理 |
| ExamService | Control | ExamService | 业务逻辑编排 |
| Exam | Entity | Exam | 考试领域对象 |
| IExamRepository | Interface | IExamRepository | 数据访问抽象 |
| DB | External | - | 持久化存储 |
| AutoGrader | Service | AutoGrader | 自动评分 |
```

### 3. 消息契约

```
| 消息 | 方向 | 参数 | 返回 | 异常 |
|------|------|------|------|------|
| startExam | → | examId, studentId | ExamStartedDTO | ExamNotFound, ExamNotAvailable |
| findById | → | examId | Optional<Exam> | - |
| start() | → | - | void | IllegalStateException |
| submitAnswers | → | examId, studentId, answers | ExamResultDTO | ValidationException |
| grade() | → | answers | Score | - |
```

### 4. 组合片段说明

```
| 片段 | 用途 | 示例 |
|------|------|------|
| alt | 条件分支 | 正常/异常路径 |
| loop | 循环 | 遍历题目列表 |
| opt | 可选 | 仅当条件满足时执行 |
| par | 并行 | 同时发多个请求 |
| ref | 引用 | 引用另一个时序图 |
```

## OO 设计要点

- **Boundary**: 处理外部请求，不包含业务逻辑
- **Control**: 编排业务流程，协调多个 Entity/Service
- **Entity**: 领域对象，封装自身状态和行为
- **自调用** (`A -> A`): 表示对象内部方法调用，体现封装
- **返回消息** (`-->`): 虚线箭头，表示返回值

## 自查清单

- [ ] 每个 Use Case 主成功场景是否完整覆盖？
- [ ] Border/Control/Entity 是否职责分明？
- [ ] 是否有超长生命线（>10 条消息）→ 考虑拆分
- [ ] 异常路径是否通过 alt/opt 片段覆盖？
- [ ] 数据库/外部系统是否正确标识？
