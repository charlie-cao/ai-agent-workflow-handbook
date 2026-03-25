# 🤖 AI Agent 工作流实战手册

> 用 144 个 AI 专家 Agent + GitHub Copilot，从零构建 SaaS 产品的完整实战记录

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agents](https://img.shields.io/badge/agents-144-blueviolet)]()
[![VS Code](https://img.shields.io/badge/VS%20Code-Agent%20Mode-blue)]()

---

## 这是什么

这个仓库记录了一个真实的使用过程：

用 **[agency-agents](https://github.com/msitarzewski/agency-agents)** 项目的 144 个专家 AI 角色，结合 **GitHub Copilot Agent 模式**，构建一个面向程序员的 AI 学习 SaaS（DevCoach）。

整个过程从安装配置到市场研究、产品定义、技术架构、营销内容，每一步都用对应的 Agent 来完成，并记录在 `docs/` 目录下。

**你可以把这个仓库当作一个模板：** 复制它，替换项目名称，用同样的工作流构建你自己的产品。

---

## 核心工具链

```
agency-agents           // 144 个专家角色 (.md 文件)
    ↕
GitHub Copilot          // 在 VS Code Agent 模式中激活这些角色
    ↕
MCP Memory              // Agent 之间自动共享上下文（可选）
    ↕
awesome-openclaw-agents // 产品本身的 AI 功能模板 (SOUL.md)
    ↕
OpenClaw/自建 API       // 生产环境部署 Agent（高级）
```

---

## 快速开始（5 分钟）

### 1. 安装 Agents 到 GitHub Copilot

```powershell
# 克隆 agency-agents
git clone https://github.com/msitarzewski/agency-agents
cd agency-agents

# 安装到 Copilot（Windows PowerShell）
$src = "$(Get-Location)"
$destGithub = "$env:USERPROFILE\.github\agents"
$destCopilot = "$env:USERPROFILE\.copilot\agents"
New-Item -ItemType Directory -Force -Path $destGithub, $destCopilot | Out-Null

@("engineering","design","marketing","product","project-management",
  "testing","support","sales","paid-media","spatial-computing",
  "specialized","academic","strategy","game-development") | ForEach-Object {
  Get-ChildItem "$src\$_\*.md" -EA SilentlyContinue |
    ForEach-Object { Copy-Item $_.FullName $destGithub -Force; Copy-Item $_.FullName $destCopilot -Force }
}
Write-Host "Done!"
```

### 2. 配置 VS Code

在 `.vscode/settings.json` 中添加：

```json
{
  "chat.agent.enabled": true,
  "github.copilot.chat.agent.runTasks": true,
  "github.copilot.chat.codesearch.enabled": true
}
```

### 3. 配置 MCP Memory（让 Agent 自动共享上下文）

创建 `.vscode/mcp.json`：

```json
{
  "servers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "type": "stdio"
    }
  }
}
```

### 4. 重启 VS Code，打开 Copilot Chat，切换到 Agent 模式

从下拉列表选择 Agent，开始工作。

---

## Agent 是什么，能做什么

**Agent = 专家角色**。选择「安全工程师」，AI 就进入安全专家模式，主动用威胁建模视角看问题，输出 OWASP 格式报告。选择「产品经理」，AI 就输出 PRD 格式，自动追问用户故事和验收标准。

**不是换了一个 AI，而是给同一个 AI 换了不同的工作手册。**

```
普通对话：AI 回答你的问题
Agent 模式：AI 扮演专家角色 + 主动搜索/读写文件/运行命令 + 按专业格式输出
```

### 三种使用模式

| 模式 | 适合场景 | 示例 |
|------|----------|------|
| **单 Agent** | 完成一个具体任务 | 激活「代码审查工程师」审查一段代码 |
| **流水线** | 功能开发全流程 | 架构师出方案 → 安全工程师审查 → 技术文档工程师写文档 |
| **并行** | 产品立项 | 同时跑：市场研究 + 技术架构 + 增长方案 |

---

## 分工目录（144 个 Agent）

| 部门 | 代表 Agent | 典型用途 |
|------|-----------|----------|
| 💻 工程（23 个）| 软件架构师、安全工程师、代码审查工程师 | 设计、实现、审查 |
| 🎨 设计（8 个）| UI 设计师、用户体验架构师 | 界面设计、用户研究 |
| 📢 营销（25 个）| 增长黑客、抖音策略师、小红书专家 | 内容、投放、增长 |
| 📊 产品（5 个）| 产品经理、Sprint 规划师 | 需求、路线图 |
| 🧪 测试（8 个）| 生产就绪验证员、API 测试工程师 | QA、上线把关 |
| 🎯 特化（25 个）| 多智能体编排师、MCP 构建专家 | 复杂系统、AI 工具链 |

---

## 实战案例：DevCoach SaaS 构建过程

本仓库同时记录了用这套工具链构建「DevCoach」（面向程序员的 AI 学习 SaaS）的完整过程。

### 已完成步骤

| 步骤 | Agent | 文档 |
|------|-------|------|
| ✅ 市场分析 | 市场趋势研究员 | [step-01-market-analysis.md](docs/03-devcoach/step-01-market-analysis.md) |
| ⏳ 产品定义 | 产品经理 | 进行中 |
| ⏳ 技术架构 | 软件架构师 + 安全工程师 | 待开始 |
| ⏳ Sprint 规划 | Sprint 优先级规划师 | 待开始 |

### 市场分析摘要

> 目标用户：工作 1-5 年的后端/全栈工程师  
> 核心痛点：写了大量代码，但不知道自己的能力盲区在哪里  
> 差异化方向：实战代码诊断（提交代码 → AI 分析能力盲区，而不只是 Review）  
> 定价：¥0 / ¥39-59 / ¥99 每月

---

## Agent vs Skill vs MCP — 三者的关系

```
Agent  = 专家角色    （你是谁，用什么视角看问题）
Skill  = 工作手册    （按什么流程做，输出什么格式）
MCP    = 外部工具    （能用什么工具：搜索/记忆/GitHub等）
```

**组合效果：** 「安全工程师」+ DevCoach 工作流规范 + MCP GitHub 工具 = AI 以安全专家视角、按项目规范、直接读取 PR 代码做审查。

---

## 高级用法：用 OpenClaw 部署产品级 Agent

agency-agents 用于**开发阶段**辅助，真正运行在产品里的 AI 功能使用 `awesome-openclaw-agents` 中的 SOUL.md 模板：

```python
# MVP 最简实现：把 SOUL.md 作为 system prompt 调用
import anthropic

with open("code-reviewer/SOUL.md") as f:
    soul = f.read()

response = client.messages.create(
    model="claude-opus-4-5",
    system=soul,
    messages=[{"role": "user", "content": f"请审查这段代码：\n{user_code}"}]
)
```

等产品规模增长后，再升级为 OpenClaw/LangGraph 等编排框架，实现多 Agent 并行自动化。

---

## 文档目录

```
docs/
├── README.md                              — 教程总目录
├── 01-setup/
│   └── agency-agents-guide.md            — 完整安装指南
├── 02-tutorials/
│   ├── what-is-agency-agents.md          — 概念入门
│   ├── mcp-memory-usage.md               — MCP Memory 用法
│   └── advanced-agent-orchestration.md   — 高级编排
└── 03-devcoach/
    └── step-01-market-analysis.md        — 市场分析（已完成）
```

---

## 这套工具链适合谁

- ✅ 想用 AI 加速产品开发的**独立开发者/小团队**
- ✅ 想建立**系统化 AI 工作流**的工程师
- ✅ 想用 AI 做**营销内容**（抖音/小红书/微信/X）同时兼顾产品开发的创始人
- ❌ 不适合：只想写代码不想做产品的纯工程师

---

## 参考资源

- [agency-agents 原始仓库](https://github.com/msitarzewski/agency-agents)
- [awesome-openclaw-agents](https://github.com/mergisi/awesome-openclaw-agents)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [VS Code Copilot Agent 文档](https://code.visualstudio.com/docs/copilot/copilot-chat)
