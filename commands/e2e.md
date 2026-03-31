---
description: 生成并运行基于 Playwright 的端到端测试，涵盖用户旅程、测试执行、失败产物捕获和报告上传。
---

# E2E Command

这个命令会调用 **e2e-runner** agent，负责使用 Playwright 生成、维护并执行端到端测试。

## What This Command Does

1. **生成测试旅程**：为关键用户流程创建 Playwright 测试
2. **运行 E2E 测试**：在多浏览器环境下执行
3. **捕获测试产物**：失败时保存截图、视频与 trace
4. **上传结果**：生成 HTML 报告与 JUnit XML
5. **识别不稳定测试**：标记并隔离 flaky tests

## When to Use

在以下场景使用 `/e2e`：
- 测试关键用户旅程（登录、交易、支付等）
- 验证多步骤流程是否端到端可用
- 测试 UI 交互与页面导航
- 验证前后端集成是否正常
- 准备上线前做回归验证

## How It Works

e2e-runner agent 会：

1. **分析用户流程**并识别测试场景
2. **生成 Playwright 测试代码**，默认采用 Page Object Model
3. **运行测试**，覆盖 Chrome / Firefox / Safari 等浏览器
4. **捕获失败现场**，包括截图、视频与 trace
5. **生成报告**并附带测试产物
6. **识别 flaky tests** 并给出修复建议

## Example Usage

```text
User: /e2e Test the market search and view flow

Agent (e2e-runner):
# E2E Test Generation: Market Search and View Flow

## Test Scenario Identified

**User Journey:** Market Search → View Results → Click Market → View Details

**Test Coverage:**
1. 打开市场页
2. 执行语义搜索
3. 验证搜索结果
4. 点击第一个市场
5. 验证详情页加载成功
6. 验证图表已渲染
```

## Generated Test Code

```typescript
// tests/e2e/markets/search-and-view.spec.ts
import { test, expect } from '@playwright/test'
import { MarketsPage } from '../../pages/MarketsPage'
import { MarketDetailsPage } from '../../pages/MarketDetailsPage'

test.describe('Market Search and View Flow', () => {
  test('user can search markets and view details', async ({ page }) => {
    // 1. 打开市场页
    const marketsPage = new MarketsPage(page)
    await marketsPage.goto()

    await expect(page).toHaveTitle(/Markets/)
    await expect(page.locator('h1')).toContainText('Markets')

    // 2. 执行语义搜索
    await marketsPage.searchMarkets('election')

    await page.waitForResponse(resp =>
      resp.url().includes('/api/markets/search') && resp.status() === 200
    )

    // 3. 验证搜索结果
    const marketCards = marketsPage.marketCards
    await expect(marketCards.first()).toBeVisible()
    const resultCount = await marketCards.count()
    expect(resultCount).toBeGreaterThan(0)

    await page.screenshot({ path: 'artifacts/search-results.png' })

    // 4. 点击第一个结果
    const firstMarketTitle = await marketCards.first().textContent()
    await marketCards.first().click()

    // 5. 验证详情页
    await expect(page).toHaveURL(/\/markets\/[a-z0-9-]+/)

    const detailsPage = new MarketDetailsPage(page)
    await expect(detailsPage.marketName).toBeVisible()
    await expect(detailsPage.marketDescription).toBeVisible()

    // 6. 验证图表渲染
    await expect(detailsPage.priceChart).toBeVisible()

    const detailsTitle = await detailsPage.marketName.textContent()
    expect(detailsTitle?.toLowerCase()).toContain(
      firstMarketTitle?.toLowerCase().substring(0, 20) || ''
    )

    await page.screenshot({ path: 'artifacts/market-details.png' })
  })
})
```

## Running Tests

```bash
# 运行生成的测试
npx playwright test tests/e2e/markets/search-and-view.spec.ts
```

## Test Report

```text
╔══════════════════════════════════════════════════════════════╗
║                    E2E Test Results                          ║
╠══════════════════════════════════════════════════════════════╣
║ Status:     ✅ ALL TESTS PASSED                              ║
║ Total:      3 tests                                          ║
║ Passed:     3 (100%)                                         ║
║ Failed:     0                                                ║
║ Flaky:      0                                                ║
║ Duration:   9.1s                                             ║
╚══════════════════════════════════════════════════════════════╝
```

## Test Artifacts

运行测试后会生成以下产物：

**所有测试都会生成：**
- HTML 报告
- JUnit XML（便于 CI 集成）

**仅失败时生成：**
- 当前失败界面截图
- 测试视频
- Trace 文件（逐步回放）
- 网络日志
- 控制台日志

## Viewing Artifacts

```bash
# 在浏览器查看 HTML 报告
npx playwright show-report

# 查看指定 trace 文件
npx playwright show-trace artifacts/trace-abc123.zip

# 截图通常保存在 artifacts/ 目录
open artifacts/search-results.png
```

## Flaky Test Detection

如果测试存在间歇性失败，应输出类似结果：

```text
⚠️  FLAKY TEST DETECTED: tests/e2e/markets/trade.spec.ts

Test passed 7/10 runs (70% pass rate)

Common failure:
"Timeout waiting for element '[data-testid=\"confirm-btn\"]'"

Recommended fixes:
1. 增加显式等待
2. 调整 timeout
3. 排查组件中的竞态问题
4. 确认动画或遮挡没有影响元素可见性

Quarantine recommendation: 在修复前暂时标记为 test.fixme()
```

## Browser Configuration

默认可在多浏览器运行：
- ✅ Chromium（桌面 Chrome）
- ✅ Firefox
- ✅ WebKit（桌面 Safari）
- ✅ Mobile Chrome（可选）

## CI/CD Integration

可在 CI 中接入 Playwright：

```yaml
# .github/workflows/e2e.yml
name: E2E

on:
  pull_request:
  push:
    branches: [main]
```

## PMX-Specific Critical Flows

对于示例项目，优先覆盖这些关键流程：
- 市场搜索与浏览
- 下单与确认
- 钱包连接
- 资金进出
- 登录与鉴权

## Best Practices

- 优先为关键旅程写测试，不要平均撒网
- 选择稳定的定位器，优先 `data-testid`
- 对动态内容使用显式等待，不依赖脆弱的时间延迟
- 每个测试只验证一个清晰目标
- 对 flaky tests 建立隔离与修复机制

## Important Notes

- E2E 测试应服务于关键业务路径，而不是替代所有单元测试
- 测试产物必须保留，方便失败时复盘
- 如果流程高度依赖外部服务，应准备 mock 或测试环境隔离方案

## Integration with Other Commands

- 先用 `/plan` 规划需要覆盖的用户路径
- 实现期间配合 `/tdd`
- 测试失败或构建异常时，使用 `/build-fix`
- 完成后用 `/code-review` 做最终检查

## Related Agents

此命令调用的 agent：
`~/.claude/agents/e2e-runner.md`

## Quick Commands

```bash
# 运行所有 E2E 测试
npx playwright test

# 运行指定文件
npx playwright test tests/e2e/markets/search.spec.ts

# 以 headed 模式运行
npx playwright test --headed

# 调试测试
npx playwright test --debug

# 从浏览器操作生成测试代码
npx playwright codegen http://localhost:3000

# 查看报告
npx playwright show-report
```
