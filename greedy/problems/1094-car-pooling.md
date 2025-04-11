# 1094. Car Pooling

## Problem Summary
Given a list of trips `[num_passengers, start_location, end_location]`, and a car with a fixed capacity:
Determine whether it's possible to pick up and drop off all passengers without exceeding the car’s capacity at any time.

## Pattern
Greedy + Min-Heap (or Line Sweep / Difference Array)

## Approach 1: Min-Heap (Meeting Rooms Pattern)
1. Sort trips by start location.
2. Use a min-heap to track ongoing trips, where the heap stores `(end_location, passengers)`.
3. For each trip:
   - Remove any finished trips from the heap (`end <= start`).
   - Add the current trip to the heap and update current usage.
   - If at any point usage exceeds capacity, return False.

```python
import heapq

def carPooling(trips, capacity):
    trips.sort(key=lambda x: x[1])
    heap = []  # (end, passengers)
    usage = 0

    for p, start, end in trips:
        while heap and heap[0][0] <= start:
            _, prev = heapq.heappop(heap)
            usage -= prev

        heapq.heappush(heap, (end, p))
        usage += p

        if usage > capacity:
            return False

    return True
```

## Approach 2: Line Sweep (Difference Array)
1. For each trip, mark:
   - `+p` at `start`, `-p` at `end`
2. Sort all events.
3. Accumulate passenger change and ensure it never exceeds capacity.

```python
def carPooling(trips, capacity):
    events = []
    for p, start, end in trips:
        events.append((start, p))
        events.append((end, -p))

    events.sort()
    usage = 0

    for _, delta in events:
        usage += delta
        if usage > capacity:
            return False

    return True
```

## My Learning Notes
- My first instinct was to apply the meeting-room-style heap approach.
- It worked, but I initially had redundant counters.
- Later I discovered line sweep (difference array) which is more concise and cache-friendly.
- Heap-based version generalizes well to other allocation problems.

## Key Insights
- Heap-based solution is ideal if you need to track overlapping time intervals.
- Line sweep is elegant and highly efficient for purely additive/subtractive capacity tracking.
- Sorting events or intervals upfront allows for efficient greedy simulation.

## Time and Space Complexity
- Time: O(n log n) for sorting events or intervals
- Space: O(n) for event list or heap

## Follow-Up
- How to track exact passenger positions or assign them to seats?
- What if you need to minimize the number of stops instead of validate?
