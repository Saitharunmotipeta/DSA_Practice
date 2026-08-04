# 986. Interval List Intersections

## Pattern

- Intervals
- Two Pointers
- Interval Intersection

---

## Problem

Given two lists of closed intervals,

return the intersection of these two interval lists.

Each list is already sorted by starting time.

---

## Example

Input

```text
firstList  = [[0,2],[5,10],[13,23],[24,25]]

secondList = [[1,5],[8,12],[15,24],[25,26]]
```

Output

```text
[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
```

---

## Test Cases

### Test Case 1

Input

```text
firstList = [[0,2],[5,10],[13,23],[24,25]]

secondList = [[1,5],[8,12],[15,24],[25,26]]
```

Output

```text
[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
```

---

### Test Case 2

Input

```text
firstList = [[1,3],[5,9]]

secondList = []
```

Output

```text
[]
```

---

### Test Case 3

Input

```text
firstList = [[1,7]]

secondList = [[3,10]]
```

Output

```text
[[3,7]]
```

---

## Brute Force

### Approach

- Compare every interval from the first list with every interval from the second list.
- If they overlap, add the intersection.

### Complexity

Time Complexity

```
O(n × m)
```

Space Complexity

```
O(1)
```

---

## Optimized Approach

### Observation 1

Both interval lists are already sorted.

No sorting is required.

---

### Observation 2

Use two pointers.

```
i → firstList

j → secondList
```

---

### Observation 3

Two intervals overlap if

```
firstList[i][0] <= secondList[j][1]

AND

secondList[j][0] <= firstList[i][1]
```

---

### Observation 4

The intersection is

```
Start = max(start1, start2)

End = min(end1, end2)
```

---

### Observation 5

After comparing two intervals,

move the pointer of the interval that finishes first.

```
end1 < end2

↓

i++

Otherwise

↓

j++
```

---

## Algorithm

1. Initialize two pointers.
2. Compare the current intervals.
3. If they overlap
   - Compute the intersection.
   - Add it to the answer.
4. Move the pointer whose interval ends first.
5. Continue until one list is exhausted.
6. Return the result.

---

## Interview Explanation

Since both interval lists are already sorted,

we never need to compare an interval again once it finishes.

After comparing two intervals,

the interval with the smaller ending point cannot intersect any future interval in the other list.

Therefore,

we move the pointer of the interval that ends first.

Whenever intervals overlap,

their intersection is

```
[
max(start1, start2),
min(end1, end2)
]
```

which is added to the answer.

---

## Success Solution

```csharp
public class Solution {
    public int[][] IntervalIntersection(int[][] firstList, int[][] secondList) {

        List<int[]> result = new List<int[]>();

        int i = 0;
        int j = 0;

        while(i < firstList.Length && j < secondList.Length){

            if(firstList[i][0] <= secondList[j][1] &&
               secondList[j][0] <= firstList[i][1]){

                result.Add(new int[]
                {
                    Math.Max(firstList[i][0], secondList[j][0]),
                    Math.Min(firstList[i][1], secondList[j][1])
                });
            }

            if(firstList[i][1] < secondList[j][1]){
                i++;
            }
            else{
                j++;
            }
        }

        return result.ToArray();
    }
}
```

---

## Success Template

```
i = 0

j = 0

↓

While both pointers are valid

    Check overlap

        Yes

            Add

            [
            max(start1,start2),
            min(end1,end2)
            ]

    Compare end times

        First ends first

            i++

        Otherwise

            j++

↓

Return Answer
```

---

## Complexity

Traversal

```
O(n + m)
```

Space Complexity

```
O(k)

k = Number of intersections
```

---

## Revision Notes

- Two sorted interval lists.
- No sorting required.
- Use two pointers.
- Overlap condition:
  - start1 <= end2
  - start2 <= end1
- Intersection:
  - max(start1, start2)
  - min(end1, end2)
- After every comparison:
  - Move the pointer whose interval ends first.
- Every comparison advances one pointer.
- Time Complexity:
  - O(n + m)