---
name: security-review
description: 在实现鉴权、处理用户输入、接触 secrets、创建 API、支付或其他敏感功能时使用。提供完整安全检查清单与推荐模式。
---

# Security 审查 技能

这个 skill 用来确保代码遵循安全最佳实践，并主动识别潜在漏洞。

## When to Activate

- 实现认证或授权逻辑
- 处理用户输入或文件上传
- 新增 API 端点
- 使用 secrets / credentials
- 支付、账务、交易相关功能
- 存储或传输敏感数据
- 集成第三方 API

## Security Checklist

### 1. Secrets Management

#### ❌ NEVER Do This
```typescript
const apiKey = "sk-proj-xxxxx"
const dbPassword = "password123"
```

#### ✅ ALWAYS Do This
```typescript
const apiKey = process.env.OPENAI_API_KEY
const dbUrl = process.env.DATABASE_URL

if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}
```

#### Verification Steps
- [ ] 没有硬编码 key / token / password
- [ ] secrets 全部来自环境变量
- [ ] `.env.local` 已加入 `.gitignore`
- [ ] git 历史中没有泄露
- [ ] 生产 secrets 仅存放在托管平台

### 2. Input Validation

#### Always Validate 用户 Input
```typescript
const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  age: z.number().int().min(0).max(150),
})
```

#### File Upload Validation
```typescript
function validateFileUpload(file: File) {
  const maxSize = 5 * 1024 * 1024
  if (file.size > maxSize) throw new Error('File too large')
}
```

#### Verification Steps
- [ ] 所有输入都用 schema 校验
- [ ] 文件上传受大小 / 类型 / 扩展名限制
- [ ] 不直接把用户输入放进查询或命令
- [ ] 优先白名单校验
- [ ] 错误消息不泄露内部细节

### 3. SQL Injection Prevention

#### ❌ NEVER Concatenate SQL
```typescript
const query = `SELECT * FROM users WHERE email = '${userEmail}'`
```

#### ✅ ALWAYS Use Parameterized Queries
```typescript
await db.query(
  'SELECT * FROM users WHERE email = $1',
  [userEmail]
)
```

#### Verification Steps
- [ ] 所有查询使用参数化方式
- [ ] 没有 SQL 字符串拼接
- [ ] ORM / query builder 使用正确

### 4. Authentication & Authorization

#### JWT Token Handling
- 不把敏感 token 直接放在脆弱存储中
- 校验签名、过期时间、issuer / audience

#### Authorization Checks
- 每个敏感路由都检查权限
- 不只判断“是否登录”，还要判断“是否有权访问该资源”

#### Row Level Security (Supabase)
- 生产环境默认启用 RLS
- 策略基于用户身份和资源归属

#### Verification Steps
- [ ] 鉴权逻辑正确
- [ ] 授权逻辑逐路由检查
- [ ] 没有越权读写

### 5. XSS Prevention

#### Sanitize HTML
- 渲染用户内容前进行清洗
- 对 `dangerouslySetInnerHTML` 保持零信任

#### 内容 Security Policy
- 配置 CSP，限制脚本来源

#### Verification Steps
- [ ] 输出已转义或清洗
- [ ] 无不受控 HTML 注入
- [ ] CSP 已配置

### 6. CSRF Protection

#### CSRF Tokens
- 对敏感写操作考虑 token 方案

#### SameSite Cookies
- 默认使用更严格的 cookie 策略

#### Verification Steps
- [ ] 跨站请求具备防护
- [ ] cookie 属性设置合理

### 7. Rate Limiting

#### API Rate Limiting
- 登录、注册、密码重置、导出等接口必须限流

#### Expensive Operations
- AI 请求、支付、批量任务等高成本操作要额外限制

#### Verification Steps
- [ ] 高风险端点有限流
- [ ] 有滥用防护

### 8. Sensitive Data Exposure

#### Logging
- 不记录 token、密码、私钥、支付信息

#### 错误 Messages
- 不向前端暴露内部实现与栈信息

#### Verification Steps
- [ ] 日志已脱敏
- [ ] 错误输出安全

### 9. Blockchain Security (Solana)

#### Wallet Verification
- 验证签名与钱包归属

#### Transaction Verification
- 链上状态必须二次确认，不信任客户端回传结果

#### Verification Steps
- [ ] 签名验证到位
- [ ] 交易状态有后端确认

### 10. Dependency Security

#### Regular Updates

```bash
# 检查漏洞
npm audit

# 自动修复可修问题
npm audit fix

# 检查过期包
npm outdated
```

#### Lock Files

```bash
# lock file 必须提交
```

#### Verification Steps
- [ ] 无高危依赖漏洞
- [ ] lock file 已提交
- [ ] 依赖更新有验证

## Security Testing

### Automated Security Tests
- 依赖审计
- secrets 扫描
- 关键输入的 schema 测试
- 权限边界测试

## Pre-Deployment Security Checklist

- [ ] secrets 管理正确
- [ ] 输入校验完成
- [ ] 鉴权 / 授权验证完成
- [ ] 关键路由有限流
- [ ] 日志与错误输出已脱敏

## Resources

- OWASP Top 10
- Supabase RLS 文档
- Zod / Valibot 输入校验文档
- 平台托管服务的 secrets 管理文档

---

**原则**：所有外部输入默认不可信，所有敏感能力默认不开放，所有 secrets 默认不允许进入仓库。
