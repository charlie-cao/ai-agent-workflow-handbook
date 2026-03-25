# Agency Agents 使用指南

> 本文档针对 **GitHub Copilot (VS Code)** 集成进行说明，介绍 agency-agents 项目的安装方式、使用方法及其为工程带来的价值。

---

## 目录

1. [项目简介](#项目简介)
2. [项目能为工程带来什么](#项目能为工程带来什么)
3. [分工目录（Agent 分类）](#分工目录agent-分类)
4. [安装方法](#安装方法)
5. [在 GitHub Copilot 中激活 Agent](#在-github-copilot-中激活-agent)
6. [VS Code 配置](#vs-code-配置)
7. [常用 Agent 速查](#常用-agent-速查)
8. [多 Agent 协作示例](#多-agent-协作示例)
9. [新增/维护 Agent](#新增维护-agent)

---

## 项目简介

[agency-agents](../agency-agents/README.md) 是一套**开箱即用的 AI 专家角色库**，每个"Agent"都是一个精心调校的 `.md` 文件，包含：

- **角色身份与个性**：独特的沟通风格、思维框架
- **核心使命与工作流**：聚焦其领域的操作步骤
- **可交付成果模板**：真实代码、文档、流程产出
- **成功评判标准**：量化指标与验收要点

项目起源于 Reddit，经过大量迭代，现已包含 **144+ 个专业 Agent**，覆盖工程、设计、产品、营销、销售等部门。

---

## 项目能为工程带来什么

| 价值维度 | 说明 |
|----------|------|
| **专业深度** | 每个 Agent 深耕单一领域（如安全工程、数据库优化），而非通用的泛泛回答 |
| **一致性** | 团队成员激活同一 Agent，获得风格与深度一致的辅助 |
| **效率提升** | 无需反复描述背景，Agent 已内置领域 context 和工作流 |
| **多角色协作** | 可在同一个项目中切换不同 Agent，覆盖全链路（前端 → 后端 → 安全审查 → 技术文档） |
| **可扩展** | 任何人可按规范新增 Agent，沉淀团队知识 |
| **零学习成本** | 在 Copilot Chat 中直接@激活，无需额外工具链 |

---

## 分工目录（Agent 分类）

| 部门 | 包含 Agent 数量 | 典型用途 |
|------|----------------|----------|
| 💻 Engineering | 23 个 | 前后端开发、架构、安全、DevOps、数据库、嵌入式、智能合约等 |
| 🎨 Design | 8 个 | UI/UX 设计、品牌、视觉叙事、无障碍图像 |
| 📢 Marketing | 25 个 | 内容营销、SEO、抖音/小红书/微博等中国平台、播客 |
| 💼 Sales | 8 个 | 外呼策略、商务发现、提案、销售辅导 |
| 💰 Paid Media | 7 个 | Google/Meta 广告、程序化购买、创意策略 |
| 📊 Product | 5 个 | 需求优先级、用户反馈分析、产品经理 |
| 🎬 Project Management | 6 个 | 项目协调、Jira 工作流、Sprint 管理 |
| 🧪 Testing | 8 个 | QA、性能测试、API 测试、可访问性审计 |
| 🛟 Support | 6 个 | 客服、数据分析、财务、合规 |
| 🥽 Spatial Computing | 6 个 | XR/AR/VR、Vision Pro、WebXR |
| 🎯 Specialized | 25 个 | 多 Agent 编排、MCP 构建、区块链审计、知识管理等 |
| 🎓 Academic | 5 个 | 人类学、地理学、历史学、叙事学、心理学视角 |
| 🎮 Game Development | 多个 | Unity/Unreal/Godot/Roblox，跨引擎游戏开发 |
| ♟️ Strategy | 若干 | 战略规划与分析 |

---

## 安装方法

### 前提条件

- Windows 10/11 + VS Code
- GitHub Copilot 订阅（Individual / Business / Enterprise）
- 安装 [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) 和 [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) 扩展

### 方式一：PowerShell 手动安装（已完成）

```powershell
$src = "C:\Users\charlie\work\ai_agent_projects\agency-agents"
$destGithub = "$env:USERPROFILE\.github\agents"
$destCopilot = "$env:USERPROFILE\.copilot\agents"

New-Item -ItemType Directory -Force -Path $destGithub | Out-Null
New-Item -ItemType Directory -Force -Path $destCopilot | Out-Null

$categories = @("engineering","design","marketing","product","project-management",
                "testing","support","sales","paid-media","spatial-computing",
                "specialized","academic","strategy","game-development")

foreach ($cat in $categories) {
  Get-ChildItem "$src\$cat\*.md" -ErrorAction SilentlyContinue |
    ForEach-Object {
      Copy-Item $_.FullName $destGithub -Force
      Copy-Item $_.FullName $destCopilot -Force
    }
}
```

> ✅ 当前已安装 **144 个 agents** 到 `%USERPROFILE%\.github\agents\`

### 方式二：使用官方脚本（需 Git Bash / WSL）

```bash
cd agency-agents

# 1. 生成集成文件（首次或修改 agent 后执行）
bash scripts/convert.sh

# 2. 安装到 Copilot
bash scripts/install.sh --tool copilot --no-interactive
```

> ⚠️ 在 Windows WSL 中运行时，`$HOME` 指向 WSL 的 `/root/` 而非 Windows 用户目录，建议优先使用方式一。

### 更新 Agents

当 `agency-agents` 仓库更新后，重新运行安装脚本即可覆盖更新。

---

## 在 GitHub Copilot 中激活 Agent

安装完成后，在 VS Code 的 Copilot Chat 面板中：

### 方法 1：通过 Agent 模式选择器

1. 打开 **Copilot Chat** 面板（`Ctrl+Alt+I`）
2. 点击聊天框左侧的模式下拉菜单
3. 选择想要的 Agent（如 `Frontend Developer`）

### 方法 2：在对话中直接引用

```
激活 Frontend Developer 模式，帮我构建一个 React 组件
```

```
使用 Code Reviewer agent 审查这段代码
```

```
切换到 Security Engineer，对这个 API 做威胁建模
```

### 方法 3：工作区级别指令（`.github/copilot-instructions.md`）

在项目根目录创建 `.github/copilot-instructions.md`，写入需要持久激活的 Agent 上下文。

---

## VS Code 配置

在工作区 `.vscode/settings.json` 中启用以下设置：

```json
{
  "chat.agent.enabled": true,
  "github.copilot.chat.agent.runTasks": true,
  "github.copilot.chat.codesearch.enabled": true,
  "github.copilot.chat.search.semanticTextResults": true
}
```

| 设置项 | 说明 |
|--------|------|
| `chat.agent.enabled` | 启用 Copilot **Agent 模式**，允许 Copilot 使用工具（运行终端、读写文件等） |
| `github.copilot.chat.agent.runTasks` | 允许 Agent 自动执行 VS Code 任务 |
| `github.copilot.chat.codesearch.enabled` | 启用代码语义搜索 |
| `github.copilot.chat.search.semanticTextResults` | 启用语义文本搜索结果 |

---

## 常用 Agent 速查

### 日常开发

| 场景 | 推荐 Agent | 文件 |
|------|-----------|------|
| 前端组件开发 | Frontend Developer | `engineering-frontend-developer.md` |
| 后端 API 设计 | Backend Architect | `engineering-backend-architect.md` |
| 代码审查 | Code Reviewer | `engineering-code-reviewer.md` |
| 安全审计 | Security Engineer | `engineering-security-engineer.md` |
| 数据库优化 | Database Optimizer | `engineering-database-optimizer.md` |
| 技术文档 | Technical Writer | `engineering-technical-writer.md` |
| 软件架构设计 | Software Architect | `engineering-software-architect.md` |
| CI/CD 自动化 | DevOps Automator | `engineering-devops-automator.md` |

### 质量保障

| 场景 | 推荐 Agent | 文件 |
|------|-----------|------|
| API 接口测试 | API Tester | `testing-api-tester.md` |
| 性能基准测试 | Performance Benchmarker | `testing-performance-benchmarker.md` |
| 可访问性检查 | Accessibility Auditor | `testing-accessibility-auditor.md` |
| 生产可用性确认 | Reality Checker | `testing-reality-checker.md` |

### 项目管理

| 场景 | 推荐 Agent | 文件 |
|------|-----------|------|
| Sprint 规划 | Sprint Prioritizer | `product-sprint-prioritizer.md` |
| 项目协调 | Project Shepherd | `project-management-project-shepherd.md` |
| 需求转任务 | Senior Project Manager | `project-manager-senior.md` |

---

## 多 Agent 协作示例

一个典型的新功能开发流程：

```
1. [Software Architect]   → 评审架构方案，输出设计文档
2. [Frontend Developer]   → 实现 UI 组件
3. [Backend Architect]    → 设计 API 与数据模型
4. [Security Engineer]    → 威胁建模与安全审查
5. [Code Reviewer]        → PR 代码审查
6. [Reality Checker]      → 生产就绪确认
7. [Technical Writer]     → 生成 API 文档
```

也可参考 [workflow 示例](../agency-agents/examples/) 获取端到端工作流模板。

---

## 新增/维护 Agent

在团队中沉淀专业知识，可自定义 Agent：

```markdown
---
name: My Custom Agent
description: 一句话描述该 Agent 的专长
color: purple
emoji: 🔧
---

# My Custom Agent

## 角色定义
...

## 核心工作流
...
```

保存为 `.md` 文件，放入 `agency-agents/<category>/` 目录，然后重新运行安装脚本即可。

详细规范参见 [CONTRIBUTING.md](../agency-agents/CONTRIBUTING.md)。
