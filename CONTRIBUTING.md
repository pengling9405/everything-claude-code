# Contributing to Everything Claude Code 中文整理版

> 说明：这是贡献说明的中文整理版，方便中文读者快速了解参与方式。英文原文保持不变。

## 仓库欢迎哪些贡献

### Agents

适合补充新的专用代理，例如：

- 语言专项 reviewer，如 Python、Go、Rust
- 框架专项专家，如 Django、Rails、Laravel、Spring
- DevOps 专家，如 Kubernetes、Terraform、CI/CD
- 领域专家，如 ML pipeline、数据工程、移动开发

### 技能

适合补充新的技能与方法论，例如：

- 语言最佳实践
- 框架模式
- 测试策略
- 架构指南
- 领域知识工作流

### Commands

适合补充新的 slash command，例如：

- 部署命令
- 测试命令
- 文档命令
- 代码生成命令

### Hooks

适合补充新的自动化逻辑，例如：

- lint / format hooks
- 安全检查 hooks
- 参数验证 hooks
- 通知 hooks

### Rules

适合补充始终生效的规则，例如：

- 安全规范
- 代码风格
- 测试要求
- 命名规范

### MCP Configurations

适合补充新的 MCP 服务配置，例如：

- 数据库集成
- 云平台集成
- 监控工具
- 协作工具

## 如何贡献

### 1. Fork 仓库

```bash
git clone https://github.com/YOUR_USERNAME/everything-claude-code.git
cd everything-claude-code
```

### 2. 创建分支

```bash
git checkout -b add-python-reviewer
```

### 3. 把内容放到对应目录

- 新 agent 放到 `agents/`
- 新 skill 放到 `skills/`
- 新 command 放到 `commands/`
- 新 rule 放到 `rules/`
- 新 hook 放到 `hooks/`
- 新 MCP 配置放到 `mcp-configs/`

### 4. 遵循既有格式

#### Agents

需要使用 frontmatter：

```markdown
---
name: agent-name
description: What it does
tools: Read, Grep, Glob, Bash
model: sonnet
---

Instructions here...
```

#### 技能

建议清晰说明：

- 什么时候用
- 如何工作
- 例子

#### Commands

建议清晰说明：

- 命令做什么
- 什么时候用
- 触发后会执行什么流程

#### Hooks

建议包含：

- matcher
- hooks
- description

### 5. 提交前先自己验证

请先确认你的配置在 Claude Code 中可以正常工作，再提交 PR。

### 6. 提交 Pull Request

```bash
git add .
git commit -m "Add Python code reviewer agent"
git push origin add-python-reviewer
```

PR 建议包含：

- 你新增了什么
- 为什么它有价值
- 你如何验证它可用

## 建议

### Do

- 保持配置聚焦、模块化
- 写清楚描述
- 提交前测试
- 遵循现有风格
- 说明依赖项

### Don't

- 不要提交敏感信息，如 API key、token、本机私有路径
- 不要提交过于复杂或过于小众的配置
- 不要提交未测试内容
- 不要做重复功能
- 不要强绑定必须付费服务且没有替代方案的配置

## 文件命名建议

- 使用小写加连字符，例如 `python-reviewer.md`
- 名字尽量描述清晰
- 文件名尽量与 agent / skill 名称一致

## 相关文件

- 英文原文：[CONTRIBUTING.md](/Users/zhanyu/projects/everything-claude-code/CONTRIBUTING.md)
- 仓库入口：[README.md](/Users/zhanyu/projects/everything-claude-code/README.md)

