---
name: oo-activity-diagram
description: 为复杂业务流程生成 UML 活动图（泳道图）。输入 Use Case 规约，输出 PlantUML 活动图：泳道、活动节点、分支/合并、分叉/汇合、对象流。触发词：活动图、Activity Diagram、泳道图、swimlane、业务流程、工作流、flowchart。
---

# OO 能力胶囊 C08：Activity Diagram 生成器

将复杂流程可视化为 UML 活动图（泳道图）。

## 触发条件

活动图、Activity Diagram、泳道、swimlane、业务流程、工作流、flowchart、BPMN

## 输入

- Use Case 规约（C03）
- 或 Sequence Diagram（C06）
- 或业务规则描述

## 输出规范

```plantuml
@startuml
|择偶者|
start
:浏览推荐列表;
:查看用户 B 详细资料;

|系统|
:展示资料+「选择TA」按钮;
:检查择偶者资格;

if (已实名且ACTIVE?) then (是)
  if (未重复选择B?) then (是)
    |择偶者|
    :确认选择;
    :选择支付方式;
    
    |支付网关|
    :生成支付二维码;
    |择偶者|
    :扫码支付¥100;
    
    |支付网关|
    if (支付成功?) then (是)
      |系统|
      :创建MatchSelection;
      :创建72h Conversation;
      :通知 B;
      |择偶者|
      :进入聊天;
      stop
    else (否)
      |系统|
      :提示支付失败;
      stop
    endif
  else (否)
    |系统|
    :提示"已选择过该用户";
    stop
  endif
else (否)
  |系统|
  :提示"请先完成实名认证";
  :引导至认证流程;
  detach
endif
@enduml
```

### 泳道（Partition）定义

```
| 泳道 | 职责 | 对应 OO 类 |
|------|------|-----------|
| 择偶者 | 触发动作、输入信息 | Actor |
| 系统 | 业务规则判断、数据持久化 | MatchService |
| 支付网关 | 支付处理 | IPaymentGateway |
| AI引擎 | 计算匹配 | IAIMatcher |
```

### 节点类型速查

```
(action)     → 原子活动
if/else      → 条件分支
fork/join    → 并行分叉
(*) → start  → 开始
(*) → stop   → 终止
detach       → 异常退出
```

## OO 检查

- [ ] 每个泳道对应明确的 OO 组件？
- [ ] 条件分支是否有互斥完备的守卫？
- [ ] fork 节点是否有对应的 join？
- [ ] 是否有死路径（不可达的 stop）？
