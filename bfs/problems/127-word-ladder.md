# 127. Word Ladder

## Problem Summary
Given `beginWord`, `endWord`, and a word list, return the number of steps in the shortest transformation sequence from `beginWord` to `endWord`, such that:
- Only one letter can be changed at a time.
- Each transformed word must exist in the word list.

Return 0 if no such sequence exists.

## 🔍 Pattern
**BFS (Breadth-First Search)** + **Graph traversal with implicit edge building**

## 💡 Approach

两个单词组建一个路径，主要是找最短路径。
以及 hash 模式匹配。

We treat each word as a node. Two words are connected if they differ by exactly one character. We search for the shortest path from `beginWord` to `endWord` using BFS.

However, building the graph with pairwise comparisons of all words is too slow (O(n² · L)). To optimize:

### 🔥 Key Idea:
**Use wildcard pattern matching to pre-connect words.**
- For each word, replace each letter with `*` to create patterns (e.g., `h*t`, `*it`, `hi*`)
- Store: `pattern → list of matching words`
- During BFS, for each popped word, generate all its patterns, and use these patterns to get next candidates in O(L) time per word

---

## 🪜 Learning Process

### ❌ My first try:
```python
for i in range(len(words)):
    for j in range(i+1, len(words)):
        if diff(words[i], words[j]) == 1:
            graph[words[i]].add(words[j])
```
- Time Limit Exceeded on large inputs
- O(n² · L) complexity when n is large (e.g., 5000 words)

### ✅ Optimized version:
```python
from collections import defaultdict, deque

def ladderLength(beginWord: str, endWord: str, words: List[str]) -> int:
    patternMap = defaultdict(list)
    # O(n * L^2)
    for word in words: # O(n)
        for i in range(len(word)): # O(L)
            patternMap[word[:i] + '*' + word[i+1:]].append(word) # O(L) to construct word str

    queue = deque([(beginWord, 1)]) # beginWord is step 1 since it might not in the list
    visited = { beginWord }

    while queue:
        word, steps = queue.popleft()
        if word == endWord:
            return steps

        for i in range(len(word)):
            pat = word[:i] + '*' + word[i+1:]
            for neighbor in patternMap[pat]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append((neighbor, steps + 1))
            patternMap[pat].clear()  # Optimization: Avoid repeated lookup
```

### ✅ Insights:
- BFS guarantees the shortest path
- Using pattern-based lookup avoids O(n²) comparisons
- Small Optimizations:
    - `visited` prevents loops
    - `patternMap[pat].clear()` avoids revisiting the same pattern multiple times 
    - 在 BFS 中，每次访问完一个单词对应的 pat 列表后，可以把 patternMap[pat] 清空（patternMap[pat].clear()）。这样下次如果别的单词共享这个 pattern 时，就不会再重复迭代出相同的 neighbors

---

## 📌 Time & Space Complexity
- Preprocessing: O(n · L²)
- BFS: O(n · L²)
- Total: ~O(n · L²), where n = number of words, L = word length

---

## ✨ Follow-up
- Try [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/), which requires reconstructing all shortest paths
