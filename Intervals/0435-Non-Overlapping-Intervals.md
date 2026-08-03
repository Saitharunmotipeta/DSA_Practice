# 435. Non-overlapping Intervals

## Pattern

- Intervals
- Greedy
- Sorting

---

## Problem

Given an array of intervals, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Two intervals overlap if

```
Current End > Next Start
```

Intervals touching at endpoints are **not** considered overlapping.

---

## Example

Input

```text
[[1,2],[2,3],[3,4],[1,3]]
```

Output

```text
1
```

Explanation

Remove

```text
[1,3]
```

Remaining intervals become

```text
[1,2]
[2,3]
[3,4]
```

which are non-overlapping.

---

## Test Cases

### Test Case 1

Input

```text
[[1,2],[2,3],[3,4],[1,3]]
```

Output

```text
1
```

---

### Test Case 2

Input

```text
[[1,2],[1,2],[1,2]]
```

Output

```text
2
```

---

### Test Case 3

Input

```text
[[1,2],[2,3]]
```

Output

```text
0
```

---

### Test Case 4

Input

```text
[[1,100],[2,3],[3,4],[4,5]]
```

Output

```text
1
```

---

## Brute Force

### Approach

- Try removing every interval.
- Check whether the remaining intervals overlap.
- Return the minimum removals.

### Complexity

Time Complexity

```
O(n²)
```

Space Complexity

```
O(1)
```

---

## Optimized Approach

### Observation 1

Sort intervals by their starting point.

This allows comparing only adjacent intervals.

---

### Observation 2

If

```
Current End > Next Start
```

the intervals overlap.

One of them must be removed.

---

### Observation 3

When two intervals overlap,

keep the interval that finishes first.

The interval with the smaller ending point leaves more room for future intervals.

---

## Algorithm

1. Sort intervals by starting point.
2. Store the first interval as Current.
3. Traverse remaining intervals.
4. If Current overlaps with Next
   - Increment removals.
   - Keep the interval with the smaller ending point.
5. Otherwise
   - Update Current to Next.
6. Return removals.

---

## Interview Explanation

The goal is not to merge intervals,

but to maximize the number of intervals that can remain.

Whenever two intervals overlap,

one interval must be removed.

To maximize future scheduling opportunities,

we always keep the interval that ends earlier.

This greedy choice guarantees the optimal answer because an earlier ending interval leaves more space for future non-overlapping intervals.

---

## Success Solution

```csharp
public class Solution {
    public int EraseOverlapIntervals(int[][] intervals) {

        Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

        int removals = 0;
        int i = 1;

        int[] current = intervals[0];

        while(i < intervals.Length){

            int[] next = intervals[i];

            if(current[1] > next[0]){

                removals++;

                if(next[1] < current[1]){
                    current = next;
                }
            }
            else{
                current = next;
            }

            i++;
        }

        return removals;
    }
}
```

---

## Success Template

```
Sort intervals by start

↓

Current = First Interval

↓

For every remaining interval

    Overlap?

        Yes

            Removals++

            Keep interval with smaller end

        No

            Current = Next

↓

Return Removals
```

---

## Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Overall Time Complexity

```
O(n log n)
```

Space Complexity

```
O(1)
```

---

## Revision Notes

- Sort intervals by starting point.
- Overlap condition:
  - Current End > Next Start
- Touching intervals are allowed.
- When overlapping:
  - Increment removals.
  - Keep the interval with the smaller ending point.
- Goal is to maximize future scheduling opportunities.
- Greedy choice:
  - Keep the earliest finishing interval.