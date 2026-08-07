# LeetCode 1288 - Remove Covered Intervals

**Pattern:** Intervals (Sorting + Greedy)

---

## Problem

Given an array of intervals where `intervals[i] = [li, ri]`, remove all intervals that are covered by another interval.

An interval `[a, b]` is covered by `[c, d]` if:

```text
c <= a

AND

b <= d
```

Return the number of remaining intervals.

### Test Case 1

```text
Input:
intervals = [[1,4],[3,6],[2,8]]

Output:
2
```

Explanation:

```text
[3,6]

is covered by

[2,8]
```

---

### Test Case 2

```text
Input:
intervals = [[1,4],[2,3]]

Output:
1
```

Explanation:

```text
[2,3]

is covered by

[1,4]
```

---

## Approach

### Brute Force

Compare every interval with every other interval.

For every interval:

- Check whether another interval completely covers it.
- If covered, remove it.

Time Complexity:

**O(n²)**

---

### Optimized Approach

Sort the intervals:

- Start in **Ascending Order**
- If starts are equal, End in **Descending Order**

This guarantees that larger intervals with the same start appear first.

Traverse the sorted intervals.

Maintain the current interval.

For every next interval:

- If its end is less than or equal to the current end, it is completely covered.
- Otherwise, update the current interval.

Finally,

```text
Remaining Intervals

=

Total Intervals

-

Covered Intervals
```

---

## Interview Explanation

I first sort the intervals by their start values.

If two intervals have the same start, I place the interval with the larger end first.

This prevents smaller intervals from becoming the current interval before the larger covering interval.

While traversing the array, I only compare the ending values.

Since the intervals are already sorted by their start values, the start condition for coverage is automatically satisfied.

If the next interval ends before or at the current interval's end, it is covered.

Otherwise, I update the current interval.

Finally, I subtract the number of covered intervals from the total number of intervals.

---

## Success Solution

```csharp
public class Solution {
    public int RemoveCoveredIntervals(int[][] intervals) {

        Array.Sort(intervals, (a, b) =>
        {
            if (a[0] == b[0])
                return b[1].CompareTo(a[1]);

            return a[0].CompareTo(b[0]);
        });

        int covered = 0;
        int i = 1;

        int[] current = intervals[0];

        while(i < intervals.Length){

            int[] next = intervals[i];

            if(current[1] >= next[1]){
                covered++;
            }
            else{
                current = next;
            }

            i++;
        }

        return intervals.Length - covered;
    }
}
```

---

## Success Template

```text
Sort Intervals

Start Ascending

If Same Start

End Descending

covered = 0

current = first interval

Traverse remaining intervals

    If nextEnd <= currentEnd

        covered++

    Else

        current = next

Return

Total Intervals - Covered
```

---

## Complexity

**Time Complexity:** `O(n log n)`

**Space Complexity:** `O(1)` *(Ignoring sorting space used internally.)*

---

## Revision Notes

- Sort by **Start Ascending**
- If starts are equal, sort by **End Descending**
- Only one traversal after sorting
- Keep track of the current interval
- Compare only the ending values
- Covered condition:

```text
nextEnd <= currentEnd
```

- No extra data structures required
- Answer = Total Intervals − Covered Intervals

### Why End Descending for Equal Starts?

Consider:

```text
[1,4]

[1,5]
```

If sorted normally:

```text
[1,4]

[1,5]
```

The algorithm would incorrectly update the current interval before detecting coverage.

Instead, sorting as:

```text
[1,5]

[1,4]
```

ensures the larger interval appears first, allowing the smaller one to be correctly identified as covered.

### Why Don't We Compare Starts During Traversal?

Coverage is defined as:

```text
currentStart <= nextStart

AND

nextEnd <= currentEnd
```

After sorting by **Start Ascending**, we already know:

```text
currentStart <= nextStart
```

Therefore, during traversal, only the second condition needs to be checked:

```text
nextEnd <= currentEnd
```

This simplifies the algorithm to a single comparison.

**Mental Question:** Why is comparing only the end values sufficient after sorting?

**Answer:** Because sorting guarantees that every future interval starts at or after the current interval, satisfying the first condition of coverage automatically.

---

## Pattern Learned

```text
Sort Intervals

↓

Start Ascending

If Same Start

End Descending

↓

Single Linear Scan

↓

Count Covered Intervals

↓

Return Remaining Intervals
```