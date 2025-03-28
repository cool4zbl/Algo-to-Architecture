# 3. Longest Substring Without Repeating Characters

## 🧠 Problem Summary
Given a string `s`, find the length of the longest substring without repeating characters.

## 🛠️ Pattern
Sliding Window + Hash Map

## 💡 Approach
```python
if window[c] >= left:
    left = window[c] + 1
```
1. Maintain a sliding window using `[left, right]`.
2. Use a hashmap to track the **last seen index** of each character.
3. If a character is repeated **within the current window**, move `left` to `last_index + 1`.
4. Update `max_len` with the current valid window size.

##  Clean Implementation
```python
def lengthOfLongestSubstring(s: str) -> int:
    window = {}
    left = 0
    max_len = 0

    for right, c in enumerate(s):
        if c in window and window[c] >= left:
            left = window[c] + 1  # shrink the window to avoid duplicate
        window[c] = right
        max_len = max(max_len, right - left + 1)

    return max_len
```

## 💬 My Attempt
```python
from collections import defaultdict

def lengthOfLongestSubstring(self, s: str) -> int:
    window = defaultdict(int)
    left = 0
    max_len = float('-inf')

    for right, c in enumerate(s):
        if c not in window:
            window[c] = right
        else:
            if window[c] >= left:
                left = window[c] + 1
            window[c] = right
        max_len = max(max_len, right - left + 1)

    return 0 if max_len == float('-inf') else max_len
```

## ✨ Notes
- 你的解法完全正确，只需稍微精简下结构。
- 推荐不要使用 `defaultdict(int)`，因为我们其实是在做索引更新，不是累加计数。
- 初始化 `max_len = 0` 更语义化，也避免最后 return 条件判断。