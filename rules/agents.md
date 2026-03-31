# Agent Orchestration

## 可用 Agents

位于 `~/.claude/agents/`：

| Agent | 用途 | 何时使用 |
|-------|------|----------|
| planner | 实施规划 | 复杂功能、重构 |
| architect | 系统设计 | 架构决策 |
| tdd-guide | 测试驱动开发 | 新功能、修 bug |
| code-reviewer | 代码审查 | 写完代码后 |
| security-reviewer | 安全分析 | 提交前 |
| build-error-resolver | 修复构建错误 | 构建失败时 |
| e2e-runner | E2E 测试 | 关键用户流程 |
| refactor-cleaner | 清理死代码 | 代码维护 |
| doc-updater | 文档更新 | 更新文档时 |

## 应立即使用 Agent 的场景

无需等待用户额外提示：
1. 复杂功能请求：使用 **planner**
2. 刚写完或修改完代码：使用 **code-reviewer**
3. 修 bug 或新功能：使用 **tdd-guide**
4. 架构决策：使用 **architect**

## 并行任务执行

对于相互独立的操作，**始终优先并行执行**：

```markdown
# GOOD: 并行执行
同时启动 3 个 agent：
1. Agent 1：分析 auth.ts 的安全问题
2. Agent 2：评审缓存系统的性能
3. Agent 3：检查 utils.ts 的类型问题

# BAD: 没必要的串行执行
先跑 agent 1，再跑 agent 2，再跑 agent 3
```

## 多视角分析

对于复杂问题，可以拆成不同角色的子代理：
- 事实核查者
- 高级工程师
- 安全专家
- 一致性审查者
- 冗余检查者
