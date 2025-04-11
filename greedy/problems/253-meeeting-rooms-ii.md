# 253. Meeting Rooms II

## Problem Summary
Given a list of meeting time intervals `[[start1, end1], [start2, end2], ...]`, determine the minimum number of meeting rooms required to hold all meetings without overlap.

## Pattern
Greedy + Min-Heap (Priority Queue) or Line Sweep (Difference Array)

## Approach 1: Min-Heap (Priority Queue)
1. Sort all intervals by their start time.
2. Use a min-heap to track the end times of ongoing meetings.
3. For each meeting:
   - If the earliest ending meeting ends before the current one starts, reuse the room (pop from heap).
   - Otherwise, add a new room (push into heap).
4. The size of the heap at the end is the number of meeting rooms needed.
 

```python
# TC: O(n log n) for sorting + O(n log n) for heap operations
# Space: O(n)
def minMeetingRooms(intervals):
    if not intervals:
        return 0

    intervals.sort(key=lambda x: x[0])
    heap = []

    for start, end in intervals:
        if heap and heap[0] <= start:
            heapq.heappop(heap)
        heapq.heappush(heap, end)

    return len(heap)
```

## Approach 2: Line Sweep (Difference Array)
1. Create a list of events: `(time, +1)` for meeting start, `(time, -1)` for meeting end.
2. Sort events by time. If times are equal, `+1` comes before `-1`.
3. Track the ongoing meeting count while iterating over events.
4. The maximum ongoing count is the number of rooms needed.

```python
# TC: O(n log n) total in sweep
# SC: O(n) for events
def minMeetingRooms(intervals):
    events = []
    for start, end in intervals:
        events.append((start, 1))
        events.append((end, -1))

    events.sort()  # Sort by time, start before end on tie

    rooms, max_rooms = 0, 0
    for _, delta in events:
        rooms += delta
        max_rooms = max(max_rooms, rooms)

    return max_rooms
```

## My Learning Notes
- My first attempt used a counter variable `room_count` and heap, which was redundant.
- Later learned that just tracking heap size is enough.
- The difference array / line sweep approach is simple, very readable, and often easier to reason about for overlapping interval problems.
- Meeting Rooms II and Car Pooling (1094) share the same structure: timeline-based capacity tracking.

## Key Insights
- If you're tracking **how many resources are needed concurrently**, it's a strong sign line sweep can be used.
- Min-heap helps when you need to dynamically reuse resources (like rooms or seats).
- Always sort by start time (or by time, for events) to preserve chronological correctness.
- Avoid manually managing resource counts when you can derive them from heap size or event deltas.



## Related Problems (Same Pattern)
- [1094. Car Pooling](./1094-car-pooling.md) — Capacity tracking on a line
- [759. Employee Free Time] — Find gaps in overlapping schedules (variation)
