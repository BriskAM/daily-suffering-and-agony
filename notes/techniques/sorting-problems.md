# Sorting for Problem Transformation

## What is it?

A technique where sorting the input transforms a complex problem into a simpler, linearly-processable form. The sorted order establishes an invariant that enables greedy or sequential algorithms.

## When to Use

- Processing order matters but isn't given by input order
- Need to compare/merge adjacent or related elements
- "Closest", "next greater/smaller", or "preceding" relationships
- Interval problems (meetings, events)
- Fleet/merge problems where order determines collisions

**Key indicators:**
- Problem involves relative positions or times
- A slower/smaller/earlier element "blocks" later ones
- Need to process in a specific sequence

## Template/Pattern

```python
def solve_with_sorting(items):
    # Sort by relevant key (position, start time, value, etc.)
    sorted_items = sorted(items, key=lambda x: x.position, reverse=True)
    
    result = 0
    current_max = 0  # or other tracking variable
    
    for item in sorted_items:
        if item.value > current_max:
            # This item is a "leader" / "new group"
            current_max = item.value
            result += 1
    
    return result
```

## Problems Using This Technique

- [[solutions/stack-queue/car-fleet]] - Sort by position to process front-to-back
- [[solutions/arrays/merge-intervals]] - Sort by start time to merge overlapping
- [[solutions/arrays/meeting-rooms]] - Sort by start time to detect conflicts

## Related Techniques

- [[notes/techniques/greedy-comparison]] - Often used after sorting
- [[notes/techniques/monotonic-stack]] - Alternative approach for NGE problems

## Tips & Pitfalls

### Tips
- Descending sort when "blocking" happens from front to back
- Ascending sort when merging/extending happens left to right
- Consider what invariant the sorted order provides

### Pitfalls
- Forgetting to sort by the correct key
- Wrong sort direction (ascending vs descending)
- O(n log n) from sorting may dominate time complexity
