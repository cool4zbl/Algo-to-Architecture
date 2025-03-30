# 239. Sliding Window Maximum

## 🧠 Problem Summary
Given an array `nums` and a sliding window of size `k`, return the maximum value in each sliding window as it moves from left to right.

## 🛠️ Pattern
Sliding Window + Monotonic Queue (Deque)

## 💡 Approach
- Use a deque to store **indices** of elements in a way that ensures the **front always contains the index of the max** value in the current window.
- Before adding a new element:
    - Remove indices that are **out of the current window** (i.e. `i - k`)
    - Remove indices of elements **smaller than the current value**, because they're no longer useful
- Append result when the window reaches size `k`

## ✅ Clean Implementation
```python
from collections import deque
from typing import List

def maxSlidingWindow(nums: List[int], k: int) -> List[int]:
    q = deque()
    res = []

    for right, num in enumerate(nums):
        # Remove out-of-window indices
        while q and q[0] <= right - k:
            q.popleft()

        # Maintain decreasing order in queue
        while q and nums[q[-1]] < num:
            q.pop()

        q.append(right)

        # Append result once the window hits size k
        if right >= k - 1:
            res.append(nums[q[0]])

    return res
```

## 💬 My Attempt
```python
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    q = deque()
    res = []
    for right, num in enumerate(nums):
        while q and q[0] <= right - k:
            q.popleft()

        while q and num > nums[q[0]]:  # ❌ wrong: should be q[-1], not q[0]
            q.popleft()
        q.append(right)
        if right >= k - 1:
            res.append(nums[q[0]])
    return res
```

## 🛠️ Bug Analysis
- You're comparing with `q[0]`, which is the **front** (i.e., current max).
- But when building the queue, you need to compare with **q[-1]** — the **last** element — to maintain **monotonic decreasing order**.

## 📝 My Notes
- 这题很好地将「数据结构（deque）」与「滑窗逻辑」融合，是“系统级”滑窗的代表题。
- 可以类比到实时系统中维护“最近一段时间内的最大/最小值”的窗口。
