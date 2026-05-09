---
name: oo-component-diagram
description: 从 Class Diagram 生成 UML 组件图。输入完整类图，输出 PlantUML 组件图：组件、接口（提供/需求）、端口、依赖关系、部署节点。触发词：组件图、Component Diagram、架构图、模块设计、微服务、系统拆分、接口设计。
---

# OO 能力胶囊 C09：Component Diagram 生成器

从类图推导系统组件划分与接口。

## 触发条件

组件图、Component Diagram、模块设计、架构图、系统拆分、微服务、包图

## 输出规范

```plantuml
@startuml
package "用户域" {
  [UserService] <<service>>
  [IdentityService] <<service>>
  interface "IUserRepository" as IUR
  [UserService] -- IUR
}

package "匹配域" {
  [MatchService] <<service>>
  [AIEngine] <<service>>
  interface "IMatchRepository" as IMR
  interface "IAIMatcher" as IAM
  [MatchService] -- IMR
  [MatchService] --> IAM
}

package "交流域" {
  [ConversationService] <<service>>
  [NotificationService] <<service>>
  interface "IConversationRepository" as ICR
  [ConversationService] -- ICR
}

package "支付域" {
  [PaymentService] <<service>>
  interface "IPaymentGateway" as IPG
  [PaymentService] --> IPG
}

cloud "外部服务" {
  [微信支付] as WX
  [支付宝] as ALI
  [活体检测SDK] as Liveness
}

[PaymentService] --> IUR : 用户查询
[MatchService] --> IUR : 候选查询
[ConversationService] --> IMR : 选择关联
[IPG] <.. WX
[IPG] <.. ALI
[IdentityService] --> Liveness
@enduml
```

### 组件拆分原则

```
| 原则 | 说明 |
|------|------|
| 高内聚 | 同一聚合的类放同一组件 |
| 低耦合 | 组件间仅通过接口通信 |
| 接口隔离 | 每个组件暴露最小接口 |
| 单向依赖 | 避免循环依赖 |
```

### 组件描述表

```
| 组件 | 类型 | 提供接口 | 需求接口 | 技术栈建议 |
|------|------|---------|---------|-----------|
| UserService | Service | UserAPI | IUserRepository | Spring Boot |
| MatchService | Service | MatchAPI | IAIMatcher, IUserRepository | Python/FastAPI |
| PaymentService | Service | PaymentAPI | IPaymentGateway | Go/Gin |
```
