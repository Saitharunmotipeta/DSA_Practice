# 56. Merge Intervals

## Pattern

- Intervals
- Sorting
- Greedy

---

## Problem

Given an array of intervals where each interval is represented as:

```
[start, end]
```

Merge all overlapping intervals and return the merged intervals.

### Example

Input

```text
[[1,3],[2,6],[8,10],[15,18]]
```

Output

```text
[[1,6],[8,10],[15,18]]
```

---

## Test Cases

### Test Case 1

Input

```text
[[1,3],[2,6],[8,10],[15,18]]
```

Output

```text
[[1,6],[8,10],[15,18]]
```

---

### Test Case 2

Input

```text
[[1,4],[4,5]]
```

Output

```text
[[1,5]]
```

---

### Test Case 3

Input

```text
[[1,4],[5,6]]
```

Output

```text
[[1,4],[5,6]]
```

---

### Test Case 4

Input

```text
[[1,10],[2,3],[4,8]]
```

Output

```text
[[1,10]]
```

---

## Brute Force

Compare every interval with every other interval.

### Approach

- Compare each interval against all remaining intervals.
- Merge whenever overlap occurs.
- Continue until no more merges are possible.

### Complexity

Time Complexity

```
O(n²)
```

Space Complexity

```
O(n)
```

---

## Optimized Approach

### Observation 1

If intervals are sorted by their starting point,

only adjacent intervals need to be compared.

---

### Observation 2

Two intervals overlap if

```
Current End >= Next Start
```

---

### Observation 3

When two intervals overlap,

keep the earliest start

and extend the ending point.

```
Current End = max(Current End, Next End)
```

---

### Algorithm

1. Sort intervals by starting point.
2. Store the first interval as Current.
3. Traverse the remaining intervals.
4. If Current overlaps with Next
   - Extend Current.
5. Otherwise
   - Save Current.
   - Make Next the new Current.
6. After traversal, save the final Current.
7. Return the merged intervals.

---

## Interview Explanation

Sorting arranges intervals in chronological order.

Once sorted, if the current interval does not overlap with the next interval,

it will never overlap with any future interval.

Therefore,

instead of comparing every interval with every other interval,

we only compare the current merged interval with the next interval.

Whenever they overlap,

we simply extend the ending boundary.

Whenever they do not,

the current interval is complete,

so we save it and begin building a new merged interval.

---

## Success Solution

```csharp
public class Solution {
    public int[][] Merge(int[][] intervals) {
        Array.Sort(intervals,(a,b)=>a[0].CompareTo(b[0]));

        List<int[]> res = new List<int[]>();

        int[] current = intervals[0];

        for(int i=1;i<intervals.Length;i++){

            int[] next = intervals[i];

            if(current[1] >= next[0]){
                current[1] = Math.Max(current[1],next[1]);
            }
            else{
                res.Add(current);
                current = next;
            }
        }

        res.Add(current);

        return res.ToArray();
    }
}
```

---

## Success Template

```
Sort intervals by start

Current = first interval

For every remaining interval

    If Current overlaps Next

        Extend Current

    Else

        Save Current

        Current = Next

Save Current

Return Answer
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
O(n)
```

(Result list)

---

## Revision Notes

- Sort intervals by starting point.
- Overlap condition:
  - Current End >= Next Start
- Extend only the ending point.
- Save Current whenever overlap stops.
- Always add the last Current after the loop.
- Sorting dominates the overall complexity.