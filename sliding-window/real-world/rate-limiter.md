# Real-World Application: Rate Limiter

## 🎯 Goal
Prevent clients from sending too many requests in a short time.

## 💡 Sliding Window Analogy
- Treat time as a sliding window.
- Count requests within a window (e.g., last 60 seconds).
- Allow if count < threshold; reject otherwise.

## 🔨 Implementation Approaches
- Fixed window (e.g., reset every minute)
- Sliding window (more accurate, costlier)
- Token bucket / Leaky bucket (smoothing, burstable)

## 📦 Components
- Redis / Memcached for state
- Timestamps or counters
- Middleware or Gateway filters

## 🔁 Related System Concepts
- API Gateway
- Distributed rate limiting
- Time series storage (for fine-grained sliding)

## 🔗 See Also
- [Token Bucket vs Leaky Bucket](./token-bucket-vs-leaky-bucket.md)
