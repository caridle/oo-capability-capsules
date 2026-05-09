---
name: oo-state-diagram
description: 为关键领域对象生成 UML 状态图。输入 Class Diagram 中的实体类，输出 PlantUML 状态图：状态、转移、守卫条件、entry/exit/do 动作。触发词：状态图、State Diagram、状态机、State Machine、状态转移、生命周期。
---

# OO 能力胶囊 C07：State Diagram 生成器

从 Class Diagram 提取有状态对象，生成 UML 状态图。

## 触发条件

状态图、State Diagram、状态机、state machine、生命周期、状态转移、guard condition

## 输入

- Class Diagram（C05 产出）
- 或指定需要状态建模的类名

## 输出规范

### 对每个有状态实体，生成：

```plantuml
@startuml
state "User 生命周期" as UserLife {

  [*] --> ACTIVE : 注册+实名通过

  state ACTIVE {
    [*] --> BROWSING
    BROWSING --> SELECTING : 发起选择
    SELECTING --> BROWSING : 选择完成
  }

  ACTIVE --> LOCKED : 双方确认关系 [bothConfirmed()]
  LOCKED --> ACTIVE : 付费解封 /payUnsealFee()/

  ACTIVE --> BANNED : 违规审核 /3次举报/
  LOCKED --> BANNED : 严重违规

  BANNED --> [*]
}

note right of LOCKED
  entry / removeFromPool()
  exit / addToPool()
  do / preventNewSelection()
end note
@enduml
```

### 状态规格表

```
| 状态 | entry动作 | exit动作 | do动作 | 合法转移至 |
|------|----------|---------|--------|-----------|
| ACTIVE | - | - | 接收推荐 | LOCKED, BANNED |
| LOCKED | removeFromPool | addToPool | 阻断选择 | ACTIVE, BANNED |
| BANNED | disableAccount | - | - | - |
```

### 守卫条件标注

每个转移上标注 `[guard]`：
- `[bothConfirmed()]` → 仅双方确认时触发
- `[paymentSuccess()]` → 支付成功
- `[reportCount >= 3]` → 举报达阈值

## OO 检查

- [ ] 是否识别了所有有状态的核心类？
- [ ] 每个状态是否有明确的 entry/exit 动作？
- [ ] 是否有不可达状态（orphan state）？
- [ ] 转移条件是否互斥且完备？
