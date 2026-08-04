# 452. Minimum Number of Arrows to Burst Balloons

## Pattern

- Intervals
- Greedy
- Interval Intersection

---

## Problem

There are multiple balloons represented as intervals.

```
[start, end]
```

An arrow shot at position **x** bursts every balloon satisfying

```
start <= x <= end
```

Return the minimum number of arrows required to burst all balloons.

---

## Example

Input

```text
[[10,16],[2,8],[1,6],[7,12]]
```

Output

```text
2
```

Explanation

Arrow 1 bursts

```text
[1,6]
[2,8]
```

Arrow 2 bursts

```text
[7,12]
[10,16]
```

---

## Test Cases

### Test Case 1

Input

```text
[[10,16],[2,8],[1,6],[7,12]]
```

Output

```text
2
```

---

### Test Case 2

Input

```text
[[1,2],[3,4],[5,6],[7,8]]
```

Output

```text
4
```

---

### Test Case 3

Input

```text
[[1,2],[2,3],[3,4],[4,5]]
```

Output

```text
2
```

---

### Test Case 4

Input

```text
[[1,6],[2,8],[4,7]]
```

Output

```text
1
```

---

## Brute Force

### Approach

- Try placing arrows at different positions.
- Check how many balloons each arrow bursts.
- Continue until all balloons are burst.

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

Sort balloons by their starting point.

---

### Observation 2

If two balloons overlap,

one arrow can burst both.

---

### Observation 3

Instead of merging using the union,

maintain the **common intersection** where an arrow can still burst every balloon in the current group.

```
Start = max(Current Start, Next Start)

End = min(Current End, Next End)
```

---

### Observation 4

If there is no overlap,

the current arrow cannot burst the next balloon.

A new arrow is required.

---

## Algorithm

1. Sort balloons by starting point.
2. Initialize arrows as 1.
3. Store the first balloon as the current shooting region.
4. Traverse remaining balloons.
5. If they overlap
   - Shrink the shooting region using intersection.
6. Otherwise
   - Increment arrows.
   - Start a new shooting region.
7. Return arrows.

---

## Interview Explanation

The goal is to maximize the number of balloons burst by a single arrow.

Whenever balloons overlap,

there exists a common region where an arrow can burst all of them.

Instead of expanding the interval,

we continuously shrink the valid shooting region by taking the intersection.

Once there is no overlap,

the current arrow cannot burst the next balloon,

so a new arrow is required.

---

## Success Solution

```csharp
public class Solution {
    public int FindMinArrowShots(int[][] points) {

        Array.Sort(points, (a, b) => a[0].CompareTo(b[0]));

        int arrows = 1;
        int i = 1;

        int[] current = points[0];

        while(i < points.Length){

            int[] next = points[i];

            if(next[0] <= current[1]){

                current[0] = Math.Max(current[0], next[0]);
                current[1] = Math.Min(current[1], next[1]);
            }
            else{

                arrows++;
                current = next;
            }

            i++;
        }

        return arrows;
    }
}
```

---

## Success Template

```
Sort by Start

↓

Arrows = 1

↓

Current = First Balloon

↓

For every remaining balloon

    Overlap?

        Yes

            Take Intersection

        No

            Arrows++

            Current = Next Balloon

↓

Return Arrows
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

- Sort balloons by starting point.
- Overlap condition:
  - Next Start <= Current End
- Overlapping balloons share one arrow.
- Maintain the intersection of overlapping balloons.
- Intersection:
  - Start = max(Current Start, Next Start)
  - End = min(Current End, Next End)
- If no overlap:
  - New arrow required.
- Difference from Merge Intervals:
  - Merge uses **Union**.
  - Balloons use **Intersection**.