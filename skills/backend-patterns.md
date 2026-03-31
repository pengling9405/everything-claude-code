---
name: backend-patterns
description: 面向 Node.js、Express 与 Next.js API routes 的后端架构模式、API 设计、数据库优化与服务端最佳实践。
---

# Backend Development Patterns

适用于可扩展服务端应用的后端模式与实践集合。

## API Design Patterns

### RESTful API Structure

```typescript
// ✅ 资源化 URL
GET    /api/markets
GET    /api/markets/:id
POST   /api/markets
PUT    /api/markets/:id
PATCH  /api/markets/:id
DELETE /api/markets/:id

// ✅ 用 query 参数做筛选 / 排序 / 分页
GET /api/markets?status=active&sort=volume&limit=20&offset=0
```

核心要求：
- URL 表达资源，不表达动作
- 请求体承载输入，响应体承载结果
- 错误码与错误结构保持一致

### Repository Pattern

```typescript
interface MarketRepository {
  findAll(filters?: MarketFilters): Promise<Market[]>
  findById(id: string): Promise<Market | null>
  create(data: CreateMarketDto): Promise<Market>
  update(id: string, data: UpdateMarketDto): Promise<Market>
  delete(id: string): Promise<void>
}
```

适用场景：
- 需要将数据访问与业务逻辑分离
- 未来可能切换数据源
- 测试中需要 mock 数据层

### Service Layer Pattern

```typescript
class MarketService {
  constructor(private marketRepo: MarketRepository) {}

  async searchMarkets(query: string, limit = 10) {
    const embedding = await generateEmbedding(query)
    const results = await this.vectorSearch(embedding, limit)
    const markets = await this.marketRepo.findByIds(results.map(r => r.id))
    return markets
  }
}
```

服务层只做三件事：
- 编排业务流程
- 保持领域规则集中
- 隔离控制器与数据层

### Middleware Pattern

```typescript
export function withAuth(handler: NextApiHandler): NextApiHandler {
  return async (req, res) => {
    const token = req.headers.authorization?.replace('Bearer ', '')

    if (!token) {
      return res.status(401).json({ error: 'Unauthorized' })
    }

    const user = await verifyToken(token)
    req.user = user
    return handler(req, res)
  }
}
```

适合抽离：
- 鉴权
- 日志
- rate limit
- tracing
- 错误包装

## Database Patterns

### Query Optimization

- 只查需要的字段
- 为高频过滤字段建立索引
- 避免在热点路径里做大范围 scan
- 对长链路查询加缓存或预聚合

### N+1 Query Prevention

```typescript
// ❌ 循环里逐条查
for (const market of markets) {
  market.owner = await db.users.findById(market.userId)
}

// ✅ 批量查
const userIds = [...new Set(markets.map(m => m.userId))]
const users = await db.users.findManyByIds(userIds)
```

### Transaction Pattern

```typescript
await db.transaction(async tx => {
  await tx.wallets.decrementBalance(userId, amount)
  await tx.orders.create(order)
  await tx.auditLogs.insert(log)
})
```

适用于：
- 支付
- 转账
- 库存与订单联动
- 多表一致性更新

## Caching Strategies

### Redis Caching Layer

缓存适合用于：
- 读多写少的数据
- 热门榜单
- 聚合结果
- 外部 API 响应

### Cache-Aside Pattern

```typescript
async function getMarket(id: string) {
  const cached = await redis.get(`market:${id}`)
  if (cached) return JSON.parse(cached)

  const market = await db.markets.findById(id)
  await redis.set(`market:${id}`, JSON.stringify(market), 'EX', 60)
  return market
}
```

注意：
- TTL 要明确
- 写入后要失效或回填缓存
- 缓存失败时业务应可回退

## Error Handling Patterns

### Centralized Error Handler

```typescript
export function handleApiError(error: unknown) {
  if (error instanceof ZodError) {
    return { status: 400, body: { error: 'Invalid input' } }
  }

  return { status: 500, body: { error: 'Internal server error' } }
}
```

### Retry with Exponential Backoff

```typescript
for (let attempt = 0; attempt < 3; attempt++) {
  try {
    return await callExternalApi()
  } catch (error) {
    await wait(2 ** attempt * 100)
  }
}
```

只对可重试错误使用：
- 网络抖动
- 429 / 5xx
- 外部服务偶发超时

## Authentication & Authorization

### JWT Token Validation

- 校验签名
- 校验过期时间
- 校验 audience / issuer
- 对服务端资源访问进行二次授权判断

### Role-Based Access Control

```typescript
function requireRole(user: User, role: Role) {
  if (user.role !== role) {
    throw new Error('Forbidden')
  }
}
```

## Rate Limiting

### Simple In-Memory Rate Limiter

本地内存限流适用于开发或单实例服务；生产环境优先使用 Redis / 网关层限流。

## Background Jobs & Queues

### Simple Queue Pattern

适用于：
- 邮件发送
- Webhook 重试
- 数据同步
- 批量计算

要求：
- 任务幂等
- 可重试
- 有失败记录

## Logging & Monitoring

### Structured Logging

```typescript
logger.info('market_created', {
  marketId,
  userId,
  category,
})
```

建议统一记录：
- request id
- user id
- route / action
- duration
- error code

---

**原则**：后端模式的目标不是追求“层数更多”，而是让职责更清晰、错误更可控、性能更可预测。
