# 567. Permutation in String

## 🧠 Problem Summary
Given two strings `s1` and `s2`, return `True` if `s2` contains a permutation of `s1`, or `False` otherwise.

In other words, check if one of `s1`'s permutations is a substring of `s2`.

## 🛠️ Pattern
Fixed-length Sliding Window + Frequency Array (size 26)

## 💡 Approach
- Count the frequency of each character in `s1` using a size-26 array (since the input contains only lowercase letters).
- Slide a window of size `len(s1)` over `s2`, maintaining a similar frequency array for the current window.
- Compare both arrays; if equal, it means the window is a permutation of `s1`.

## ✅ Correct Implementation
```python
def checkInclusion(s1: str, s2: str) -> bool:
    if len(s2) < len(s1):
        return False

    need = [0] * 26
    for c in s1:
        need[ord(c) - ord('a')] += 1

    window = [0] * 26
    left = 0
    for right, c in enumerate(s2):
        window[ord(c) - ord('a')] += 1

        if right - left + 1 > len(s1):
            window[ord(s2[left]) - ord('a')] -= 1
            left += 1

        if window == need:
            return True

    return False
```

## 💬 My Attempt
```python
def checkInclusion(self, s1: str, s2: str) -> bool:
    if len(s2) < len(s1):
        return False

    need = [0] * 26
    chars_len = len(Counter(s1))  # ❌ bug: this is number of unique chars, not window size
    for c in s1:
        need[ord(c) - ord('a')] += 1

    window = [0] * 26
    left = 0
    for right, c in enumerate(s2):
        window[ord(c) - ord('a')] += 1
        if right - left + 1 < chars_len:  # ❌ bug: should compare with len(s1)
            continue
        if tuple(window) == tuple(need):
            return True
        left += 1
        window[ord(s2[left]) - ord('a')] -= 1  # ❌ bug: should decrement before incrementing left

    return False
```

## 🔍 Bug Analysis
- Compared with wrong window size (`chars_len` vs `len(s1)`)
- Decremented `window[...]` **after** incrementing `left`, causing an off-by-one error
- `tuple()` comparison unnecessary; list comparison is fine in Python

## 📝 My Notes
- 本质是「固定长度滑窗 + 频次匹配」，和最小窗口类题不同。
- 思路简单，但细节易错（长度判断、窗口更新顺序）
- 很适合训练对滑动窗口结构的掌握：左移、右移、频次变化、窗口长度一致判断等技巧。