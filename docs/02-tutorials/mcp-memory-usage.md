# MCP Memory：Agent 间自动共享上下文

> 配置完成后，不再需要在 Agent 对话之间手动复制粘贴输出。

---

## 为什么需要 MCP Memory

**没有 MCP Memory 的工作流（手动）**：
```
产品经理输出 PRD
  ↓ 你：选中全文 → Ctrl+C → 开新对话 → 粘贴
架构师读到 PRD，设计架构
  ↓ 你：选中全文 → Ctrl+C → 开新对话 → 粘贴
安全工程师审查架构
```

**有了 MCP Memory（自动）**：
```
产品经理输出 PRD → 存入 memory（标签：DevCoach, prd）
  ↓
架构师：「从 memory 调取 DevCoach PRD」→ 自动获取
  ↓
安全工程师：「从 memory 调取 DevCoach 架构」→ 自动获取
```

---

## 配置方法（已完成）

`.vscode/mcp.json` 文件已就位：

```json
{
  "servers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

VS Code 会在打开工作区时自动启动 Memory 服务器，Copilot Chat Agent 模式中将可用 `remember`、`recall`、`search`、`rollback` 工具。

---

## 使用模式

### 模式 1：Agent 保存输出

在每个 Agent 对话结束时，加上：
```
请将本次输出保存到 memory，使用以下标签：
- 项目：DevCoach
- 阶段：market-analysis（或 prd / architecture / sprint-plan）
- 日期：今天日期
包含完整内容，不要摘要。
```

### 模式 2：下一个 Agent 调取

在下一个 Agent 对话开始时：
```
激活 软件架构师

请先从 memory 中调取标签包含 DevCoach 的内容（PRD 和市场分析），
然后基于这些内容设计 DevCoach 的技术架构。
```

### 模式 3：回滚（当结果不理想时）

```
这个架构方案评估后我们不满意，请回滚到上一个存档点，
重新基于简化版本的 PRD 设计。
```

---

## 标签规范（保持一致才能可靠调取）

| 保存时机 | 建议标签 |
|----------|---------|
| 市场分析完成 | `DevCoach, market-analysis, step-01` |
| PRD 完成 | `DevCoach, prd, step-02` |
| 技术架构完成 | `DevCoach, architecture, step-03` |
| Sprint 计划完成 | `DevCoach, sprint-plan, step-04` |
| 安全审查完成 | `DevCoach, security-review, step-05` |
| 营销文案（微信） | `DevCoach, marketing, wechat` |
| 营销文案（抖音） | `DevCoach, marketing, douyin` |

---

## 注意事项

- **当前版本**：会话内持久（关闭 VS Code 后清空）
- **跨会话持久化**：需要升级为文件存储版本（`@modelcontextprotocol/server-filesystem` 配合 memory 插件），或使用 SQLite 后端
- **内存大小**：存储大量内容时可能超出 LLM 上下文窗口，建议每次调取时指定标签而不是「全部调取」

---

## 完整工作流（配合 MCP Memory）

参考：[agency-agents/examples/workflow-with-memory.md](../../agency-agents/examples/workflow-with-memory.md)

这是官方提供的、带 MCP Memory 的完整 MVP 开发工作流模板，可以直接套用到 DevCoach 项目。
