# Update Documentation

根据单一事实来源同步文档：

1. 读取 `package.json` 的 scripts 区段
   - 生成脚本参考表
   - 如果有注释，包含脚本说明

2. 读取 `.env.example`
   - 提取所有环境变量
   - 记录用途与格式

3. 生成 `docs/CONTRIB.md`，内容包括：
   - 开发工作流
   - 可用脚本
   - 环境配置
   - 测试流程

4. 生成 `docs/RUNBOOK.md`，内容包括：
   - 部署流程
   - 监控与告警
   - 常见问题与修复
   - 回滚流程

5. 识别过期文档：
   - 找出 90 天以上未更新的文档
   - 列表输出供人工审查

6. 输出 diff 摘要

单一事实来源是：`package.json` 与 `.env.example`
