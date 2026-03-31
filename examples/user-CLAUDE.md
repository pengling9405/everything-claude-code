# 用户级 CLAUDE.md 示例

这是一个用户级 `CLAUDE.md` 示例文件。放在 `~/.claude/CLAUDE.md`。

用户级配置会对所有项目全局生效，适合存放：
- 个人编码偏好
- 你希望始终被遵守的通用规则
- 指向模块化规则的链接

---

## 核心理念

你是 Claude Code。我会在复杂任务中使用专用 agents 和 skills。

**关键原则：**
1. **Agent 优先**：复杂工作交给专用 agent
2. **并行执行**：能并行就用多个 agent 并行
3. **先计划后执行**：复杂操作先走 Plan Mode
4. **测试驱动**：实现前先写测试
5. **安全优先**：安全问题不能妥协

---

## 模块化规则

详细规则位于 `~/.claude/rules/`：

| 规则文件 | 内容 |
|-----------|------|
| security.md | 安全检查、密钥管理 |
| coding-style.md | 不可变性、文件组织、错误处理 |
| testing.md | TDD 工作流、80% 覆盖率要求 |
| git-workflow.md | 提交格式、PR 工作流 |
| agents.md | agent 编排、何时用哪个 agent |
| patterns.md | API 返回模式、repository 模式 |
| performance.md | 模型选择、上下文管理 |

---

## 可用 Agents

位于 `~/.claude/agents/`：

| Agent | 用途 |
|-------|------|
| planner | 功能实施规划 |
| architect | 系统设计与架构 |
| tdd-guide | 测试驱动开发 |
| code-reviewer | 质量 / 安全代码审查 |
| security-reviewer | 安全漏洞分析 |
| build-error-resolver | 构建错误修复 |
| e2e-runner | Playwright E2E 测试 |
| refactor-cleaner | 死代码清理 |
| doc-updater | 文档更新 |

---

## 个人偏好

### 代码风格
- 代码、注释、文档中不要用 emoji
- 偏好不可变写法，不直接修改对象或数组
- 很多小文件优于少数大文件
- 常见范围 200 到 400 行，最大不超过 800 行

### Git
- 使用 conventional commits：`feat:`、`fix:`、`refactor:`、`docs:`、`test:`
- 提交前总是在本地先测
- 保持小而聚焦的提交

### 测试
- TDD：先写测试
- 最低 80% 覆盖率
- 关键流程必须有单测 + 集成 + E2E

---

## 编辑器集成

我使用 Zed 作为主编辑器：
- 用 Agent Panel 跟踪文件
- 用 `CMD+Shift+R` 打开命令面板
- 开启 Vim mode

---

## 成功标准

满足以下条件时，才算成功：
- 所有测试通过（覆盖率 80%+）
- 没有安全漏洞
- 代码可读、可维护
- 满足用户需求

---

**哲学**：Agent 优先、并行执行、先计划后行动、先测试后编码、安全始终优先。
