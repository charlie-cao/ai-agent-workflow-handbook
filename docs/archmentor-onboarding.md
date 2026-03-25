# ArchMentor Onboarding 流程设计

> **版本**: v1.0 | **日期**: 2026-03-25  
> **北极星指标**: 用户完成第一次 5 轮以上 AI 架构对话 = Activated  
> **目标转化率**: 注册→激活 > 60%

---

## 流程总览（5步，目标 < 3 分钟）

```
[访客落地页]
     ↓
[Step 1: GitHub 一键登录]       ← 30秒
     ↓
[Step 2: 技术栈选择]             ← 20秒（让 AI 提前了解你）
     ↓
[Step 3: AI 直接推荐第一个场景]  ← 10秒（无选择困难）
     ↓
[Step 4: AI 开场 + 用户第一次回答]
     ↓
  （对话循环 5-8 轮）
     ↓
[Step 5: 设计报告 + 激活完成]   ← 情感高潮设计
     ↓
[付费引导]
```

---

## 每步详细设计

### Step 1：GitHub OAuth 登录

**UI 要求**:
- 只有一个主按钮："Continue with GitHub"
- 文案："No password needed. 30 seconds to your first session."
- 技术：NextAuth.js GitHub Provider

**禁止**:
- ❌ 要求填写姓名/公司/邮箱（摩擦过高）
- ❌ 展示注册表单
- ❌ 邮件验证步骤

---

### Step 2：技术栈选择（10秒完成）

**标题**: "What's your primary stack?"

**方式**: 多选 Chip/Tag，不是下拉框

**选项**:
```
[Python] [Java] [Go] [Node.js] [Rust] [Other]
[Backend] [Full-Stack] [Infrastructure] [Data Engineering]
```

**CTA**: "Let's Go →"（选 1 个即可继续）

**存储用途**: 根据技术栈智能推荐第一个场景
- 选 Python + Backend → 推荐 S-07 消息队列
- 选 Node.js + Full-Stack → 推荐 S-01 URL短链接
- 选 Infrastructure → 推荐 S-03 速率限制器

---

### Step 3：直接推荐第一个场景（零选择困难）

**设计原则**: 引导选择 > 自由选择（留存率高约 30%）

**UI 示例**:
```
┌─────────────────────────────────────────┐
│ 🎯 Your First Session                   │
│                                         │
│ Based on your backend focus, we've      │
│ picked a great starting design:         │
│                                         │
│ Design a URL Shortener                  │
│ (Used by: 87% of backend engineers)     │
│ ⏱ Estimated: 15-20 minutes             │
│                                         │
│ [Start Session →]    [Browse All 20]    │
└─────────────────────────────────────────┘
```

---

### Step 4：第一次 AI 对话（关键体验时刻）

**首次用户专用开场语**（比常规更温和）:
```
"Welcome! I'm your AI architecture mentor. My job is NOT to 
give you answers — it's to ask you the right questions so you 
build your own intuition.

Let's design a URL Shortener. Before anything else: 
What do you think are the top 2 challenges in building 
this system at scale?

Take your time — there's no wrong answer here."
```

**设计原则**:
- 第一轮有意降低门槛（"no wrong answer"）→ 减少因怕答错而离开
- 进度条显示 "Round 1 of 5"（减少不确定感）
- AI 第一轮回应要**肯定用户输入**，再追问（增强自信心）

---

### Step 5：激活完成 — 设计报告（情感高潮设计）

**触发时机**: 5 轮对话后，或用户输入 `/report`

**UI 设计要求**（制造"成就感时刻"）:
```
┌─────────────────────────────────────────────┐
│ 🎉 Session Complete!                         │
│                                             │
│ You just completed your first ArchMentor    │
│ design session.                             │
│                                             │
│ YOUR DESIGN ASSESSMENT                      │
│                                             │
│ ✅ Strengths                                │
│ • Strong requirement clarification          │
│ • Recognized the read-heavy workload        │
│                                             │
│ 🔧 Growth Areas                             │
│ • Cache invalidation strategy (not covered) │
│ • Hot key problem for viral URLs            │
│                                             │
│ 🎯 Skill Snapshot (First Session)           │
│ Scalability      ████░░ 65/100              │
│ Availability     ███░░░ 55/100              │
│                                             │
│ You have 2 free sessions remaining          │
│ [Continue Free]  [Unlock Unlimited - $39/mo]│
└─────────────────────────────────────────────┘
```

**关键设计**:
- 付费引导**在报告下方**，不在进入时推
- 报告里不推"去学这门课"，而是"下次练这个场景会补足这个弱点"
- 提供"分享你的设计评分"功能（社交分享裂变触点）

---

## 关键转化原则

| 原则 | 实现方式 | 预期效果 |
|------|---------|---------|
| **免摩擦开始** | GitHub OAuth，无表单，< 30秒到第一个问题 | 注册完成率 > 85% |
| **消除选择困难** | AI 直接推荐场景，不让用户选 | Step 3→4 转化 > 90% |
| **进度可见** | "Round 2 of 5" 进度条 | 对话完成率 > 70% |
| **情感峰值设计** | 报告页面是最强体验时刻 | D7 留存 > 45% |
| **精准付费时机** | 报告看完后才出现付费引导 | Free→Paid 转化 > 12% |

---

## 邮件 Nurture 序列

### Day 0（注册后立即）
```
Subject: Your first system design session 👇

You just completed your first ArchMentor session.

Here's what stood out: [personalized 1-line from report]

One quick question: what made you try ArchMentor today?
[Preparing for an interview] [Leveling up for promotion] [Just curious]
```

### Day 3（如果未回来）
```
Subject: The gap we revealed in your first session

Your design had a strength most engineers miss, but also 
one gap that trips up 73% of engineers at the Senior level.

Here's the specific scenario that builds that skill in 
exactly 15 minutes: [direct link]
```

### Day 7（Free Tier 用完）
```
Subject: You've used all 3 free sessions

[用户姓名], you've completed 3 design sessions.

Your skills have moved from [Session 1 score] → [Session 3 score].
That's real progress.

Here's what's waiting for you in the next session: [场景预览]

[Unlock Unlimited Access - $39/month]
[Or $199/year (save 57%)]
```

---

## A/B 测试计划

| 测试项 | A 版本 | B 版本 | 指标 |
|--------|--------|--------|------|
| 开场语语气 | 温和引导（当前版本） | 更有挑战性（"Let's see if you can handle this..."） | 对话完成率 |
| Step 2 位置 | 注册后询问技术栈 | 第 3 次对话后再询问 | Step 2 完成率 |
| 报告触发时机 | 固定 5 轮后 | 用户主动点击 "Get Report" | 对话轮次 + 满意度 |
| 付费引导文案 | ROI 导向（"save $40k/year"） | 情感导向（"Join 2,000 engineers leveling up"） | 付费转化率 |

---

*Onboarding 设计 v1.0 — 需配合前端工程师实现后进行用户测试*
