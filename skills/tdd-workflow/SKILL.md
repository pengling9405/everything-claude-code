---
name: tdd-workflow
description: 开发新功能、修复 缺陷 或做重构时使用。强制执行测试驱动开发，并要求单元、集成、E2E 综合覆盖达到 80%+。
---

# 测试-Driven Development 工作流

这个 skill 用来确保代码开发遵循 TDD 原则，并具备足够测试覆盖。

## When to Activate

- 开发新功能
- 修复 bug
- 重构现有代码
- 新增 API
- 创建新组件

## 核心原则

### 1. Tests BEFORE Code
永远先写测试，再写实现。

### 2. Coverage Requirements
- 最低 80% 覆盖率
- 覆盖边界情况
- 覆盖错误场景
- 覆盖边界值

### 3. 测试 Types

#### Unit Tests
- 单个函数
- 工具方法
- 纯逻辑

#### Integration Tests
- API 路由
- 数据库交互
- 服务间调用

#### E2E Tests (Playwright)
- 关键用户流程
- 完整工作流
- 浏览器交互

## TDD 工作流 Steps

### 步骤 1: Write 用户 Journeys
```text
As a [role], I want to [action], so that [benefit]
```

### 步骤 2: Generate 测试 Cases

```typescript
describe('Semantic Search', () => {
  it('returns relevant markets for query', async () => {})
  it('handles empty query gracefully', async () => {})
  it('falls back when Redis unavailable', async () => {})
})
```

### 步骤 3: Run Tests (They Should Fail)
```bash
npm test
# 测试此时应该失败
```

### 步骤 4: Implement Code
```typescript
export async function searchMarkets(query: string) {
  // 根据测试实现最小代码
}
```

### Step 5: Run Tests Again
```bash
npm test
# 测试现在应该通过
```

### Step 6: Refactor
- 去重
- 改善命名
- 提升可读性
- 必要时优化性能

### Step 7: Verify Coverage
```bash
npm run test:coverage
# 验证覆盖率达到 80%+
```

## Testing Patterns

### Unit 测试 模式 (Jest/Vitest)
```typescript
describe('Button Component', () => {
  it('renders with correct text', () => {})
  it('calls onClick when clicked', () => {})
  it('is disabled when disabled prop is true', () => {})
})
```

### API Integration 测试 模式
```typescript
describe('GET /api/markets', () => {
  it('returns markets successfully', async () => {})
})
```

### E2E 测试 模式 (Playwright)
```typescript
test('user can search and view market', async ({ page }) => {
  await page.goto('/')
})
```

## 测试 File Organization

建议结构：
```text
src/
tests/
├── unit/
├── integration/
└── e2e/
```

## Mocking External Services

### Supabase Mock
- mock 数据查询结果
- 覆盖错误分支

### Redis Mock
- 模拟缓存命中 / miss / 异常

### OpenAI Mock
- 固定 embedding / completion 响应
- 保证测试可重复

## 测试 Coverage Verification

### Run Coverage 报告
```bash
npm run test:coverage
```

### Coverage Thresholds
- Branches: 80%
- Functions: 80%
- Lines: 80%
- Statements: 80%

## Common Testing Mistakes to Avoid

### ❌ WRONG: Testing 实现 Details
- 不要测试内部 state、私有方法、临时实现细节

### ✅ CORRECT: 测试 用户-Visible Behavior
- 测试用户可观察到的输出、状态与副作用

### ❌ WRONG: Brittle Selectors
- 不要依赖容易变化的 DOM 结构

### ✅ CORRECT: Semantic Selectors
- 优先 `role`、`label`、`data-testid`

### ❌ WRONG: No 测试 Isolation
- 不要让测试相互依赖

### ✅ CORRECT: Independent Tests
- 每个测试独立准备数据与环境

## Continuous Testing

### Watch 模式 During Development
```bash
npm test -- --watch
```

### Pre-Commit Hook
```bash
npm test && npm run lint
```

### CI/CD Integration
```bash
npm test -- --coverage --ci
```

## Best Practices

- 每次只让一个失败测试变绿
- 保持实现最小化
- 重构前后都依赖测试护栏
- 让测试名称表达业务意图

## Success Metrics

- 关键逻辑都有测试
- 覆盖率达标
- 重构时可以放心修改
- bug 修复都附带回归测试

---

**原则**：TDD 的重点不是“测试写得多”，而是让实现始终被真实需求牵引，并让后续修改有可靠护栏。
