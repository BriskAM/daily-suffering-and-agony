---
tags:
  - difficulty/medium
  - technique/monotonic-stack
  - pattern/next-greater-element
  - status/solved
---

# Daily Temperatures

**Source:** LeetCode #739  
**Difficulty:** Medium  
**Date Solved:** 2024-12-24

## Problem Statement

Given an array of integers `temperatures` representing daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day with a warmer temperature, keep `answer[i] == 0`.

## Approach

**Primary Technique:** [[techniques/monotonic-stack]]  
**Pattern:** Next Greater Element (NGE)

We use a **monotonic decreasing stack** that stores indices of temperatures. The key insight is:

1. Iterate through temperatures from left to right
2. For each temperature, pop all indices from the stack where the current temperature is greater (we found their "next warmer day")
3. The distance `i - prev` gives us the number of days to wait
4. Push current index onto the stack

The stack maintains a decreasing order of temperatures - whenever we find a warmer day, we've found the answer for all cooler days waiting in the stack.

## Solution

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        answer = [0] * n
        stack = []  # stores indices, not values

        for i in range(n):
            # Pop all days that found their warmer day
            while stack and temperatures[i] > temperatures[stack[-1]]:
                prev = stack.pop()
                answer[prev] = i - prev
            stack.append(i)

        return answer
```

## Complexity Analysis

- **Time Complexity:** O(n) - each element is pushed and popped at most once
- **Space Complexity:** O(n) - stack can hold all n indices in worst case (strictly decreasing temperatures)

## Key Insights

- Store **indices** in the stack, not values - this lets us calculate distances
- Monotonic decreasing stack: elements remaining in stack after iteration never found a warmer day (answer stays 0)
- Same pattern as "Next Greater Element" problems
- The while loop doesn't make it O(n²) because each element is processed at most twice (one push, one pop)

## Visual Walkthrough

```
temperatures = [73, 74, 75, 71, 69, 72, 76, 73]
indices:        0   1   2   3   4   5   6   7

Step-by-step:
i=0: stack=[] → push 0 → stack=[0]
i=1: 74>73 → pop 0, answer[0]=1 → push 1 → stack=[1]
i=2: 75>74 → pop 1, answer[1]=1 → push 2 → stack=[2]
i=3: 71<75 → push 3 → stack=[2,3]
i=4: 69<71 → push 4 → stack=[2,3,4]
i=5: 72>69 → pop 4, answer[4]=1
     72>71 → pop 3, answer[3]=2 → push 5 → stack=[2,5]
i=6: 76>72 → pop 5, answer[5]=1
     76>75 → pop 2, answer[2]=4 → push 6 → stack=[6]
i=7: 73<76 → push 7 → stack=[6,7]

Final: answer = [1, 1, 4, 2, 1, 1, 0, 0]
```

## Related Problems

- [[solutions/stack-queue/next-greater-element-i]]
- [[solutions/stack-queue/next-greater-element-ii]]
- [[solutions/stack-queue/largest-rectangle-in-histogram]]
- [[solutions/stack-queue/trapping-rain-water]]

## Techniques Used

- [[techniques/monotonic-stack]]
