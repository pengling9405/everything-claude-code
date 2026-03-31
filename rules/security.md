# Security Guidelines

## 强制安全检查

在 **任何提交之前**：
- [ ] 没有硬编码密钥（API key、密码、token）
- [ ] 所有用户输入都已校验
- [ ] 已防止 SQL 注入（参数化查询）
- [ ] 已防止 XSS（对 HTML 做净化）
- [ ] 已启用 CSRF 防护
- [ ] 已验证认证与授权
- [ ] 所有接口都有速率限制
- [ ] 错误信息不会泄漏敏感数据

## 密钥管理

```typescript
// NEVER: Hardcoded secrets
const apiKey = "sk-proj-xxxxx"

// ALWAYS: Environment variables
const apiKey = process.env.OPENAI_API_KEY

if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}
```

## 安全响应流程

如果发现安全问题：
1. 立即停止继续推进
2. 调用 **security-reviewer** agent
3. 在继续前先修复所有 CRITICAL 问题
4. 轮换任何已暴露的密钥
5. 回查整个代码库中是否还有类似问题
