# 57. Insert Interval

## Pattern

- Intervals
- Greedy
- One Pass

---

## Problem

You are given a set of non-overlapping intervals sorted by their starting points.

Insert a new interval into the intervals such that the resulting intervals remain sorted and non-overlapping.

Merge overlapping intervals whenever necessary.

---

## Example

Input

```text
Intervals

[[1,3],[6,9]]

New Interval

[2,5]
```

Output

```text
[[1,5],[6,9]]
```

---

## Test Cases

### Test Case 1

Input

```text
Intervals

[[1,3],[6,9]]

New Interval

[2,5]
```

Output

```text
[[1,5],[6,9]]
```

---

### Test Case 2

Input

```text
Intervals

[[1,2],[3,5],[6,7],[8,10],[12,16]]

New Interval

[4,9]
```

Output

```text
[[1,2],[3,10],[12,16]]
```

---

### Test Case 3

Input

```text
Intervals

[]

New Interval

[5,7]
```

Output

```text
[[5,7]]
```

---

### Test Case 4

Input

```text
Intervals

[[1,5]]

New Interval

[2,3]
```

Output

```text
[[1,5]]
```

---

## Brute Force

### Approach

- Insert the new interval into the intervals array.
- Sort all intervals by starting point.
- Reuse the Merge Intervals algorithm.
- Return the merged intervals.

### Complexity

Time Complexity

```
O(n log n)
```

Space Complexity

```
O(n)
```

---

## Optimized Approach

### Observation 1

The intervals are already sorted.

No sorting is required.

---

### Observation 2

The intervals naturally divide into three regions.

```
Left

↓

Middle (Overlapping)

↓

Right
```

---

### Left Region

Intervals whose end is before the new interval starts.

```
Current End < New Interval Start
```

Simply copy them.

---

### Middle Region

Intervals that overlap with the new interval.

```
Current Start <= New Interval End
```

Merge them by expanding the new interval.

```
New Start = min(New Start, Current Start)

New End = max(New End, Current End)
```

---

### Right Region

Remaining intervals.

Simply copy them.

---

## Algorithm

1. Copy every interval completely before the new interval.
2. Merge all overlapping intervals into the new interval.
3. Add the merged new interval to the answer.
4. Copy all remaining intervals.
5. Return the answer.

---

## Interview Explanation

The intervals are already sorted and non-overlapping.

Instead of inserting the new interval and sorting again,

we process the array in one pass.

Intervals before the new interval are copied directly.

All overlapping intervals are merged by expanding the boundaries of the new interval.

After merging finishes,

the merged interval is added to the answer,

followed by all remaining intervals.

This avoids sorting and achieves linear time complexity.

---

## Success Solution

```csharp
public class Solution {
    public int[][] Insert(int[][] intervals, int[] newInterval) {

        List<int[]> result = new List<int[]>();

        int i = 0;

        while(i < intervals.Length &&
              intervals[i][1] < newInterval[0]){

            result.Add(intervals[i]);
            i++;
        }

        while(i < intervals.Length &&
              intervals[i][0] <= newInterval[1]){

            newInterval[0] = Math.Min(intervals[i][0], newInterval[0]);
            newInterval[1] = Math.Max(intervals[i][1], newInterval[1]);

            i++;
        }

        result.Add(newInterval);

        while(i < intervals.Length){

            result.Add(intervals[i]);
            i++;
        }

        return result.ToArray();
    }
}
```

---

## Success Template

```
Create Answer List

↓

Copy Left Region

↓

Merge Middle Region

↓

Add Merged Interval

↓

Copy Right Region

↓

Return Answer
```

---

## Complexity

Time Complexity

```
O(n)
```

Space Complexity

```
O(n)
```

(Result List)

---

## Revision Notes

- No sorting is required.
- Left Region:
  - Current End < New Interval Start
- Middle Region:
  - Current Start <= New Interval End
- Expand the new interval while merging.
- Add the merged interval only once.
- Copy remaining intervals.
- Single pass solution.