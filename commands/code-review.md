# Code Review

对未提交改动做一次完整的安全与质量审查：

1. 获取变更文件：
   - `git diff --name-only HEAD`

2. 对每个变更文件检查：

**安全问题（CRITICAL）：**
- 硬编码凭据、API key、token
- SQL 注入风险
- XSS 风险
- 缺少输入校验
- 不安全依赖
- 路径遍历风险

**代码质量（HIGH）：**
- 函数长度大于 50 行
- 文件长度大于 800 行
- 嵌套深度超过 4 层
- 缺少错误处理
- 存在 `console.log`
- 存在 `TODO` / `FIXME`
- 公共 API 缺少 JSDoc

**最佳实践（MEDIUM）：**
- 直接修改对象或数组（应使用不可变写法）
- 代码或注释里使用 emoji
- 新代码缺少测试
- 可访问性问题（a11y）

3. 生成审查报告，包含：
   - 严重等级：CRITICAL / HIGH / MEDIUM / LOW
   - 文件位置与行号
   - 问题描述
   - 修复建议

4. 如果发现 CRITICAL 或 HIGH 级问题，则阻止提交

永远不要批准带安全漏洞的代码。
