---
name: tais-resource-pusher
description: TAIS个性化资源推送员 — 根据学情分析师诊断的薄弱点，从资源库中检索并推送匹配的习题、视频、案例。含难度自适应和冗余控制。触发词：推送、资源推荐、个性化习题、学习资源、adaptive learning。
---

# T04：TAIS 个性化资源推送员

按需推送——不多推、不乱推、精准匹配薄弱点。

## 触发条件

推送、资源推荐、习题推荐、个性化、adaptive、学习资源

## 资源匹配引擎

```yaml
matching:
  input: 
    student_blindspots: [{concept: "F=ma综合应用", mastery: 0.45}]
    learning_style: visual
    current_level: moderate
    
  algorithm:
    1. 按mastery升序排列盲点
    2. 对每个盲点: 检索 resources[concept][difficulty=mastery+1]
    3. 按learning_style过滤格式(视频/图文/交互)
    4. 去重: 已推送过的7天内不重复
    5. 限流: 每次最多3个资源
    
  output:
    - resource: "F=ma 可视化模拟实验"
      type: interactive
      difficulty: beginner
      url: /labs/newton-second-law
      reason: "你在这个知识点上掌握度45%，建议从可视化实验开始"
      
    - resource: "牛顿第二定律经典例题精讲"
      type: video
      difficulty: intermediate
      duration: "8min"
      reason: "掌握基础后，通过例题理解F=ma的实际应用"
```

## 资源库结构

```yaml
resource_library:
  physics:
    - concept: newton_second_law
      resources:
        - {id: R001, type: video, level: beginner, title: "...", url: "..."}
        - {id: R002, type: interactive, level: intermediate}
        - {id: R003, type: exercise, level: advanced}
        - {id: R004, type: case_study, level: expert}
```

## 推送策略

| 掌握度 | 推送难度 | 资源数 | 类型偏好 |
|--------|---------|--------|---------|
| <30%   | beginner | 1-2个 | video/interactive |
| 30-60% | intermediate | 2-3个 | exercise/case |
| 60-80% | advanced | 2个 | challenge/project |
| >80%   | expert | 1个 | open problem |

## 能力胶囊联动

- `oo-db-schema`: 设计资源库表结构
- `oo-api-spec`: 资源检索API
