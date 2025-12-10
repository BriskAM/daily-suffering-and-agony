# DSA Study Assistant Agent Guidelines

## Purpose

This agent helps manage and organize your Data Structures and Algorithms (DSA) practice by:

1. **Storing solutions** in properly categorized folders
2. **Creating comprehensive notes** with technique analysis
3. **Building a knowledge graph** using Obsidian-style links
4. **Documenting reusable methods/patterns** for quick reference
5. **Creating Manim animations** for complex algorithm visualizations

---

## Directory Structure

```
daily-suffering-and-agony/
├── GEMINI.md                    # This file - Agent guidelines
├── solutions/                   # All problem solutions
│   ├── arrays/
│   ├── strings/
│   ├── linked-lists/
│   ├── trees/
│   ├── graphs/
│   ├── dynamic-programming/
│   ├── backtracking/
│   ├── binary-search/
│   ├── two-pointers/
│   ├── sliding-window/
│   ├── stack-queue/
│   ├── heap/
│   ├── greedy/
│   ├── bit-manipulation/
│   ├── math/
│   └── miscellaneous/
├── notes/                       # Topic and technique notes
│   ├── techniques/              # Algorithmic techniques
│   ├── patterns/                # Common problem patterns
│   └── concepts/                # Core CS concepts
├── methods/                     # Documentation for reusable methods/utilities
│   ├── collections/             # Python collections module
│   ├── itertools/               # Python itertools module
│   ├── built-ins/               # Built-in functions and utilities
│   └── custom/                  # Custom helper functions
├── animations/                  # Manim animations for complex problems
│   ├── trees/
│   ├── graphs/
│   ├── dp/
│   └── sorting/
└── index.md                     # Master index with links to everything
```

---

## Workflow: When User Provides a Problem + Solution

### Step 1: Analyze the Solution

- Identify the **primary technique** used (e.g., Two Pointers, Sliding Window, DP)
- Identify **secondary techniques** or helper methods (e.g., Counter, defaultdict, sorting)
- Determine the **time and space complexity**
- Note any **edge cases** handled

### Step 2: Categorize and Store the Solution

Create the solution file at: `solutions/<category>/<problem-name>.md`

**Solution File Template:**

```markdown
# Problem Name

**Source:** LeetCode/CodeForces/etc. #problem-number  
**Difficulty:** Easy/Medium/Hard  
**Date Solved:** YYYY-MM-DD

## Problem Statement

[Brief description or link to the problem]

## Approach

**Primary Technique:** [[techniques/<technique-name>]]  
**Secondary Techniques:** [[methods/<module>/<method-name>]], ...

[Explanation of the approach in your own words]

## Solution

```python
# Your solution code here
```

## Complexity Analysis

- **Time Complexity:** O(?)
- **Space Complexity:** O(?)

## Key Insights

- Insight 1
- Insight 2

## Related Problems

- [[solutions/<category>/<related-problem-1>]]
- [[solutions/<category>/<related-problem-2>]]

## Techniques Used

- [[techniques/<technique-1>]]
- [[methods/<module>/<method-1>]]
```

### Step 3: Create/Update Notes

If this is a new technique or pattern, create a note for it.  
If it already exists, add this problem as an example.

**Technique Note Template:** (`notes/techniques/<technique-name>.md`)

```markdown
# Technique Name

## What is it?

[Clear explanation of the technique]

## When to Use

- Condition 1
- Condition 2
- Key indicators that suggest this technique

## Template/Pattern

```python
# Generic template code
```

## Problems Using This Technique

- [[solutions/<category>/<problem-1>]] - Brief note
- [[solutions/<category>/<problem-2>]] - Brief note

## Related Techniques

- [[techniques/<related-technique>]]

## Tips & Pitfalls

- Tip 1
- Common mistake to avoid
```

### Step 4: Document Methods/Utilities

When a Python method or utility is used, ensure it's documented.

**Method Documentation Template:** (`methods/<module>/<method-name>.md`)

```markdown
# Method Name (e.g., Counter)

**Module:** `collections`  
**Import:** `from collections import Counter`

## What is it?

[Explanation of what this method does]

## When to Use

- Use case 1
- Use case 2
- Key indicators

## Syntax & Examples

```python
# Basic usage
from collections import Counter

# Example 1
counter = Counter([1, 2, 2, 3, 3, 3])
# Counter({3: 3, 2: 2, 1: 1})

# Example 2 - most_common
counter.most_common(2)  # [(3, 3), (2, 2)]
```

## Common Operations

| Operation | Syntax | Description |
|-----------|--------|-------------|
| Count elements | `Counter(iterable)` | Creates frequency map |
| Most common | `.most_common(n)` | Returns n most frequent |
| ... | ... | ... |

## Problems Using This Method

- [[solutions/<category>/<problem-1>]]
- [[solutions/<category>/<problem-2>]]

## Related Methods

- [[methods/collections/defaultdict]]
- [[methods/built-ins/sorted]]
```

### Step 5: Update Links

After adding a new solution:

1. **Add backlinks** to related problems
2. **Update technique notes** with the new problem
3. **Update method documentation** with the new problem
4. **Update `index.md`** if it's a new category

---

## Obsidian Linking Strategy

### Link Types

| Link Type | Syntax | Purpose |
|-----------|--------|---------|
| Basic link | `[[filename]]` | Link to any note |
| Aliased link | `[[filename\|display text]]` | Custom display text |
| Header link | `[[filename#header]]` | Link to specific section |
| Block link | `[[filename#^block-id]]` | Link to specific block |

