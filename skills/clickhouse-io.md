---
name: clickhouse-io
description: 面向高性能分析场景的 ClickHouse 数据模式、查询优化、分析建模与数据工程最佳实践。
---

# ClickHouse Analytics Patterns

适用于高吞吐分析场景的 ClickHouse 模式与实践。

## 概览

ClickHouse 是面向 OLAP 的列式数据库，适合：
- 大规模分析查询
- 事件流与埋点分析
- 漏斗、留存、队列聚合
- 实时看板与报表

**核心特点：**
- 列式存储
- 高压缩率
- 并行执行
- 分布式查询
- 适合追加写入

## Table 设计 Patterns

### MergeTree Engine (Most Common)

```sql
CREATE TABLE markets_analytics (
    date Date,
    market_id String,
    volume UInt64,
    trades UInt32,
    created_at DateTime
) ENGINE = MergeTree()
PARTITION BY toYYYYMM(date)
ORDER BY (date, market_id)
SETTINGS index_granularity = 8192;
```

适合大多数事实表。

### ReplacingMergeTree (Deduplication)

```sql
CREATE TABLE user_events (
    event_id String,
    user_id String,
    event_type String,
    timestamp DateTime
) ENGINE = ReplacingMergeTree()
PARTITION BY toYYYYMM(timestamp)
ORDER BY (user_id, event_id, timestamp);
```

适合多源写入、可能重复的事件数据。

### AggregatingMergeTree (Pre-aggregation)

```sql
CREATE TABLE market_stats_hourly (
    hour DateTime,
    market_id String,
    total_volume AggregateFunction(sum, UInt64),
    total_trades AggregateFunction(count, UInt32)
) ENGINE = AggregatingMergeTree()
PARTITION BY toYYYYMM(hour)
ORDER BY (hour, market_id);
```

适合预聚合，减少实时大查询成本。

## Query Optimization Patterns

### Efficient Filtering

```sql
-- ✅ 优先利用分区与排序键
SELECT *
FROM markets_analytics
WHERE date >= '2025-01-01'
  AND market_id = 'market-123'
ORDER BY date DESC
LIMIT 100;
```

建议：
- 先按分区裁剪，再按排序键过滤
- 少用无法命中索引的模糊条件
- 避免无上限的大范围扫描

### Aggregations

```sql
SELECT
    toStartOfDay(created_at) AS day,
    market_id,
    sum(volume) AS total_volume,
    count() AS total_trades,
    uniq(trader_id) AS unique_traders
FROM trades
WHERE created_at >= today() - INTERVAL 7 DAY
GROUP BY day, market_id
ORDER BY day DESC;
```

优先使用 ClickHouse 自带聚合函数，而不是把明细拉回应用层处理。

### Window Functions

适合：
- 排名
- 移动平均
- 累积值
- 分组内比较

## Data Insertion Patterns

### Bulk Insert (Recommended)

批量写入优于逐条写入，尤其是事件与日志数据。

建议：
- 按批次写
- 统一 schema
- 做幂等或去重设计

### Streaming Insert

适用于实时埋点流，但要控制小批次数量，避免过多碎片 part。

## Materialized Views

### Real-time Aggregations

用物化视图把明细流实时汇总成：
- 小时统计
- 用户级聚合
- 漏斗阶段数据
- 市场维度面板数据

## Performance Monitoring

### Query Performance

重点观察：
- query latency
- scanned rows / bytes
- memory usage
- read amplification

### Table Statistics

关注：
- part 数量
- 分区大小
- TTL 清理效果
- merge 压力

## Common Analytics Queries

### Time Series Analysis

适合：
- DAU / WAU / MAU
- GMV / volume 趋势
- 错误数趋势

### Funnel Analysis

适合：
- 访问 → 注册 → 激活 → 支付
- 曝光 → 点击 → 下单

### Cohort Analysis

适合：
- 用户留存
- 首次交易后复访
- 版本上线后的行为变化

## Data Pipeline Patterns

### ETL 模式

流程通常为：
1. 提取源数据
2. 清洗与标准化
3. 写入事实表 / 维度表
4. 生成预聚合表

### Change Data Capture (CDC)

适合：
- 从业务数据库同步分析库
- 追踪订单、交易、用户状态变更

## Best Practices

### 1. Partitioning 策略
- 用时间字段分区最常见
- 分区不要过细，否则 part 太多

### 2. Ordering Key
- 优先选择高频过滤 / 聚合字段
- 按查询模式设计，不按直觉设计

### 3. Data Types
- 尽量用更紧凑的类型
- 日期、枚举、整数优先

### 4. Avoid
- 高频小批写入
- 大量 `SELECT *`
- 无限制扫描历史全量数据

### 5. Monitoring
- 持续观察慢查询
- 监控 part 数与 merge 压力
- 对热点看板提前建预聚合

---

**原则**：ClickHouse 的性能来自“正确建模 + 正确查询”。不要把它当事务数据库使用，而应把它当高吞吐分析引擎来设计。
