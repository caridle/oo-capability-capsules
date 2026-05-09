---
name: oo-ui-prototype
description: 从 User Story 生成可交互 HTML UI 原型。输入用户故事和 Class Diagram，输出完整 HTML/CSS 页面（基于 html-ppt 设计系统或独立），含导航、表单、列表、状态展示。触发词：UI原型、界面原型、HTML原型、wireframe、mockup、页面设计、前端原型。
---

# OO 能力胶囊 C10：UI Prototype 生成器

从用例和类图生成可运行的 HTML 界面原型。

## 触发条件

UI原型、界面原型、HTML原型、wireframe、mockup、页面、前端

## 输入

- User Story（C02）+ Use Case（C03）
- 或 Class Diagram 中的 Boundary 类
- 或页面清单

## 输出规范

### 1. 页面清单

```
| 页面 | 路由 | 对应用例 | 核心组件 |
|------|------|---------|---------|
| 推荐页 | /recommend | UC-04,06 | 用户卡片列表 |
| 资料详情 | /profile/:id | UC-05 | 资料面板+选择按钮 |
| 支付确认 | /pay/confirm | UC-05,08 | 价格+支付方式 |
| 聊天页 | /chat/:id | UC-07 | 消息列表+AI建议 |
| 关系页 | /relationship | UC-08,09 | 状态+解封按钮 |
```

### 2. 生成 HTML 原型

参照 html-ppt 设计系统或独立 HTML：
- 使用 CSS 变量（Token）保持一致性
- 响应式设计（mobile-first）
- 遵循 Boundary 类接口

### 3. OO 视角

- 每个页面 = 一个 Boundary 类的实例化
- 页面状态映射到实体的状态机
- 按钮触发 → Control 类方法调用
- 数据展示 → DTO/ViewModel

### 4. 自查

- [ ] 每个用例主成功场景是否有对应页面？
- [ ] LOCKED/ACTIVE/BANNED 状态是否有差异化 UI？
- [ ] 核心操作（选择/确认/解封）是否有确认步骤防误触？
