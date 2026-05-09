---
name: tais-evolution
description: TAIS自进化引擎 — TextGrad风格Prompt自动优化。采集教学效果数据→生成优化梯度→自动改进AI智能体的提示词。实现"部署即进化"，越教越聪明。触发词：进化、自优化、TextGrad、prompt优化、auto improve、教学进化。
---

# T07：TAIS 自进化引擎

"部署即进化"——教学智能体越用越聪明。

## 触发条件

进化、自优化、TextGrad、prompt优化、auto improve、迭代、教学进化

## 进化闭环

```yaml
cycle:
  step1_collect:
    source: tais-feedback-collector
    data: {student_outcomes, stuck_rate, hitl_escalation_rate}
    
  step2_evaluate:
    metrics:
      learning_gain: post_score - pre_score
      efficiency: successful_dialogues / total_dialogues
      autonomy: (total - hitl_escalations) / total
      
    composite_score: 0.4*learning_gain + 0.3*efficiency + 0.3*autonomy
    
  step3_diagnose:
    if composite_score < threshold:
      identify_weak_agent: 哪些智能体的提示词效果差？
      pinpoint_issue: 追问太快？推送不准？反馈太笼统？
      
  step4_optimize:
    for each weak_agent:
      generate_prompt_variant: 基于问题诊断生成改进版提示词
      A/B_test: 部分学生用新版，部分用旧版
      select_winner: 更高的composite_score胜出
      
  step5_deploy:
    update_agent_prompt: 用胜出版本的提示词替换
    log: {version, improvement, before_score, after_score}
```

## 优化示例

### 苏格拉底导师优化

```yaml
问题诊断:
  指标: 平均追问8.2轮到突破，理想值5轮
  分析: 追问跳跃太大，学生跟不上

当前提示词:
  "通过连续追问引导学生理解F=ma"

优化后提示词:
  "通过连续追问引导学生。每次只改变一个问题变量。
   先问'当质量不变时，加速度和力的关系？'
   再问'当力不变时，加速度和质量的关系？'
   最后合起来问'力和质量同时变化时呢？'
   如果学生卡住，退回到单变量问题。"

效果: 平均追问降至5.5轮，突破率提升23%
```

### 资源推送员优化

```yaml
问题诊断:
  指标: 资源点击率仅42%
  分析: 推送太多，学生选择困难

当前提示词:
  "推送5个匹配资源"

优化后提示词:
  "每次只推送最匹配的2个资源。
   第一个：可视化/视频（降低认知负荷）
   第二个：交互练习（即时检验）
   不要推送第3个，除非学生主动请求更多。"

效果: 点击率提升至71%
```

## 进化日志

```yaml
evolution_log:
  - version: v1.0
    date: 2026-05-01
    changes: 初始部署
    score: 0.68
    
  - version: v1.1
    date: 2026-05-15
    changes: Socratic Tutor Prompt优化
    improvement: +12%
    score: 0.76
    
  - version: v1.2
    date: 2026-06-01
    changes: Resource Pusher 限流+ HITL阈值调整
    improvement: +8%
    score: 0.82
```

## 教师审查

```yaml
review_gate:
  所有优化结果在部署前需教师审查：
    - 优化后的Prompt是否符合教学原则？
    - 是否引入了不恰当的引导方式？
    - 是否需要微调或回滚？
  
  教师可以：✅批准 / ✏️修改 / ❌驳回
```
