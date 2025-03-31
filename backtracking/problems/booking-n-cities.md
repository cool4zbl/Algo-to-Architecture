# A Tale of N Cities

## Problem Summary
Guests want to visit all cities listed. Each city has a list of daily hotel prices indexed by trip day (global timeline). The guest must spend `D = numberOfDaysPerCity` days in each city. The total trip has `N × D` days (where N = number of cities).

The guest can visit the cities in any order. When a city is assigned to a segment of days (e.g., days 0~1), we fetch that city's prices on those exact days. Total trip cost = sum of prices from all cities over their assigned days.

**Output**

Sorted list (asc) of prices for the trips matching the guest's budget

**Example**
```
Input: {
    "numberOfDaysPerCity": 2,
    "guestBudget": 309,
    "dailyPricePerCity": {
    "Paris" : [10,40,5,80, 10,50],
    "London": [60,30,20,70,50,70],
    "Amsterdam" : [20,80,20,50,80, 100]
    }
}
Output:
[220,240,250,305]
```

## Pattern
Backtracking + Permutations + Indexed Range Mapping

## 💡 Approach
1. Total trip length = `D × N`
2. For each permutation of the city list:
   - Assign the i-th city to global days `[i×D, (i+1)×D - 1]`
   - Fetch that city's prices at those days
   - Sum total trip cost
3. If the sum ≤ budget, keep it.

## ✅ Correct Implementation
```python
from itertools import permutations

def find_valid_trip_costs(days_per_city, budget, daily_price_per_city):
    cities = list(daily_price_per_city.keys())
    result = set()
    N = len(cities)

    for city_order in permutations(cities):
        total_cost = 0
        for i, city in enumerate(city_order):
            start_day = i * days_per_city
            end_day = start_day + days_per_city
            total_cost += sum(daily_price_per_city[city][start_day:end_day])
        if total_cost <= budget:
            result.add(total_cost)

    return sorted(result)
```

## 💬 My Attempt (Backtracking version)
```python
def lowestCostTrip(numOfDaysPerCity, guestBudget, dailyPricePerCity):
    res = set()
    city_count = len(dailyPricePerCity)
    cities = list(dailyPricePerCity.keys())

    def backtrack(running_sum, dayIndex, selectedCity):
        if running_sum > guestBudget:
            return
        if len(selectedCity) == city_count:
            res.add(running_sum)
            return

        for city in cities:
            if city in selectedCity:
                continue

            selectedCity.add(city)
            city_cost = sum(dailyPricePerCity[city][dayIndex:dayIndex + numOfDaysPerCity])
            backtrack(running_sum + city_cost, dayIndex + numOfDaysPerCity, selectedCity)
            selectedCity.remove(city)

    backtrack(0, 0, set())
    return sorted(res)
```

## 🧪 Time Complexity
- City permutations: `O(N!)`
- For each permutation: `O(N × D)`
- Total: `O(N! × N × D)`

## ✨ Notes
- 本题虽然不是经典的排列组合回溯题，但它是典型的“全排列 + 区间索引 + 条件约束”模型。
- 不是典型回溯题，没有那种“标准套路感”, “城市顺序 → 行程日段 → 成本计算”这个组合空间是一个排列问题，且每一步有决策依赖 同时带约束（预算）→ 可以剪枝 → 才能想到 backtracking
