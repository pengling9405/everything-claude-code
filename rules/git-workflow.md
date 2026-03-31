# Git Workflow

## Commit Message 格式

```
<type>: <description>

<optional body>
```

类型：`feat`、`fix`、`refactor`、`docs`、`test`、`chore`、`perf`、`ci`

说明：全局 attribution 已在 `~/.claude/settings.json` 中关闭。

## Pull Request 工作流

创建 PR 时：
1. 分析完整提交历史，而不仅是最新提交
2. 使用 `git diff [base-branch]...HEAD` 查看全部改动
3. 起草完整 PR 摘要
4. 包含测试计划与 TODO
5. 如果是新分支，使用 `-u` 推送

## 功能实现工作流

1. **先做计划**
   - 用 **planner** agent 创建实施计划
   - 识别依赖与风险
   - 按阶段拆解

2. **TDD 路径**
   - 用 **tdd-guide** agent
   - 先写测试（RED）
   - 写实现让测试通过（GREEN）
   - 重构（IMPROVE）
   - 验证覆盖率达到 80% 以上

3. **代码审查**
   - 写完代码后立即运行 **code-reviewer**
   - 先解决 CRITICAL 和 HIGH 问题
   - 尽可能修复 MEDIUM 问题

4. **提交与推送**
   - 提交信息要足够具体
   - 遵循 conventional commits
