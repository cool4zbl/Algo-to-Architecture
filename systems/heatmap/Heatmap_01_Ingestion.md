# Heatmap Design – Module 01: Ingestion Layer

tags: #streaming / #event-driven / #rate-limiting / #geo-spatial

## Problem

We need to handle frequent, high-volume location updates from drivers using a mobile app. Each update contains GPS coordinates and a timestamp. These events must be collected reliably and made available for real-time and batch processing downstream.

## Input

Event Schema Design(LocationUpdateEvent):

```json
{
  "driver_id": "abc123",
  "lat": 52.378,
  "lon": 4.900,
  "timestamp": 1711490415
}

```
- Precision: optional rounding / geo-binning (e.g., GeoHash)	
- Timestamp: server-side or device-side?	
- Payload Size: consider compression / batching for mobile data limits

## Responsibilities

- Receive high-frequency GPS updates from thousands of drivers
- Ensure events are reliably ingested without loss or duplication
- Decouple frontend (driver app) from backend consumers
- Support horizontal scaling for ingestion pipeline

## Architecture

1. Driver App periodically pushes location updates via HTTP/gRPC to a Collector Service
2. Collector validates and formats the event
3. Collector asynchronously publishes events to a Kafka topic
4. Kafka serves as a buffer and fan-out point to downstream consumers

## Technologies

- Protocol: HTTP or gRPC
- Message Queue: Apache Kafka (or Google PubSub / AWS Kinesis)
- Message Format: JSON or Protobuf
- Optionally use load balancer or API gateway (e.g., NGINX, Envoy)

## Key Design Patterns (Ingestion Layer)

| Pattern                      | Purpose                                             |
|-----------------------------|-----------------------------------------------------|
| Queue-Based Load Leveling   | Decouple ingestion spikes from processing speed     |
| Competing Consumers         | Allow parallel consumer instances for scalability   |
| Idempotency                 | Ensure retry-safe event handling (deduplication)    |
| Rate Limiting / Throttling  | Protect system from overload due to rapid updates from rogue clients / bad GPS devices |

## Key Trade-off / Design Questions

- How frequently should drivers push updates (every 2–5 seconds)?
- How to control update frequency? (enforce client-side and server-side throttling)
- How to handle network failures or retries?
- Rea-time freshness expectations? (e.g. within 1-2 sec latency)
- Is ordering required per driver or globally?

## 🧭 Design Tips / Personal Reflections
- Consider using geo-hashing or grid rounding for lat/lon to reduce payload size and improve aggregation efficiency.
- Collect metadata (device info, GPS accuracy) for quality control.
- Start simple with 1 Kafka topic and scale with partitioning strategy (e.g., by driver_id or geo region).
- 把“高频行为”先抽象成“标准化事件流”，这是系统设计关键第一步； 
- Kafka 是一个天然的“流中台”（streaming buffer），很适合 decouple； 
- 不要急着做聚合，先把 ingestion 流设计 clean，后面才能 scale； 
- 这一层做对了，系统下限就稳了

## Next Steps (for future cards)

- [ ] Streaming Aggregator → Real-time grid binning + sliding window
- [ ] Redis Materialized View + Cache Invalidation
- [ ] Query API (geo bounding box, map overlay)
- [ ] Cold Storage → S3 + Batch ETL for trend analysis
