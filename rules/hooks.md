# Hooks System

## Hook 类型

- **PreToolUse**：工具执行前（做校验、改参数）
- **PostToolUse**：工具执行后（做自动格式化、检查）
- **Stop**：会话结束时（做最终验证）

## 当前 Hooks（配置在 `~/.claude/settings.json`）

### PreToolUse
- **tmux 提醒**：对长时间运行命令建议使用 tmux（如 npm、pnpm、yarn、cargo 等）
- **git push 审查暂停**：推送前先打开 Zed 复查
- **文档阻断器**：阻止随意创建无必要的 `.md` / `.txt` 文件

### PostToolUse
- **PR 创建日志**：记录 PR URL 与 GitHub Actions 状态
- **Prettier**：编辑 JS / TS 文件后自动格式化
- **TypeScript 检查**：编辑 `.ts` / `.tsx` 后自动运行 `tsc`
- **console.log 警告**：编辑后发现 `console.log` 就报警

### Stop
- **console.log 审计**：会话结束前检查所有已修改文件中是否还残留 `console.log`

## Auto-Accept 权限

谨慎使用：
- 只在可信、定义明确的计划中启用
- 探索性工作时不要开
- 永远不要使用 `dangerously-skip-permissions`
- 应改用 `~/.claude.json` 中的 `allowedTools`

## TodoWrite 最佳实践

使用 TodoWrite：
- 跟踪多步骤任务进度
- 验证你是否真正理解了指令
- 让用户能实时纠偏
- 展示足够细颗粒度的实施步骤

Todo 列表能够暴露：
- 步骤顺序不对
- 漏项
- 多余步骤
- 粒度不对
- 对需求理解错误
