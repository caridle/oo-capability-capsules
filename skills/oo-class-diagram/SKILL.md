---
name: oo-class-diagram
description: 从领域模型生成完整 OO 类图。输入领域模型，输出 PlantUML 类图含：属性/方法/可见性、接口与抽象类、继承/实现/组合/聚合/依赖、设计模式标注、SOLID 原则验证。触发词：类图、Class Diagram、UML类图、OO设计、接口设计、继承、多态、SOLID。
---

# OO 能力胶囊 C05：Class Diagram 设计器

从领域模型深化为完整的面向对象类图。

## 触发条件

用户提到：类图、Class Diagram、UML、面向对象设计、接口、抽象类、继承、多态、封装、SOLID、设计模式

## 输入格式

- 领域模型（来自 C04）
- 或用户故事 + 用例
- 或现有代码（逆向工程）

## 输出规范

### 1. 完整 PlantUML 类图

```plantuml
@startuml
skinparam classAttributeIconSize 0
skinparam classFontStyle bold

' ── 接口 ──
interface IExamRepository {
  + findById(id: ExamId): Optional<Exam>
  + save(exam: Exam): void
  + findByTeacher(teacherId: TeacherId): List<Exam>
}

interface IGrader {
  + grade(submission: AnswerSheet): Score
}

' ── 抽象类 ──
abstract class User {
  # userId: UserId
  # name: String
  # email: String
  + {abstract} getRole(): Role
  + getName(): String
}

' ── 实体类 ──
class Student extends User {
  - className: String
  - enrolledExams: List<ExamId>
  + getRole(): Role
  + enroll(examId: ExamId): void
  + submitExam(examId: ExamId, answers: AnswerSheet): ExamResult
}

class Teacher extends User {
  - managedExams: List<ExamId>
  + getRole(): Role
  + createExam(dto: CreateExamDTO): Exam
  + publishExam(examId: ExamId): void
}

class Exam {
  - examId: ExamId
  - title: String
  - startTime: DateTime
  - duration: Minutes
  - status: ExamStatus
  - questions: List<Question>
  + publish(): void
  + start(): void
  + finish(): void
  + addQuestion(q: Question): void
  + getQuestionCount(): int
}

class Question <<Entity>> {
  - questionId: QuestionId
  - content: String
  - type: QuestionType
  - answer: String
  - score: Score
  + validate(input: String): boolean
}

class Score <<Value Object>> {
  - value: BigDecimal
  - grade: Grade
  + plus(other: Score): Score
  + isPassing(): boolean
  + toString(): String
}

' ── 枚举 ──
enum ExamStatus { DRAFT, PUBLISHED, IN_PROGRESS, FINISHED }
enum QuestionType { SINGLE_CHOICE, MULTI_CHOICE, ESSAY, FILL_BLANK }
enum Grade { A, B, C, D, F }

' ── 关系 ──
User <|-- Student
User <|-- Teacher
Exam "1" *-- "*" Question : composition ▶
Student "1" --> "*" ExamResult
Teacher "1" --> "*" Exam
Exam ..> IGrader : uses
Exam ..> IExamRepository : uses

@enduml
```

### 2. 类详细规格表

```
| 类名 | 类型 | 父类/接口 | 职责（单一） | 协作者 |
|------|------|----------|-------------|--------|
| Exam | Entity | - | 考试生命周期管理 | Question, IGrader |
| Question | Entity | - | 题目验证与打分 | Score |
| Student | Entity | User | 学生考试行为 | ExamResult |
| Score | Value Object | - | 分数运算与等级 | - |
| IGrader | Interface | - | 评分策略抽象 | - |
```

### 3. SOLID 原则检查

```
S: 每个类只有 1 个职责？
  ✓ Exam: 考试生命周期 (publish/start/finish)
  ✓ Question: 题目验证 (validate)
  ✓ Score: 分数运算 (plus/isPassing)

O: 对扩展开放，对修改关闭？
  ✓ IGrader 接口 → AutoGrader/ManualGrader 可扩展
  ✓ QuestionType 枚举 → 新题型不影响现有逻辑（需配合策略模式）

L: 子类可替换父类？
  ✓ Student.getRole() 和 Teacher.getRole() 返回不同 Role

I: 接口不应强迫实现不需要的方法？
  ✓ IGrader 只有 1 个方法，精细

D: 依赖抽象而非具体？
  ✓ Exam 依赖 IGrader 接口，不依赖具体评分实现
```

### 4. 设计模式建议

```
| 模式 | 应用场景 | 涉及类 |
|------|---------|--------|
| Strategy | 不同题型不同评分策略 | IGrader → AutoGrader, ManualGrader |
| Factory | 根据 QuestionType 创建题目 | QuestionFactory |
| Repository | 数据访问抽象 | IExamRepository |
| Observer | 考试状态变更通知 | Exam → NotificationService |
```

## OO 自查清单

- [ ] 每个类是否单一职责（SRP）？
- [ ] 是否存在 God Class（>10 个 public 方法）？
- [ ] 组合(◆)和聚合(◇)是否正确区分？
- [ ] 接口是否最小化（ISP）？
- [ ] 循环依赖是否已消除？
- [ ] Value Object 是否 immutable？
