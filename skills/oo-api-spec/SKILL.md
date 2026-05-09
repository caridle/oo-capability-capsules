---
name: oo-api-spec
description: 从 Sequence Diagram 和 Class Diagram 生成 OpenAPI 3.0 规范文档。输入时序图和类图，输出完整 RESTful API 文档（路径/方法/参数/响应/错误码/认证）。触发词：API文档、OpenAPI、Swagger、接口文档、REST API、API设计。
---

# OO 能力胶囊 C11：API Spec 生成器

从时序图提取 API 契约，生成 OpenAPI 3.0 规范。

## 触发条件

API文档、OpenAPI、Swagger、接口文档、REST API、API Spec、API设计

## 输入

- Sequence Diagram（C06）
- Class Diagram 中的 Controller/Boundary 类

## 输出规范

### OpenAPI 3.0 YAML

```yaml
openapi: "3.0.3"
info:
  title: 缘识 API
  version: "1.0.0"
  description: AI婚恋平台核心API

paths:
  /api/recommend:
    get:
      summary: 获取每日AI推荐
      tags: [Match]
      security: [{bearerAuth: []}]
      responses:
        '200':
          description: 推荐列表（最多10位）
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/ScoredCandidate'

  /api/select/{userId}:
    post:
      summary: 主动选择用户（需支付）
      tags: [Match]
      parameters:
        - name: userId
          in: path
          required: true
          schema: {type: string}
      responses:
        '200':
          description: 返回支付信息
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaymentOrder'

  /api/conversation/{id}/confirm:
    post:
      summary: 确认关系
      tags: [Conversation]
      responses:
        '200':
          description: 确认成功或等待对方

  /api/user/unseal:
    post:
      summary: 付费解封
      tags: [User]
      responses:
        '200':
          description: 解封成功

components:
  schemas:
    ScoredCandidate:
      properties:
        userId: {type: string}
        compatibility: {type: integer, minimum: 0, maximum: 100}
        age: {type: integer}
        education: {type: string}
    PaymentOrder:
      properties:
        orderId: {type: string}
        amount: {type: number}
        qrCode: {type: string}
        status: {$ref: '#/components/schemas/PaymentStatus'}
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
```

### API ←→ 时序图映射

```
| 端点 | 方法 | 控制器 | 时序图步骤 |
|------|------|--------|-----------|
| /api/recommend | GET | SelectionController | AI引擎→候选池→Top10 |
| /api/select/{id} | POST | SelectionController | 资格校验→支付→通知 |
| /api/conv/{id}/confirm | POST | ConversationController | 双向确认→锁定 |
| /api/user/unseal | POST | UserController | 支付→解除关系→解锁 |
```

### 错误码规范

```
| 状态码 | 含义 | 示例 |
|--------|------|------|
| 400 | 参数错误 | 重复选择 |
| 401 | 未认证 | Token过期 |
| 403 | 无权限 | LOCKED状态操作 |
| 404 | 不存在 | 用户/会话不存在 |
| 409 | 冲突 | 并发关系确认 |
| 422 | 业务规则 | 未实名/已在关系中 |
```
