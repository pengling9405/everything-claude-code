# Performance Optimization

## 模型选择策略

**Haiku 4.5**（约 90% 的 Sonnet 能力，成本节省约 3 倍）：
- Lightweight agents with frequent invocation
- Pair programming and code generation
- Worker agents in multi-agent systems

**Sonnet 4.5**（最佳编码模型）：
- Main development work
- Orchestrating multi-agent workflows
- Complex coding tasks

**Opus 4.5**（最强深度推理）：
- Complex architectural decisions
- Maximum reasoning requirements
- Research and analysis tasks

## 上下文窗口管理

以下场景应避免把上下文窗口用到最后 20%：
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions

对上下文敏感度较低的任务：
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes

## Ultrathink + 计划 模式

对于需要深度推理的复杂任务：
1. 用 `ultrathink` 增强思考深度
2. 开启 **Plan Mode** 形成结构化路径
3. 通过多轮批判性审视“把引擎转起来”
4. 用不同角色的子代理做多视角分析

## 构建排障

如果构建失败：
1. 使用 **build-error-resolver**
2. 分析错误信息
3. 逐步修复
4. 每修一次都重新验证
