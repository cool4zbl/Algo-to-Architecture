# 76. Minimum Window Substring

## 🧠 Problem Summary
Given two strings `s` and `t`, return the minimum window in `s` which will contain all the characters in `t` (including duplicates). If no such window exists, return an empty string.

## 🛠️ Pattern
Sliding Window + Hash Map + Two Pointers

## 💡 Approach
1. Use a hashmap `need` to count required frequency of characters in `t`
2. Use a sliding window `[left, right]` and count characters using `window`
3. When a character in the window meets the frequency in `need`, increment `have`
4. When `have == len(need)`, try to shrink the window while maintaining the condition
5. Track the minimum length and starting index

## 🔥 Common Pitfalls
- ❌ Comparing dicts directly: `if window == need` (not robust)
- ❌ Blindly doing `have -= 1` without checking if it broke the match
- ❌ Wrong updates to `left`, or shrinking window too early

## ✅ Correct Implementation

```python
from collections import Counter, defaultdict

def minWindow(s: str, t: str) -> str:
    if len(t) > len(s):
        return ''

    need = Counter(t)
    window = defaultdict(int)
    have = 0
    min_len = float('inf')
    res = [-1, -1] # to store the result since left/right is sliding

    left = 0
    for right, c in enumerate(s):
        window[c] += 1
        if c in need and window[c] == need[c]:
            have += 1

        # shrink the window if we have all characters
        while have == len(need):
            if (right - left + 1) < min_len:
                min_len = right - left + 1
                res = [left, right]

            left_char = s[left]
            window[left_char] -= 1
            if left_char in need and window[left_char] < need[left_char]:
                have -= 1
            left += 1

    l, r = res # unpack the result from res
    return s[l:r+1] if min_len != float('inf') else ''
```

## 🧪 Time Complexity
- O(n) time, O(m) space — `n` is length of `s`, `m` is length of `t`

## 📝 My Notes
- 太经典的 sliding window 模板题，细节很多，要理解而不是死记。
- 可以想象为一个“字符平衡器”：什么时候加入了满足条件的字符，什么时候破坏了条件。
- 有时候可以用 `Counter` 代替 `defaultdict(int)`，但要注意 Counter 会自动删除 value 为 0 的 key。
- 系统设计，**实时数据流处理**，比如窗口内的数据聚合、窗口内的数据过滤等, 也可以类比为 **实时数据流处理系统** 中的 **窗口函数**
- 应用：比如实时关键词报警、窗口内条件满足触发器等
