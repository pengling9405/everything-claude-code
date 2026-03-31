---
name: coding-standards
description: 适用于 TypeScript、JavaScript、React 与 Node.js 的通用编码规范、最佳实践与常见模式。
---

# Coding Standards & Best Practices

适用于大多数项目的通用编码规范。

## Code Quality Principles

### 1. Readability First
- 代码被阅读的次数远多于被编写
- 变量名和函数名要清晰
- 优先写自解释代码，而不是依赖注释
- 保持统一格式

### 2. KISS (Keep It Simple, Stupid)
- 选择能工作的最简单方案
- 避免过度设计
- 不做过早优化
- 易懂胜过“聪明”

### 3. DRY (Don't Repeat Yourself)
- 抽出公共逻辑
- 组件与工具函数尽量复用
- 避免复制粘贴式编程

### 4. YAGNI (You Aren't Gonna Need It)
- 不要提前实现未来可能用到的功能
- 避免投机性的通用化
- 复杂度只在确有需要时引入

## TypeScript/JavaScript Standards

### Variable Naming

```typescript
// ✅ 清晰命名
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// ❌ 模糊命名
const q = 'election'
const flag = true
const x = 1000
```

### Function Naming

```typescript
// ✅ 动词 + 名词
async function fetchMarketData(marketId: string) {}
function calculateSimilarity(a: number[], b: number[]) {}
function isValidEmail(email: string): boolean {}
```

### Immutability Pattern (CRITICAL)

```typescript
// ✅ 优先不可变更新
const updatedUser = {
  ...user,
  name: 'New Name',
}

const updatedArray = [...items, newItem]

// ❌ 避免直接修改
user.name = 'New Name'
items.push(newItem)
```

### Error Handling

```typescript
async function fetchData(url: string) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`)
    }

    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    throw new Error('Failed to fetch data')
  }
}
```

### Async/Await Best Practices

```typescript
// ✅ 可并行时并行
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats(),
])
```

### Type Safety

- 开启严格模式
- 避免 `any`
- 用类型守卫和 schema 校验外部输入
- 对公共 API 明确返回类型

## React Best Practices

### Component Structure
- 单个组件只承担单一职责
- props 命名清晰
- 组件体不要堆过多副作用

### Custom Hooks
- 将可复用状态逻辑抽进 hook
- hook 名称以 `use` 开头
- hook 内部管理副作用与状态

### State Management
- 本地状态优先本地管理
- 跨组件共享再提升为 context / store
- 不要把一切都塞进全局状态

### Conditional Rendering
- 让分支清晰
- 对 loading / empty / error 状态显式建模

## API Design Standards

### REST API Conventions

```typescript
// Query 参数用于筛选 / 排序 / 分页
GET /api/markets?status=active&sort=volume&limit=20
```

### Response Format

- 成功与失败结构统一
- 错误码与错误消息清晰
- 不向客户端暴露内部异常细节

### Input Validation

- 所有外部输入都要校验
- 在边界层做 schema 校验
- 不信任前端传入数据

## File Organization

### Project Structure
- 功能分组优于技术分组时，优先按功能划分
- 共享代码与业务代码分离

### File Naming
- 文件名与导出名称保持一致
- React 组件使用 PascalCase
- 工具函数文件使用 kebab-case 或 camelCase，保持项目一致

## Comments & Documentation

### When to Comment
- 解释“为什么”
- 记录反直觉约束
- 说明外部系统兼容原因

### JSDoc for Public APIs
- 公共函数、类、hook、组件应补充 JSDoc
- 重点写参数、返回值、边界与副作用

## Performance Best Practices

### Memoization
- 只在真实热点路径使用 memoization
- 不要为了“看起来专业”而滥用

### Lazy Loading
- 对大页面、图表、编辑器等重型模块启用懒加载

### Database Queries
- 避免 N+1
- 少查字段
- 把聚合尽量下推到数据库

## Testing Standards

### Test Structure (AAA Pattern)
- Arrange
- Act
- Assert

### Test Naming
- 名称应准确描述行为与预期
- 优先写“should ... when ...”这类可读句式

## Code Smell Detection

### 1. Long Functions
- 超过 50 行要检查是否职责过多

### 2. Deep Nesting
- 嵌套超过 4 层要考虑提前返回或拆函数

### 3. Magic Numbers
- 常量需要命名，不要无说明地散落数字

---

**原则**：规范的目的不是让代码更“整齐”，而是让未来的修改更安全、更低成本。
