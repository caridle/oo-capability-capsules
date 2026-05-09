# OO Capability Capsules 🧬

> 14 个独立的面向对象软件工程能力胶囊，覆盖从需求到代码的全生命周期。

## 什么是能力胶囊？

"能力胶囊"是面向 AI Agent 的独立 SOP（标准作业程序）技能包。每个胶囊封装一个 OO 软件工程阶段的标准产出物——给定输入，按规范流程产出结构化输出。

**核心理念：** 像乐高积木一样，每个胶囊独立可复用，组合起来覆盖完整 OO 开发链路。

## 胶囊清单

| 层 | 编号 | 胶囊 | 输入 | 输出 |
|----|------|------|------|------|
| 📋 需求 | C01 | PRD 生成器 | 一句话需求 | 8节结构化PRD（MoSCoW+验收+里程碑） |
| 📋 需求 | C02 | User Story 拆解 | PRD/Epic | 14条INVEST Story + 估点 + MVP范围 |
| 🎯 分析 | C03 | Use Case 建模 | User Story | PlantUML用例图 + 完整规约 |
| 🎯 分析 | C04 | 领域模型 | Use Case | DDD战术设计 + 聚合边界 |
| 🏗️ 设计 | C05 | Class Diagram | 领域模型 | 完整OO类图 + SOLID检查 |
| 🏗️ 设计 | C06 | Sequence Diagram | Use Case | PlantUML时序图（多流程+异常分支） |
| 🏗️ 设计 | C07 | State Diagram | Class Diagram | UML状态机 + entry/exit/guard |
| 🏗️ 设计 | C08 | Activity Diagram | Use Case | 泳道活动图（分支/分叉/对象流） |
| 🏗️ 设计 | C09 | Component Diagram | Class Diagram | 组件图 + 接口 + 技术栈 |
| 🖥️ 实现 | C10 | UI Prototype | User Story | HTML原型 + 页面清单 |
| 🖥️ 实现 | C11 | API Spec | Sequence Diagram | OpenAPI 3.0 YAML |
| 🖥️ 实现 | C12 | DB Schema | 领域模型 | DDL + ER图 + 索引策略 |
| 🖥️ 实现 | C13 | Design Pattern | Class Diagram | GoF 23模式推荐 + 利弊分析 |
| 🖥️ 实现 | C14 | Code Scaffold | 全套设计产出 | DDD分层项目骨架 + 测试模板 |

## 调用链路

```
一句话需求
  │
  ▼
C01 PRD ──→ C02 User Story ──→ C03 Use Case ──→ C04 领域模型
                                                    │
                                                    ▼
                                              C05 Class Diagram
                                              ├── C06 Sequence
                                              ├── C07 State
                                              ├── C08 Activity
                                              └── C09 Component
                                                    │
                                                    ▼
                                              C10 UI ── C11 API ── C12 DB
                                              ├── C13 Design Pattern
                                              └── C14 Code Scaffold
```

## 实战案例：AI 婚恋网站「缘识」

用一句话需求跑通 C01→C14 完整链路：

> "AI婚恋网站，实名入库，AI匹配，100元主动选择，确认后锁死，付费解封"

[查看完整产出 →](examples/yuanshi/README.md)

## 格式标准

- **文档输出**：Markdown（含表格、清单、矩阵）
- **图表输出**：PlantUML（`@startuml` ... `@enduml`）
- **代码输出**：按项目技术栈（Java/Python/Go/SQL）

## 设计原则

1. **独立可复用** — 每个胶囊独立工作，不强制走完整链路
2. **OO 为核心** — SOLID、设计模式、DDD 内建于每个胶囊
3. **PlantUML 原生** — 所有图表代码可直接渲染
4. **AI Agent 原生** — 设计为 AI 调用，含触发词、模板、自查清单

## 安装

```bash
# 克隆仓库
git clone https://github.com/YOUR_USER/oo-capability-capsules.git

# 复制到你的 Agent Skills 目录
cp -r oo-capability-capsules/skills/* ~/.skills/
```

## 使用

在支持 Skills 的 AI Agent（Hermes / Claude Code 等）中直接说：

```
"把这个需求生成PRD"
"画一下User的状态机"
"从类图生成数据库Schema"
```

AI 会自动加载对应胶囊并按规范产出。

## 贡献

欢迎贡献新胶囊或改进现有胶囊。每个胶囊遵循统一模板：

```markdown
---
name: oo-xxx
description: 描述 + 触发词
---

# OO 能力胶囊 CXX：名称

## 触发条件
## 输入格式
## 输出规范
## OO 原则检查
## 自查清单
```

## 协议

MIT License

## 作者

王新年 · 中南民族大学计算机学院
