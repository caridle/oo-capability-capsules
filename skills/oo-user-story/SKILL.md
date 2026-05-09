---
name: oo-user-story
description: 从 PRD/Epic 拆解用户故事，按 INVEST 原则排序。输入 PRD 或 Epic 描述，输出结构化用户故事列表（含优先级、验收标准、依赖关系）。触发词：用户故事、User Story、Story拆分、INVEST、Epic拆分、敏捷需求、用户故事地图。
---

# OO 能力胶囊 C02：User Story 拆解器

从 PRD/Epic 拆解符合 INVEST 原则的用户故事。

## 触发条件

用户提到：用户故事、User Story、Story 拆分、Epic 拆解、需求拆分、INVEST、敏捷

## 输入格式

- PRD 文档内容（来自 C01）
- 或 Epic 描述文本
- 或自由格式的功能列表

## 输出规范

### 1. Story Map（用户故事地图）

```
         Activity 1        Activity 2        Activity 3
         ─────────         ─────────         ─────────
User     Story 1-1          Story 2-1         Story 3-1
Backbone Story 1-2          Story 2-2         Story 3-2
─────────────────────────────────────────────────────
Walking  Story 1-3
Skeleton
─────────────────────────────────────────────────────
         Story 1-4
```

### 2. 每条 Story 模板

```
STORY-{编号} | 优先级: P{0-3} | 估点: {1,2,3,5,8,13}

作为 <角色>
我希望 <功能>
以便 <价值>

验收标准 (Given-When-Then):
  Scenario 1:
    Given <前置条件>
    When <触发动作>
    Then <预期结果>

  Scenario 2: (边界/异常)
    Given ...
    When ...
    Then ...

依赖: [STORY-xxx, ...]
估点理由: 为什么是这个点数
```

### 3. INVEST 校验矩阵

| Story ID | I(独立) | N(可协商) | V(有价值) | E(可估算) | S(小) | T(可测试) | 通过? |
|----------|---------|-----------|-----------|-----------|-------|-----------|-------|
| STORY-01 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✅ |
| STORY-02 | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ❌ |

### 4. 优先级说明
- P0: MVP 必须，阻塞后续
- P1: 核心流程，首版必须
- P2: 重要但可延后
- P3: 锦上添花

## OO 视角

- 每个 Story 对应一个或多个 Use Case（衔接 C03）
- Story 的角色识别影响 Actor 建模
- 高优先级 Story 的实体优先进入领域模型

## 输出后 Self-Check

- [ ] 每个 Story 是否只有一个明确角色？
- [ ] 验收标准是否覆盖了正常流和异常流？
- [ ] 依赖关系是否标注清晰？
- [ ] P0/P1 是否构成可交付 MVP？
- [ ] 是否有拆得过细（<3点）或过粗（>13点）的情况？
