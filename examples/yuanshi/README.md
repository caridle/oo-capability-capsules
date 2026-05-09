# 实战案例：AI 婚恋网站「缘识」

> 用一句话需求跑通 C01→C14 完整 OO 能力胶囊链路

## 需求

> "AI婚恋网站，实名入库免费，AI分析自动匹配，主动选择对象100元一次不退，确认关系后双方锁定，付费解封恢复选择权"

## C01→C14 产出速览

| 胶囊 | 产出 | 亮点 |
|------|------|------|
| C01 PRD | 8节完整需求文档 | MoSCoW 7项MustHave、竞品对比、8周里程碑 |
| C02 Story | 14条INVEST Story | 总估点60、MVP范围P0×8条 |
| C03 UseCase | 11个UC + PlantUML图 | UC-05主动选择含8个扩展场景 |
| C04 领域模型 | 3聚合根 + 10枚举 | DDD战术设计：User/MatchSelection/Conversation |
| C05 ClassDiagram | 6接口+2抽象类+SOLID | 6种设计模式、3个状态机 |
| C06 Sequence | 3张核心时序图 | 选择支付、确认锁死、解封全流程 |
| C07 State | 3个状态机 | User(ACTIVE⇄LOCKED)、Selection、Payment |
| C08 Activity | 泳道活动图 | 择偶者/系统/支付网关三泳道 |
| C09 Component | 5域组件图 | 用户/匹配/交流/支付 + 外部服务 |
| C10 UI | 6页面清单 | 状态差异化UI（ACTIVE/LOCKED/BANNED） |
| C11 API | 9个REST端点 | OpenAPI 3.0 + 6种错误码 |
| C12 DB | DDL + ER图 | PostgreSQL Partial Index防重复选择 |
| C13 Pattern | GoF 23模式速查 | Strategy/State/Observer等6个强推 |
| C14 Code | DDD分层骨架 | domain/application/ports/infrastructure + 测试 |

## 核心商业逻辑

```
实名认证(免费) → AI推荐(免费) → 主动选择(¥100/次不退)
→ 线上交流(72h) → 双方确认 → 自动锁死(排他)
→ 付费解封 → 恢复自由

防作弊：
- 活体检测 + 身份证OCR
- 锁定后双方同时从推荐池消失
- 解封时双方同时恢复
```

## 完整 PlantUML 图表

以下图表代码可直接复制到 https://plantuml.com 渲染：

- [Use Case 图](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/use-case.puml)
- [领域模型图](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/domain-model.puml)
- [完整 Class Diagram](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/class-diagram.puml)
- [Sequence: 主动选择](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/sequence-select.puml)
- [Sequence: 确认锁死](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/sequence-confirm.puml)
- [Sequence: 解封](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/sequence-unseal.puml)
- [State: User状态机](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/state-user.puml)
- [Activity: 主动选择泳道图](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/activity-select.puml)
- [Component: 系统组件图](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/component.puml)
- [ER: 数据库设计](https://github.com/caridle/oo-capability-capsules/blob/main/examples/yuanshi/diagrams/er-diagram.puml)

## 产出文档

完整的 C01→C14 产出文档已存入 OmniNote，按胶囊分 14 篇独立笔记，标签 `C01`~`C14`。
