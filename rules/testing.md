# Testing Requirements

## 最低测试覆盖率：80%

测试类型（全部必需）：
1. **Unit Tests**：单个函数、工具函数、组件
2. **Integration Tests**：API 接口、数据库操作
3. **E2E Tests**：关键用户流程（Playwright）

## 测试驱动开发

强制工作流：
1. 先写测试（RED）
2. 运行测试，它应当失败
3. 写最小实现（GREEN）
4. 再跑测试，它应当通过
5. 重构（IMPROVE）
6. 验证覆盖率（80%+）

## 测试失败时的排查

1. 使用 **tdd-guide** agent
2. 检查测试隔离性
3. 验证 mock 是否正确
4. 优先修实现，而不是乱改测试（除非测试本身写错）

## Agent 支持

- **tdd-guide**：新功能时主动使用，强制测试先行
- **e2e-runner**：Playwright E2E 测试专项 agent