### Link Conventions

- **Solutions → Techniques:** Every solution links to techniques used
- **Solutions → Methods:** Every solution links to utility methods used
- **Solutions → Solutions:** Related problems are cross-linked
- **Techniques → Problems:** Technique notes list all problems using them
- **Methods → Problems:** Method docs list all problems using them

---

## Additional Features

### 1. Spaced Repetition Integration

Add a review section to solutions:

```markdown
## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-01-15 | 3/5 | Need to review edge cases |
| 2024-01-22 | 4/5 | Better understanding now |
```

### 2. Difficulty Progression Tracking

The agent will suggest problems in order of difficulty within each technique.

### 3. Pattern Recognition Hints

When you share a new problem, the agent will:

- Identify similar problems you've solved
- Suggest which technique might apply
- Point to relevant documentation

### 4. Weekly Review Generation

Generate a summary of:

- Problems solved this week
- New techniques learned
- Weak areas to focus on

### 5. Quick Reference Cards

Create concise cheat sheets for:

- Common patterns
- Time complexity reference
- Python tricks for DSA

### 6. Manim Animations for Complex Problems

For problems involving complex visualizations, the agent creates Manim animations:

**When to Create Animations:**
- Tree/Graph traversals (BFS, DFS, Dijkstra)
- DP table filling and state transitions
- Sorting algorithm step-by-step
- Pointer movement (two-pointers, sliding window)
- Recursion call stack visualization

**Animation File Template:** (`animations/<category>/<problem-name>.py`)

```python
from manim import *

class ProblemNameVisualization(Scene):
    def construct(self):
        # Animation code here
        pass
```

Each animation will be linked from the solution file:

```markdown
## Visualization

Animation: [[animations/<category>/<problem-name>]]
```

### 7. Time Tracking and Analytics

Track time spent on each problem:

```markdown
## Time Log

| Attempt | Date | Time Spent | Solved? |
|---------|------|------------|---------|
| 1 | 2024-01-15 | 45 min | No - TLE |
| 2 | 2024-01-16 | 20 min | Yes |
```

Generate analytics:
- Average time per difficulty level
- Time trends over weeks
- Identify slow areas needing practice

### 8. Interview Preparation Mode

Organize problems by company tags and frequency:

```markdown
## Interview Tracker

| Company | Problems Solved | Coverage |
|---------|-----------------|----------|
| Google | 15/50 | 30% |
| Amazon | 20/45 | 44% |
```

### 9. Alternative Solutions Comparison

Document multiple approaches for the same problem:

```markdown
## Alternative Approaches

### Approach 1: Brute Force
- Time: O(n^2), Space: O(1)
- [[techniques/brute-force]]

### Approach 2: Optimized with HashMap
- Time: O(n), Space: O(n)
- [[methods/collections/Counter]]

### Trade-off Analysis
When to use which approach...
```

### 10. Bug Pattern Tracking

Track common mistakes to avoid repeating them:

```markdown
## My Bug Patterns

| Bug Type | Frequency | Example Problems |
|----------|-----------|------------------|
| Off-by-one | 5 times | [[problem-1]], [[problem-2]] |
| Forgot edge case: empty input | 3 times | [[problem-3]] |
```

### 11. Problem Dependency Graph

For problems that build on each other:

```markdown
## Prerequisites
- [[solutions/dp/house-robber]] (basic DP)

## Unlocks
- [[solutions/dp/house-robber-iii]] (tree DP extension)
```

### 12. Code Template Library

Maintain reusable code snippets:

```markdown
# Binary Search Template

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

## Variants
- Lower bound: [[templates/lower-bound]]
- Upper bound: [[templates/upper-bound]]
- Search in rotated array: [[templates/rotated-binary-search]]
```

### 13. Contest Performance Log

Track competitive programming performance:

```markdown
## Contest Log

| Date | Platform | Contest | Rank | Problems Solved | Notes |
|------|----------|---------|------|-----------------|-------|
| 2024-01-14 | LeetCode | Weekly 379 | 1500 | 3/4 | Stuck on Q4 DP |
```

### 14. Revision Queue

Auto-generate a daily revision list based on:
- Problems marked "needs-review"
- Spaced repetition schedule
- Weak techniques identified

---

## Tags Convention

Use tags in frontmatter for better organization:

```yaml
---
tags:
  - difficulty/medium
  - technique/two-pointers
  - technique/sliding-window
  - pattern/subarray
  - company/google
  - status/needs-review
---
```

---

## Commands Quick Reference

When talking to the agent, you can say:

| Command | Action |
|---------|--------|
| "Here's a problem and my solution" | Full workflow: categorize, analyze, link |
| "Create notes for [technique]" | Create technique documentation |
| "Document [method]" | Create method documentation |
| "Link similar problems" | Find and cross-link related solutions |
| "Show me problems using [technique]" | List all problems with that technique |
| "Weekly review" | Generate progress summary |
| "What technique should I use for X?" | Get technique suggestions |
| "Create animation for [problem]" | Generate Manim visualization |
| "Show my bug patterns" | Display common mistakes |
| "Interview prep for [company]" | Filter problems by company |
| "Today's revision" | Get daily practice queue |

---

## Getting Started

1. Share your first problem and solution
2. The agent will create the folder structure
3. Notes and links will be created automatically
4. The knowledge graph will grow with each problem!

---

*Happy problem solving!*
