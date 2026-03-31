---
name: frontend-patterns
description: React 与 Next.js 场景下的前端开发模式，覆盖状态管理、性能优化与 UI 最佳实践。
---

# Frontend Development Patterns

面向 React、Next.js 与高性能界面的现代前端模式。

## Component Patterns

### Composition Over Inheritance

```typescript
interface CardProps {
  children: React.ReactNode
  variant?: 'default' | 'outlined'
}

export function Card({ children, variant = 'default' }: CardProps) {
  return <div className={`card card-${variant}`}>{children}</div>
}
```

优点：
- 复用灵活
- 结构清晰
- 比继承更符合 React 心智模型

### Compound Components

```typescript
const TabsContext = createContext<TabsContextValue | undefined>(undefined)

export function Tabs({ children, defaultTab }: Props) {
  const [activeTab, setActiveTab] = useState(defaultTab)
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      {children}
    </TabsContext.Provider>
  )
}
```

适用于：
- Tabs
- Accordion
- Menu
- Dialog 组合部件

### Render Props Pattern

当需要把“数据获取逻辑”和“展示逻辑”解耦时可以使用，但在现代 React 中通常优先考虑 custom hooks。

## Custom Hooks Patterns

### State Management Hook

把局部业务状态收拢到 hook 中，减少组件体膨胀。

### Async Data Fetching Hook

要求：
- 管理 loading / error / data
- 支持取消、重试或防抖（视场景）
- 避免 effect 竞态

### Debounce Hook

适用于：
- 搜索输入
- 自动保存
- 高频筛选条件

## State Management Patterns

### Context + Reducer Pattern

适合：
- 中等复杂度共享状态
- 明确 action 和 state 转移
- 不想立即引入外部 store

不适合：
- 高频局部更新
- 超大应用全局状态

## Performance Optimization

### Memoization

- 仅在性能热点使用
- 优先先测量，再优化
- 避免过早引入 `useMemo` / `useCallback`

### Code Splitting & Lazy Loading

对以下内容优先做懒加载：
- 大图表
- 富文本编辑器
- 管理后台模块
- 不常访问的路由

### Virtualization for Long Lists

适用于：
- 长列表
- 表格
- 日志流
- 聊天消息

## Form Handling Patterns

### Controlled Form with Validation

表单应明确处理：
- 字段值
- 校验错误
- 提交状态
- 服务端错误回显

## Error Boundary Pattern

错误边界适合：
- 页面级隔离
- 重型组件隔离
- 防止局部崩溃拖垮整页

## Animation Patterns

### Framer Motion Animations

建议：
- 动画服务于信息层级和状态变化
- 不要为每个元素都加无意义 motion
- 优先用少量高信号过渡

## Accessibility Patterns

### Keyboard Navigation
- 所有关键交互都应可键盘操作
- 保持正确 tab 顺序
- 焦点态可见

### Focus Management
- Dialog 打开后自动聚焦
- 关闭后把焦点还回触发元素
- 异步内容加载后注意焦点丢失问题

---

**原则**：前端模式的目标不是堆更多 abstraction，而是让 UI 更稳定、状态更清晰、性能更可控、可访问性更可靠。
