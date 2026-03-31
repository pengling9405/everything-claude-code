# Coding Style

## 不可变性（关键）

始终创建新对象，绝不要直接修改原对象：

```javascript
// WRONG: Mutation
function updateUser(user, name) {
  user.name = name  // MUTATION!
  return user
}

// CORRECT: Immutability
function updateUser(user, name) {
  return {
    ...user,
    name
  }
}
```

## 文件组织

很多小文件优于少数大文件：
- 高内聚、低耦合
- 常见范围是 200 到 400 行，最大不超过 800 行
- 从大组件中抽取工具函数
- 按 feature / domain 组织，而不是按类型组织

## 错误处理

始终做完整错误处理：

```typescript
try {
  const result = await riskyOperation()
  return result
} catch (error) {
  console.error('Operation failed:', error)
  throw new Error('Detailed user-friendly message')
}
```

## 输入校验

始终校验用户输入：

```typescript
import { z } from 'zod'

const schema = z.object({
  email: z.string().email(),
  age: z.number().int().min(0).max(150)
})

const validated = schema.parse(input)
```

## 代码质量检查清单

在标记完成前确认：
- [ ] 代码可读、命名清晰
- [ ] 函数足够小（< 50 行）
- [ ] 文件职责聚焦（< 800 行）
- [ ] 没有过深嵌套（> 4 层）
- [ ] 错误处理完善
- [ ] 没有 `console.log`
- [ ] 没有硬编码值
- [ ] 没有直接修改对象（使用不可变模式）
