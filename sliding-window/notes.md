# Sliding Window Pattern

Sliding window is a powerful technique used to reduce the time complexity of problems involving contiguous sequences (arrays, strings).

## 🔍 Core Idea
- Maintain a moving window of elements.
- Expand/shrink based on problem-specific constraints.

## 🧠 Typical Use Cases
- Longest substring/sequence
- Maximum sum/min/max within a window
- Pattern matching

## ⏱️ Time Complexity
Usually O(n) — each element is visited at most twice (once added, once removed).

## 📚 Related Patterns
- Two Pointers
- Monotonic Queue
- Prefix Sum (for optimization)

## 📎 Real-World Applications
- Rate limiting
- API Gateway token checks
- Stream processing (e.g., real-time analytics)

## 🧩 Composable With
- Hash Maps (for frequency/count)
- Deques (for monotonic queues)

## 🧵 From Code → Pattern → System
A sliding window isn’t just a problem-solving trick — it's a core concept in designing real-time processing systems.
