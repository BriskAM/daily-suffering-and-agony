---
tags:
  - difficulty/medium
  - technique/sorting
  - technique/greedy
  - pattern/simulation
  - source/leetcode
  - status/solved
---

# Car Fleet

**Source:** LeetCode #853  
**Difficulty:** Medium  
**Date Solved:** 2024-12-28

## Problem Statement

There are `n` cars going to the same destination along a one-lane road. The destination is `target` miles away.

You are given two integer arrays `position` and `speed`, both of length `n`, where `position[i]` is the position of the `i`th car and `speed[i]` is the speed of the `i`th car (in miles per hour).

A car can never pass another car ahead of it, but it can catch up to it and drive bumper to bumper at the same speed. The faster car will slow down to match the slower car's speed.

A **car fleet** is some non-empty set of cars driving at the same position and same speed. Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it still counts as one fleet.

Return the number of car fleets that will arrive at the destination.

## Approach

**Primary Technique:** Sorting + Greedy  
**Secondary Techniques:** [[methods/built-ins/zip]], [[methods/built-ins/sorted]]

### Key Insight

The critical observation is that **a car closer to the destination blocks all cars behind it if they would catch up**. 

1. **Calculate time to reach target** for each car: `time = (target - position) / speed`
2. **Sort cars by position in descending order** (closest to target first)
3. **Iterate from front to back**: If a car takes longer to reach the target than the current "blocking" car, it becomes a new fleet leader

### Why This Works

- Cars are processed from closest to target → furthest
- A car with shorter time will catch up to the car ahead (merge into its fleet)
- A car with longer time can never catch up (starts a new fleet)
- We track `max_time` as the "blocking" time - any car with time ≤ max_time gets absorbed

## Solution

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        n = len(speed)
        time = [0] * n
        for i in range(n):
            time[i] = (target - position[i]) / speed[i]
        
        cars = list(zip(position, time))
        cars.sort(key=lambda x: x[0], reverse=True)
        
        fleets, max_time = 0, 0
        for car in cars:
            if car[1] > max_time:
                max_time = car[1]
                fleets += 1
        
        return fleets
```

### Alternative: More Concise Version

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        cars = sorted(zip(position, speed), reverse=True)
        fleets = max_time = 0
        
        for pos, spd in cars:
            time = (target - pos) / spd
            if time > max_time:
                max_time = time
                fleets += 1
        
        return fleets
```

## Complexity Analysis

- **Time Complexity:** O(n log n) - dominated by sorting
- **Space Complexity:** O(n) - for the `cars` array (or O(1) extra if we sort in place)

## Key Insights

1. **Processing order matters**: Sorting by position (descending) ensures we process "blocking" cars first
2. **No need for actual simulation**: Just compare arrival times
3. **Greedy works**: We don't need to track actual fleet merging - only count fleet leaders
4. **Float division is safe**: We're just comparing times, not indexing

## Visual Explanation

```
Target = 12

Position:  [10, 8, 0, 5, 3]
Speed:     [2,  4, 1, 1, 3]
Time:      [1,  1, 12, 7, 3]

Sorted by position (desc):
Car at 10: time = 1  → First fleet (max_time = 1)
Car at 8:  time = 1  → time ≤ max_time, merges with fleet
Car at 5:  time = 7  → time > max_time, NEW fleet (max_time = 7)
Car at 3:  time = 3  → time ≤ max_time, merges with fleet
Car at 0:  time = 12 → time > max_time, NEW fleet (max_time = 12)

Answer: 3 fleets
```

## Related Problems

- [[solutions/stack-queue/daily-temperatures]] - Monotonic stack (related pattern)
- [[solutions/arrays/meeting-rooms-ii]] - Interval-based fleet/merge problems
- [[solutions/greedy/merge-intervals]] - Similar "merging" concept

## Techniques Used

- [[notes/techniques/sorting-problems]] - Sort to establish processing order
- [[notes/techniques/greedy-comparison]] - Compare with running maximum
- [[methods/built-ins/zip]] - Pair positions with times
- [[methods/built-ins/sorted]] - Sort with custom key

## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-12-28 | 4/5 | Good understanding of the greedy approach |
