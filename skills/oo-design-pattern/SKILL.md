---
name: oo-design-pattern
description: 分析 Class Diagram，推荐适用的 GoF 设计模式。输入类图，输出模式名称+应用位置+类图改造+利弊分析+替代方案。覆盖创建型/结构型/行为型 23 种模式。触发词：设计模式、Design Pattern、GoF、重构、架构优化、pattern。
---

# OO 能力胶囊 C13：Design Pattern 推荐器

基于类图结构和 SOLID 评估智能推荐设计模式。

## 触发条件

设计模式、Design Pattern、GoF、重构、优化、pattern、Strategy、Factory、Observer

## 输入

- Class Diagram（C05）
- 或具体类的职责描述

## 输出规范

### 模式推荐模板

```
模式: Strategy（策略模式）
类型: 行为型
严重度: ★★★（强烈推荐）

问题:
  MatchService 中硬编码了 RuleBasedMatcher，
  如需切换 MLMatcher 需改源码

方案:
  抽离 IAIMatcher 接口（已存在✓）
  → 注入具体策略实现

Before:
  class MatchService {
    - matcher: RuleBasedMatcher  ← 具体类
  }

After:
  class MatchService {
    - matcher: IAIMatcher        ← 接口
  }

收益: 匹配算法可热替换，符合 OCP
代价: 增加一个接口文件
替代: Template Method（如果算法骨架相同仅步骤不同）
```

### 模式速查表（23 GoF）

```
创建型 (5):
  Factory    → 根据 OrderType 创建 PaymentOrder
  Builder    → 复杂 User 对象构建
  Singleton  → 配置管理器
  Prototype  → 模板 User 克隆
  Abstract Factory → 支付渠道工厂族

结构型 (7):
  Adapter    → 活体检测SDK适配
  Bridge     → 匹配算法×存储方式解耦
  Composite  → 组织架构树
  Decorator  → 日志/缓存增强 Repository
  Facade     → MatchService 对外统一入口
  Proxy      → 支付网关代理
  Flyweight  → 枚举/状态常量共享

行为型 (11):
  Strategy   → 匹配算法/评分策略
  Observer   → 状态变更通知
  State      → UserStatus 状态机
  Template   → 认证流程骨架
  Chain      → 匹配候选过滤链
  Command    → 选择/确认操作封装
  Mediator   → 聊天消息路由
  Memento    → 支付快照/回滚
  Iterator   → 候选池遍历
  Visitor    → 报表导出
  Interpreter → 择偶条件DSL
```

### 决策树

```
需要可替换算法？ → Strategy
需要状态机？    → State
需要通知多个？  → Observer
需要统一入口？  → Facade
需要创建逻辑？  → Factory
需要兼容旧接口？→ Adapter
需要增强功能？  → Decorator
```
