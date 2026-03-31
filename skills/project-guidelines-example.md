# Project Guidelines Skill (Example)

这是一个项目专属 skill 的示例模板，可作为你在真实项目中编写私有规范时的参考。

示例基于生产应用：[Zenith](https://zenith.chat)。

---

## When to Use

当你在某个具体项目里工作时，可以引用这类 project skill。它通常应包含：
- 架构概览
- 文件结构
- 代码模式
- 测试要求
- 部署流程

---

## Architecture Overview

**Tech Stack：**
- **Frontend**：Next.js 15（App Router）、TypeScript、React
- **Backend**：FastAPI（Python）、Pydantic
- **Database**：Supabase（PostgreSQL）
- **AI**：Claude API + tool calling + structured output
- **Deployment**：Google Cloud Run
- **Testing**：Playwright、pytest、React Testing Library

**Services：**
```text
Frontend (Next.js)
  ↓
Backend (FastAPI)
  ├─ Supabase
  ├─ Claude API
  └─ Redis
```

---

## File Structure

```text
project/
├── frontend/
│   └── src/
│       ├── app/
│       ├── components/
│       ├── hooks/
│       ├── lib/
│       ├── types/
│       └── config/
├── backend/
│   ├── routers/
│   ├── models.py
│   ├── main.py
│   ├── auth_system.py
│   ├── database.py
│   ├── services/
│   └── tests/
├── deploy/
├── docs/
└── scripts/
```

---

## Code Patterns

### API Response Format (FastAPI)

```python
class ApiResponse(BaseModel, Generic[T]):
    success: bool
    data: Optional[T] = None
    error: Optional[str] = None
```

统一响应结构有助于：
- 前后端对齐
- 错误处理统一
- 提升可测试性

### Frontend API Calls (TypeScript)

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
}
```

建议：
- 前端统一通过一个 `fetchApi` 或 client 封装请求
- 在同一层处理错误与 headers

### Claude AI Integration (Structured Output)

```python
class AnalysisResult(BaseModel):
    summary: str
    key_points: list[str]
    confidence: float
```

重点：
- 用 schema 约束 AI 输出
- 明确工具调用入口
- 不直接信任自由文本

### Custom Hooks (React)

- 把页面级状态逻辑抽成 hook
- 减少页面组件体积
- 让数据获取、过滤、提交逻辑更可复用

---

## Testing Requirements

### Backend (pytest)

```bash
# 运行全部测试
pytest

# 带覆盖率
pytest --cov

# 运行单个测试文件
pytest tests/test_api.py
```

### Frontend (React Testing Library)

```bash
# 运行测试
npm test

# 带覆盖率
npm run test:coverage

# 运行 E2E
npx playwright test
```

---

## Deployment Workflow

### Pre-Deployment Checklist

- [ ] 环境变量已配置
- [ ] 数据库迁移已执行
- [ ] 构建通过
- [ ] 核心路径测试通过

### Deployment Commands

```bash
# 构建并部署前端
npm run build

# 构建并部署后端
docker build -t app .
```

### Environment Variables

```bash
# Frontend (.env.local)
NEXT_PUBLIC_API_URL=

# Backend (.env)
DATABASE_URL=
CLAUDE_API_KEY=
```

---

## Critical Rules

- 优先遵循项目现有模式，不要引入个人偏好
- 新增功能前先看相邻实现
- 所有对外输入都要校验
- AI 输出必须结构化
- 核心路径必须有测试

---

## Related Skills

- `coding-standards`
- `frontend-patterns`
- `backend-patterns`
- `tdd-workflow`
- `security-review`

---

**建议**：每个真实项目都应该维护一份自己的 project skill，把“这个项目和别的项目到底哪里不一样”写清楚。
