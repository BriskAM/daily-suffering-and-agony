# Greedy Comparison

## What is it?

A technique where you maintain a "running" value (max, min, or threshold) and compare each element against it to make locally optimal decisions. Elements that exceed the threshold trigger an action; those that don't are absorbed.

## When to Use

- Counting groups, leaders, or distinct segments
- Fleet/merge problems (cars, intervals)
- Finding optimal splits or partitions
- Tracking "blocking" elements

**Key indicators:**
- "How many distinct groups/fleets/leaders?"
- Elements can merge or be absorbed by others
- Need to track a running maximum/minimum

## Template/Pattern

```python
def greedy_count(items):
    # Assume items are sorted appropriately
    count = 0
    current_max = float('-inf')  # or 0, depending on problem
    
    for item in items:
        if item.value > current_max:
            # This item cannot be absorbed - it's a new leader
            current_max = item.value
            count += 1
        # else: item is absorbed by current leader
    
    return count
```

## Why It Works

**Greedy choice property:** If an element's value is ≤ current_max, it can never "escape" or form a separate group. Making the local decision to absorb it is globally optimal.

## Problems Using This Technique

- [[solutions/stack-queue/car-fleet]] - Compare arrival time with max blocking time

## Related Techniques

- [[notes/techniques/sorting-problems]] - Usually precedes greedy comparison
- [[notes/techniques/monotonic-stack]] - Alternative when order matters differently

## Tips & Pitfalls

### Tips
- Initialize running value carefully (0, -inf, first element)
- Strict `>` vs `>=` depends on problem (equality = merge or not?)
- Works well after sorting establishes processing order

### Pitfalls
- Using wrong comparison operator
- Forgetting edge case when array is empty
- Not sorting first when order matters
