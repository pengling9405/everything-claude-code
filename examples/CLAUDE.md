# 示例项目 CLAUDE.md

这是一个项目级 `CLAUDE.md` 示例文件。把它放在项目根目录即可。

## 项目概览

[简要描述你的项目，包括它做什么、技术栈是什么]

## 关键规则

### 1. 代码组织

- Many small files over few large files
- High cohesion, low coupling
- 200-400 lines typical, 800 max per file
- Organize by feature/domain, not by type

### 2. 代码风格

- No emojis in code, comments, or documentation
- Immutability always - never mutate objects or arrays
- No console.log in production code
- Proper error handling with try/catch
- Input validation with Zod or similar

### 3. 测试

- TDD: Write tests first
- 80% minimum coverage
- Unit tests for utilities
- Integration tests for APIs
- E2E tests for critical flows

### 4. 安全

- No hardcoded secrets
- Environment variables for sensitive data
- Validate all user inputs
- Parameterized queries only
- CSRF protection enabled

## 文件结构

```
src/
|-- app/              # Next.js app router
|-- components/       # Reusable UI components
|-- hooks/            # Custom React hooks
|-- lib/              # Utility libraries
|-- types/            # TypeScript definitions
```

## 关键模式

### API 返回格式

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
}
```

### 错误处理

```typescript
try {
  const result = await operation()
  return { success: true, data: result }
} catch (error) {
  console.error('Operation failed:', error)
  return { success: false, error: 'User-friendly message' }
}
```

## 环境变量

```bash
# 必填
DATABASE_URL=
API_KEY=

# 可选
DEBUG=false
```

## 可用命令

- `/tdd`：测试驱动开发工作流
- `/plan`：创建实施计划
- `/code-review`：审查代码质量
- `/build-fix`：修复构建错误

## Git 工作流

- 使用 conventional commits：`feat:`、`fix:`、`refactor:`、`docs:`、`test:`
- 不要直接向 `main` 提交
- PR 必须经过审查
- 合并前必须保证测试通过
