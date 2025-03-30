# Flink Quick Reference Card

## What is Flink?

- Flink 是一个 **分布式的、有状态的流处理框架**，适用于高吞吐、低延迟的实时数据处理。
- Flink is a **distributed, stateful stream processing framework**, designed for high-throughput and low-latency real-time data processing.

---

## Key Concepts

| 中文术语        | 英文术语              | 简要说明 |
|-----------------|-----------------------|----------|
| 有状态处理      | Stateful Processing    | 每个操作可以存储和更新状态，如计数、去重、窗口数据等 |
| 无界数据流      | Unbounded Stream       | 源源不断的事件流，无固定结束 |
| 时间语义        | Event Time / Processing Time | 支持基于事件时间（Event Time）处理和乱序处理 |
| 时间窗口        | Time Window            | 将流按时间划分片段进行聚合，例如滚动窗口、滑动窗口 |
| 键控分组        | KeyBy                  | 按键（如 driver_id 或 grid_id）分组进行状态管理 |
| 状态快照        | Checkpoint             | 周期性保存当前处理状态，用于故障恢复 |
| 精确一次语义    | Exactly-Once Semantics | 每个事件被精确处理一次，结合状态与输出一致性实现 |
| 流处理作业      | Streaming Job          | 运行在 Flink 上的应用管道，由多个算子组成 |


---
## Quick comparison Stateless vs Stateful
| 特性           | 无状态处理 (Stateless) | 有状态处理 (Stateful) |
|--------------|-------------------------|------------------|
| 处理模型         | 每个事件独立处理       | 依赖于之前的状态         |
| 例子           | 数据过滤、转换         | 滑动窗口聚合、去重        |
| 事件顺序         | 无需保证               | 需要保证             |
| 状态存储         | 无                     | 有                |
| 容错机制         | 无                     | Checkpoint       |
| Exactly-once | 无                     | 依赖 state 实现      |
| 处理延迟         | 较低                   | 较高               |
| 复杂性          | 较低                   | 较高               |




---



## Common Use Cases

- 实时聚合统计（如用户活跃度、位置热力图）
- 事件去重 / 滑动窗口处理
- 实时推荐 / 异常检测
- 多流 join（如订单 + 支付匹配）
- 日志处理、监控告警

---

## Kafka Integration

- Flink 通常 **从 Kafka 消费事件流（Kafka Source）**，进行清洗/聚合/窗口计算，再输出到 Redis, DB, 或 Kafka Sink。
- 它提供 **exactly-once Kafka connector**，支持偏移提交与状态对齐。

---

## How to talk about Flink in Interviews

> “Flink is a stateful stream processing framework. It’s especially powerful when we need to process unbounded data in real time — for example, aggregating driver locations into a heatmap. With Flink, we can group events by region, maintain windowed counts, and ensure exactly-once processing using its built-in checkpointing and state recovery mechanisms.”

或用中文回答：

> “Flink 是一个有状态的流处理系统，适用于需要实时聚合、窗口计算的场景。在 Uber 地图热力图的例子中，我们可以用 Flink 对 Kafka 中的司机位置事件进行分组、时间窗口聚合，并通过其 checkpoint 机制保证数据处理的准确性和容错性。”

---

## Summary

- Flink 最大的优势在于 **内建状态管理、窗口模型、低延迟和强一致性支持**；
- 不需要你用过，只要理解它的 **能力边界、适用场景、与 Kafka 等系统的关系**，在系统设计中就能自信地提出它。