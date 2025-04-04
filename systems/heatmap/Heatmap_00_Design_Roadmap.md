# 🧭 System Design – Heatmap Service (Module Roadmap)

A real-time geo-location heatmap system for visualizing driver density on a map, similar to what Uber or delivery platform use.


---
Key Functional Requirements

•	Drivers periodically upload their current GPS location.

•	Backend aggregates recent location updates into a heatmap.

•	Client can query and visualize heat zones in near real-time.

•	Support both recent (e.g. last 5 min) and historical data analysis.

---

## 📦 Module Breakdown

### [01 - Ingestion Layer](./Heatmap_01_Ingestion.md)
- Receive driver GPS updates
- Push to Kafka stream
- Key patterns: Queue-Based Load Leveling, Competing Consumers

### [02 - Real-time Aggregation](./Heatmap_02_Aggregation.md)
- Streaming count per geo grid
- Time windowing (5min/1min)
- Technologies: Flink / Spark / Redis
- Key patterns: Sliding Window, Materialized View

### [03 - Caching & Query API](./Heatmap_03_Cache_QueryAPI.md)
- Expose REST API for frontend query
- Use Redis to serve latest heatmap snapshot
- Handle cache invalidation, data TTL

### [04 - Historical Analytics](./Heatmap_04_BatchAnalytics.md)
- Write raw events to cloud storage
- Scheduled jobs to process historical mobility trends
- Key patterns: Batch Processing, Data Lake, Scheduled Job

---

## Architecture Sketch (optional)
> You can insert a diagram or link to one here.

---

## 🧠 Key Design Themes
- High-frequency event ingestion → needs reliable queue + scalable consumer model
- Balance between real-time freshness and eventual consistency
- Use pattern layering: Streaming + Caching + Storage

---

## ⏭️ Next Ideas
- Add user-specific queries (driver location history)
- Introduce alerting system (if area overcrowded)
- Multi-region scaling & failover
