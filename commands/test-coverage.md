# 测试 Coverage

分析测试覆盖率并生成缺失测试：

1. 运行带覆盖率的测试：
   - `npm test --coverage`
   - 或 `pnpm test --coverage`

2. 分析覆盖率报告（`coverage/coverage-summary.json`）

3. 找出覆盖率低于 80% 的文件

4. 对每个覆盖率不足的文件：
   - 分析未覆盖的代码路径
   - 为函数生成单元测试
   - 为 API 生成集成测试
   - 为关键用户流程生成 E2E 测试

5. 验证新增测试通过

6. 输出修复前 / 修复后的覆盖率指标

7. 确保项目整体覆盖率达到 80% 以上

重点关注：
- Happy path
- 错误处理
- 边界情况（null、undefined、空值）
- 边界条件
