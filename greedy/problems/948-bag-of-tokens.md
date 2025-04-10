# 948. Bag of Tokens

## 🧠 Problem Summary
Given tokens and an initial power, play tokens in one of two ways:
- Face-up: if your current power ≥ token[i], pay token[i] power, gain 1 point.
- Face-down: if your score ≥ 1, lose 1 point, gain token[i] power.

Maximize the final score.

## 🔍 Pattern
**Greedy + Sorting + Two Pointers**

## 💡 Optimal Approach
Sort tokens first. Use two pointers to decide:
- Left pointer (smallest token): buy with power to gain points.
- Right pointer (largest token): sell using points to gain power.

In each loop iteration, do only one operation (buy OR sell):
- Buy if you have enough power.
- Else, sell if you have at least 1 score.
- Else, break (can't buy or sell).

---

## Learning Process

### ❌ First attempt: Backtracking
- Initially tried backtracking to explore all possible sequences.
- Realized complexity was exponential (O(2^n)), impossible for large inputs.
- 回溯的复杂度是 O(2^n)，对于大输入来说不可能。

### ❌ Second attempt: Greedy with batch operations
```python
# Incorrect: Batch operations cause incorrect results.
while left <= right:
    while left <= right and power >= tokens[left]:
        power -= tokens[left]
        score += 1
        left += 1
    while right >= left and score >= 1:
        power += tokens[right]
        score -= 1
        right -= 1
```
- Failed test case: `[33,4,28,24,96]`, power = 35
- Mistake: batch-buy and batch-sell skip optimal intermediate states, 需要单步细粒度的控制，因为每次 sell/buy 都会影响后续的选择。
- **当前循环里“买尽可能多”然后“卖尽可能多”分两段进行，会错过在卖一次之后又能继续买小令牌的场景。**
换句话说，需要在同一层循环中灵活决定：“能买就买；买不了就卖一点再买”，而不能一口气把所有能买的都买光、再把能卖的都卖光。因为卖一次之后得到的 power 可能让你再次买更多小令牌。


### ✅ Optimal solution: Single-step greedy decisions
```python
def bagOfTokensScore(self, tokens: List[int], power: int) -> int:
    N = len(tokens)
    max_score = 0

    tokens.sort()
    left, right = 0, N - 1
    score = 0
    while left <= right:
        if power >= tokens[left]:
            power -= tokens[left]
            score += 1
            max_score = max(score, max_score)
            left += 1
        elif score >= 1:
            power += tokens[right]
            score -= 1
            right -= 1
        else:
            break

    return max_score

```
---

## 📊 Complexity
- Time: O(N log N) sorting + O(N) traversal → **O(N log N)**
- Space: O(1)


---

## 📌 Key Insights
- Optimal moves are always buying smallest or selling largest.
- Use sorting + two pointers to efficiently implement greedy choices.
- Single-step operations (one buy/sell per iteration) avoid missing intermediate optimal states.
- 洞察买/卖操作各自的最优局部行为：买时要最小，卖时要最大
- 排序+双指针 是一个满足这种“最小-最大组合”需求的常见范式
- 若你发现回溯 / DP 在此类问题中难度很大且可能超时，就会更加肯定该问题可能有更简单的贪心切入
- 经验与题型归纳：遇到“用资源换分数 / 用分数换更多资源”的时候，先想想能不能用“最小或最大优先”的贪心逻辑去简化。

## Key Heuristic: “If I can’t buy more, maybe I can sell something big.”
在实际业务或算法场景中，如果你发现一个资源（score）可以兑换另一种资源（power），并且兑换效率取决于你选的对象大小，那往往可以考虑一个“先排序→双指针”类型的贪心：

排序将“资源对象”分成由小到大 (或大到小) 两部分

遇到“买不了”时，就尝试“卖掉最大的”来换取足够资源，再继续买更多

结束条件：买不到且没有分数可用时

---

## ✨ Practice
- Train intuition by solving similar greedy problems:
  - [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)
  - [881. Boats to Save People](https://leetcode.com/problems/boats-to-save-people/)
