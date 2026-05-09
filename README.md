# 能力胶囊与基因胶囊 🧬

> **AI Agent 双胶囊架构** — 能力胶囊（术）决定能做什么，基因胶囊（道）决定怎么做。28 个技能胶囊：14 OO能力 + 7 基因 + 7 TAIS教学。

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

## 基因胶囊 (Gene Capsules)

除能力胶囊外，本仓库还包含 **7 个基因胶囊**，封装 Agent 的人格与行为模式：

| 编号 | 基因胶囊 | 核心问题 |
|------|---------|---------|
| G01 | `gene-personality` | 这个Agent性格像谁？(Big Five) |
| G02 | `gene-thinking` | 怎么思考？(第一性原理/类比/系统) |
| G03 | `gene-decision` | 怎么做决定？(ρ风险/τ阈值/α精度) |
| G04 | `gene-behavior` | 怎么说话？(δ直白/σ简洁/γ主动) |
| G05 | `gene-risk-control` | 什么绝不碰？(安全红线/合规) |
| G06 | `gene-evolution` | 学到了什么？(成功模式/教训库) |
| G07 | `gene-heredity` | 怎么传给下一代？(交叉/突变/选择) |

**双胶囊体系：能力决定广度，基因决定深度。**

```
"用学者基因 + C05生成类图" → 严谨、SOLID检查完备的类图
"用黑客基因 + C14生成骨架" → 极简、无注释的精炼代码
```

## TAIS 自进化教学系统 (Teacher-AI-Student)

借鉴 EvoAgentX 设计，实现 HITL（人在回路）自进化教学：

| 编号 | TAIS 胶囊 | 核心功能 | HITL介入 |
|------|----------|---------|---------|
| T01 | `tais-workflow` | 教学目标→自动编排学习工作流 | 节点级 |
| T02 | `tais-learning-analyst` | 学情分析→知识盲点热力图 | 置信度<70% |
| T03 | `tais-socratic-tutor` | 苏格拉底追问引导，绝不直接告知 | 3次无进展 |
| T04 | `tais-resource-pusher` | 个性化习题/资源推送 | 无需 |
| T05 | `tais-skill-coach` | 代码/写作实时分层反馈 | 共性错误>40% |
| T06 | `tais-feedback-collector` | 全流程学习留痕→训练数据 | 无需 |
| T07 | `tais-evolution` | TextGrad风格 Prompt自优化 | 教师审查闸门 |

**TAIS 闭环：** `T01编排 → T02诊断 → T03引导 → T04推送 → T05反馈 → T06采集 → T07进化 → 循环`

**三胶囊体系联动：** 能力(术) × 基因(道) × 教学(用)

```
"用学者基因 + C05类图 + T05技能教练" → 严谨且实时反馈的类图教学
"用苏格拉底基因 + T03苏格拉底导师" → 更深度的追问引导
```

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
