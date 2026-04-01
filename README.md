# Everything Claude Code 中文整理版

> 说明：这是中文整理版入口文档，用于帮助中文读者快速理解仓库结构与使用方式。英文原文保持不变。

## 这是什么

`everything-claude-code` 是一套围绕 Claude Code 的完整配置资产集合，包含：

- `agents`：面向不同任务的专用子代理
- `skills`：可复用的方法论与工作流知识
- `commands`：以 `/command` 形式触发的快捷工作流
- `rules`：始终生效的规则与工程约束
- `hooks`：在工具执行前后或会话生命周期触发的自动化逻辑
- `contexts`：按开发、研究、审查等模式注入的上下文
- `mcp-configs`：常见 MCP 服务配置样板

这不是一个可直接运行的业务应用仓库，而是一个用于构建 AI 开发工作系统的“配置仓库”。

## 推荐阅读顺序

原仓库作者建议先看两篇配套指南：

1. Shorthand Guide：理解每种配置类型的职责、整体组织方式、上下文管理和基本理念
2. Longform Guide：理解高级主题，例如 token 优化、session memory、验证闭环、并行化与持续学习

## 仓库结构

```text
everything-claude-code/
├── agents/            # 专项子代理
├── commands/          # Slash commands
├── contexts/          # 模式化上下文
├── examples/          # 示例配置与 session 文件
├── hooks/             # Hook 配置与脚本
├── mcp-configs/       # MCP 服务配置样板
├── plugins/           # 插件与 marketplace 说明
├── rules/             # 全局规则
└── skills/            # 技能与工作流知识
```

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/affaan-m/everything-claude-code.git
```

### 2. 拷贝你需要的配置到 `~/.claude/`

```bash
# agents
cp everything-claude-code/agents/*.md ~/.claude/agents/

# rules
cp everything-claude-code/rules/*.md ~/.claude/rules/

# commands
cp everything-claude-code/commands/*.md ~/.claude/commands/

# 技能
cp -r everything-claude-code/skills/* ~/.claude/skills/
```

### 3. 配置 hooks

把 [hooks/hooks.json](/Users/zhanyu/projects/everything-claude-code/hooks/hooks.json) 中的配置合并到你的 `~/.claude/settings.json`。

### 4. 配置 MCP

把 [mcp-configs/mcp-servers.json](/Users/zhanyu/projects/everything-claude-code/mcp-configs/mcp-servers.json) 中你需要的服务复制到 `~/.claude.json`。

注意：

- 把 `YOUR_*_HERE` 占位符替换成真实配置
- 不要一次启用过多 MCP，避免显著压缩可用上下文窗口

## 核心概念

### Agents

`agents/` 下的文件是专用角色提示词，例如：

- `planner`：复杂需求的实施规划
- `architect`：架构设计与取舍分析
- `tdd-guide`：测试先行工作流
- `code-reviewer`：代码质量与安全审查
- `security-reviewer`：漏洞发现与修复建议
- `e2e-runner`：Playwright 端到端测试

### 技能

`skills/` 下的内容是可复用的知识与方法论，例如：

- `coding-standards`
- `frontend-patterns`
- `backend-patterns`
- `tdd-workflow`
- `security-review`
- `continuous-learning`

这些技能可以被命令、代理或宿主自动触发。

### Commands

`commands/` 是便于直接调用的工作流入口，例如：

- `/plan`：先出计划，等确认后再实施
- `/tdd`：按 TDD 节奏实施需求
- `/e2e`：生成并运行 Playwright 测试
- `/code-review`：对未提交改动做系统审查
- `/build-fix`：逐步修复构建错误

### Rules

`rules/` 定义常驻工程约束，覆盖：

- 安全
- 编码风格
- 测试要求
- Git 工作流
- 代理编排
- 性能与上下文管理

### Hooks

`hooks/` 用于在关键时机自动运行脚本，例如：

- 在启动 dev server 前阻止非 tmux 运行
- 编辑 TS/JS 文件后自动格式化
- 结束会话时检查 `console.log`
- 在 session 启动、压缩、结束时做记忆持久化

## 适合谁

这套配置更适合：

- 重度使用 Claude Code 的开发者
- 需要稳定工程流程的 solo builder
- 想把 prompt、规则、工作流、MCP、记忆机制系统化的人

如果你刚开始使用，建议只先拷贝：

- `rules/`
- `commands/plan.md`
- `commands/tdd.md`
- `skills/tdd-workflow/`
- `skills/security-review/`

后续再逐步增加 agents、hooks 和 MCP。

## 使用建议

- 不要一次性启用全部配置，先用最能解决你当前问题的部分
- 先建立自己的基础规则，再慢慢引入高级自动化
- 将这套仓库视为骨架，而不是必须原样照搬的成品

## 相关文件

- 英文原文：[README.md](/Users/zhanyu/projects/everything-claude-code/README.md)
- 贡献说明：[CONTRIBUTING.md](/Users/zhanyu/projects/everything-claude-code/CONTRIBUTING.md)

