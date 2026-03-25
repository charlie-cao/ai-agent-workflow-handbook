# 高级用法：用 OpenClaw 平台指挥多 Agent 协作

> 本节是高级内容，适合在熟悉单 Agent 和流水线工作流之后阅读。

---

## 核心概念

到目前为止，我们的工作流是：
```
你（人）→ 激活 Agent A → 复制输出 → 激活 Agent B → ...
```

**进阶模式**（OpenClaw/自建编排层）：
```
OpenClaw → 自动调用 Agent A
          → 等待完成 → 自动调用 Agent B（传入 A 的输出）
          → 等待完成 → 自动调用 Agent C
          → 汇总输出 → 发送给你
```

你从"中间人"变成了"发起者"，其余全部自动。

---

## 两类 Agent 文件的分工

本工程中有两套 Agent 文件库：

| 文件库 | 用途 | 格式 |
|--------|------|------|
| `agency-agents/` | **开发辅助** — 在 Copilot Chat 里激活，帮你设计、审查、规划 | `.md` + YAML |
| `awesome-openclaw-agents/agents/` | **产品功能** — 部署为可调用的 API Agent，成为产品本身 | `SOUL.md` |

### DevCoach 的具体对应

| DevCoach 功能 | 对应的 SOUL.md | 位置 |
|--------------|---------------|------|
| AI 代码诊断（核心功能） | `Lens - Code Reviewer` | `agents/development/code-reviewer/SOUL.md` |
| AI 导师对话 | `Tutor` | `agents/education/tutor/SOUL.md` |
| 学习路径规划 | `Study Planner` | `agents/education/study-planner/SOUL.md` |
| 代码测试用例生成 | `Test Writer` | `agents/development/test-writer/SOUL.md` |

---

## 编排层的作用

```
用户提交代码（DevCoach 前端）
  ↓
编排层（OpenClaw 或自建）
  ↓ 并行触发
  ├── Code Reviewer Agent → 安全漏洞 + 代码质量报告
  ├── Tutor Agent        → 根据代码水平生成讲解
  └── Study Planner Agent → 推荐下一步学习路径
  ↓ 汇总
  返回给用户：[质量报告 + 解释 + 学习建议] 三合一
```

---

## 如何实现（三种方式）

### 方式 A：OpenClaw 平台（最快，不需要写代码）
1. 在 [crewclaw.com](https://crewclaw.com) 注册账号
2. 上传 SOUL.md 文件
3. 配置 Agent 编排流程
4. 获得 API 端点，DevCoach 后端直接调用

**适合**：快速验证 MVP，团队小、不想维护基础设施

### 方式 B：用 OpenAI/Claude API 自行实现
1. 把 SOUL.md 的内容作为 `system prompt`
2. 用后端代码调用多个 API，串联输出
3. 不依赖任何平台，完全自主

**适合**：有技术积累，想要完全控制

### 方式 C：LangGraph / CrewAI / AutoGen（开源编排框架）
1. 用框架定义 Agent 图和执行顺序
2. SOUL.md 内容作为 Agent 描述注入
3. 部署到自己的服务器或云函数

**适合**：团队有 Python/AI 能力，需要复杂工作流

---

## 当前建议（DevCoach MVP 阶段）

**不要现在做这些**——MVP 阶段先验证用户价值。

MVP 阶段的 AI 调用直接用简单的 API 调用就够了：

```python
# 极简实现：一次 API 调用，把 SOUL.md 作为 system prompt
import anthropic

with open("code-reviewer/SOUL.md") as f:
    soul = f.read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-opus-4-5",
    system=soul,
    messages=[{"role": "user", "content": f"请帮我审查这段代码：\n{user_code}"}]
)
```

等 MVP 验证后，再升级为多 Agent 编排。

---

## 参考资料

- [awesome-openclaw-agents README](../../awesome-openclaw-agents/README.md)
- [agency-agents/examples/workflow-with-memory.md](../../agency-agents/examples/workflow-with-memory.md) — 多 Agent 协作示例
- [agency-agents/specialized/agents-orchestrator.md](../../agency-agents/specialized/agents-orchestrator.md) — 「多智能体编排师」Agent，可用于规划编排方案
