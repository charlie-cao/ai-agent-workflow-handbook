# AI Agent 工作流教程 — 从零到 SaaS 产品

> 本教程是一个**活文档**，记录了真实使用 agency-agents + GitHub Copilot 构建 SaaS 产品的完整过程。
> 随着项目推进持续更新。

---

## 目录

### 第一部分：工具安装与配置
1. [安装 agency-agents 到 GitHub Copilot](./01-setup/agency-agents-guide.md)

### 第二部分：使用教程（由实战总结）
2. [Agency Agents 是什么，能做什么](./02-tutorials/what-is-agency-agents.md)
3. [单 Agent 使用指南](./02-tutorials/single-agent-usage.md)
4. [多 Agent 流水线工作流](./02-tutorials/pipeline-workflow.md)
5. [并行工作流（多窗口同时启动）](./02-tutorials/parallel-workflow.md)
6. [MCP Memory：Agent 间自动共享上下文](./02-tutorials/mcp-memory-usage.md)
7. [高级用法：用 OpenClaw 指挥多 Agent 协作（自动化编排）](./02-tutorials/advanced-agent-orchestration.md)

### 第三部分：实战案例 — DevCoach SaaS 构建过程
6. [Step 1：市场分析（市场趋势研究员）](./03-devcoach/step-01-market-analysis.md)
7. [Step 2：产品定义（产品经理）](./03-devcoach/step-02-product-definition.md) *(待更新)*
8. [Step 3：技术架构（后端架构师）](./03-devcoach/step-03-tech-architecture.md) *(待更新)*
9. [Step 4：Sprint 规划（Sprint 优先级规划师）](./03-devcoach/step-04-sprint-plan.md) *(待更新)*
10. [Step 5：安全审查（安全工程师）](./03-devcoach/step-05-security-review.md) *(待更新)*

### 第四部分：营销内容
11. [营销文案工作台](./04-marketing/README.md)
12. [微信公众号/朋友圈文案](./04-marketing/wechat/)
13. [抖音脚本](./04-marketing/douyin/)
14. [小红书内容](./04-marketing/xiaohongshu/)
15. [Facebook 广告](./04-marketing/facebook/)

---

## 如何阅读本教程

本教程有两种读法：

**A. 学习读法（了解怎么用 Agency Agents）**
> 按第一部分 → 第二部分顺序阅读，理解工具链后自行应用到项目

**B. 实战跟随读法（同时构建自己的 SaaS）**
> 直接跳到第三部分，每个 Step 文档包含：
> - 背景说明（为什么这一步）
> - 激活哪个 Agent + 完整的提示词
> - 实际输出示例
> - 下一步建议

---

## 教程更新日志

| 日期 | 更新内容 |
|------|----------|
| 2026-03-25 | 初始化文档结构；完成安装配置、市场分析章节；创建 MCP Memory 教程；配置 .vscode/mcp.json |
