# Update Codemaps

分析代码库结构并更新架构文档：

1. 扫描所有源码文件中的 imports、exports 与依赖关系
2. 用以下格式生成精简 codemap：
   - `codemaps/architecture.md`：整体架构
   - `codemaps/backend.md`：后端结构
   - `codemaps/frontend.md`：前端结构
   - `codemaps/data.md`：数据模型与 schema

3. 计算与上一版本的差异比例
4. 如果变化超过 30%，更新前先请求用户批准
5. 给每个 codemap 添加更新时间戳
6. 把报告保存到 `.reports/codemap-diff.txt`

使用 TypeScript / Node.js 做分析，重点关注高层结构，而不是实现细节。
