---
description: 重述需求、评估风险，并生成逐步实施计划。在动任何代码之前，必须等待用户确认。
---

# Plan Command

这个命令会调用 **planner** agent，在写任何代码之前先生成一份完整实施计划。

## 这个命令会做什么

1. **重述需求**：澄清要构建什么
2. **识别风险**：提前暴露潜在问题与阻塞点
3. **生成步骤计划**：把实现拆成阶段
4. **等待确认**：必须得到用户明确批准后才能继续

## 什么时候使用

在以下场景使用 `/plan`：
- 开始一个新功能
- 做较大的架构变更
- 处理复杂重构
- 会影响多个文件或组件
- 需求本身还不够清晰

## 它如何工作

planner agent 会：

1. **分析请求**并用清晰语言重述需求
2. **按阶段拆解**为具体、可执行的步骤
3. **识别组件间依赖**
4. **评估风险**与潜在阻塞
5. **估算复杂度**（高 / 中 / 低）
6. **展示计划**并等待你的明确确认

## 示例

```
User: /plan I need to add real-time notifications when markets resolve

Agent (planner):
# Implementation Plan: Real-Time Market Resolution Notifications

## Requirements Restatement
- Send notifications to users when markets they're watching resolve
- Support multiple notification channels (in-app, email, webhook)
- Ensure notifications are delivered reliably
- Include market outcome and user's position result

## Implementation Phases

### Phase 1: Database Schema
- Add notifications table with columns: id, user_id, market_id, type, status, created_at
- Add user_notification_preferences table for channel preferences
- Create indexes on user_id and market_id for performance

### Phase 2: Notification Service
- Create notification service in lib/notifications.ts
- Implement notification queue using BullMQ/Redis
- Add retry logic for failed deliveries
- Create notification templates

### Phase 3: Integration Points
- Hook into market resolution logic (when status changes to "resolved")
- Query all users with positions in market
- Enqueue notifications for each user

### Phase 4: Frontend Components
- Create NotificationBell component in header
- Add NotificationList modal
- Implement real-time updates via Supabase subscriptions
- Add notification preferences page

## Dependencies
- Redis (for queue)
- Email service (SendGrid/Resend)
- Supabase real-time subscriptions

## Risks
- HIGH: Email deliverability (SPF/DKIM required)
- MEDIUM: Performance with 1000+ users per market
- MEDIUM: Notification spam if markets resolve frequently
- LOW: Real-time subscription overhead

## Estimated Complexity: MEDIUM
- Backend: 4-6 hours
- Frontend: 3-4 hours
- Testing: 2-3 hours
- Total: 9-13 hours

**WAITING FOR CONFIRMATION**: Proceed with this plan? (yes/no/modify)
```

## 重要说明

**关键**：planner agent 在你明确回复 “yes” / “proceed” 等确认语句之前，**不会**开始写代码。

如果你想调整计划，可以这样回复：
- `modify: [你的修改意见]`
- `different approach: [替代方案]`
- `skip phase 2 and do phase 3 first`

## 与其他命令的配合

计划完成后，你通常可以：
- 用 `/tdd` 按测试驱动开发方式实施
- 构建出错时用 `/build-and-fix`
- 完成后用 `/code-review` 做代码审查

## 相关 Agent

此命令调用的 `planner` agent 位于：
`~/.claude/agents/planner.md`
