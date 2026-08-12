# LeetCode 973 - K Closest Points to Origin

**Pattern:** Max Heap

---

## Problem

Given an array of points where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0,0)`.

The distance between two points on the X-Y plane is the Euclidean Distance.

You may return the answer in any order.

### Test Case 1

```text
Input:
points = [[1,3],[-2,2]]

k = 1

Output:
[[-2,2]]
```

### Test Case 2

```text
Input:
points = [[3,3],[5,-1],[-2,4]]

k = 2

Output:
[[3,3],[-2,4]]
```

---

## Approach

### Brute Force

- Compute the Euclidean distance of every point from the origin.
- Store every point along with its distance.
- Sort all points based on distance.
- Return the first `k` points.

Time Complexity:

**O(n log n)**

---

### Optimized Approach

Maintain a **Max Heap** of size `k`.

For every point:

- Compute its squared Euclidean distance.
- Insert the point into the Max Heap using the distance as the priority.
- If the heap size exceeds `k`, remove the point with the largest distance.

The heap always stores the `k` closest points encountered so far.

Finally, remove every remaining point from the heap and store it in the answer array.

---

## Interview Explanation

Instead of storing all points and sorting them, I maintain a Max Heap of size `k`.

Each heap element stores the point, while the priority is its squared Euclidean distance from the origin.

Whenever the heap grows beyond size `k`, I remove the point with the largest distance.

This guarantees that after processing every point, the heap contains exactly the `k` closest points.

Finally, I extract every point from the heap and return them.

---

## Success Solution

```csharp
public class Solution {
    public int[][] KClosest(int[][] points, int k) {

        PriorityQueue<int[], int> pq =
            new PriorityQueue<int[], int>(
                Comparer<int>.Create((a, b) => b.CompareTo(a))
            );

        int i = 0;

        int[][] ans = new int[k][];

        while(i < points.Length){

            int dist =
                points[i][0] * points[i][0] +
                points[i][1] * points[i][1];

            pq.Enqueue(points[i], dist);

            if(pq.Count > k){
                pq.Dequeue();
            }

            i++;
        }

        for(i = 0; i < k; i++){
            ans[i] = pq.Dequeue();
        }

        return ans;
    }
}
```

---

## Success Template

```text
Create Max Heap

Traverse every point

    Compute Squared Distance

    Insert (Point, Distance)

    If Heap Size > k

        Remove Largest Distance

Create Answer Array

Remove every remaining point

Return Answer
```

---

## Complexity

**Time Complexity:** `O(n log k)`

**Space Complexity:** `O(k)`

---

## Revision Notes

- Use a **Max Heap**
- Heap stores `(Point, Distance)`
- Priority is the squared Euclidean distance
- No need to calculate the square root
- Compare `x² + y²` instead of `√(x² + y²)`
- Heap size never exceeds `k`
- Remove the farthest point whenever heap size exceeds `k`
- Extract all remaining points using `Dequeue()`
- Answer order does **not** matter

### Why Squared Distance?

The Euclidean distance is:

```text
√(x² + y²)
```

Since the square root preserves the ordering of distances,

```text
If

a < b

Then

√a < √b
```

Therefore, comparing

```text
x² + y²
```

is sufficient and avoids the unnecessary computation of `Math.Sqrt()`.

### Why a Max Heap?

We need to keep only the `k` closest points.

Whenever the heap size exceeds `k`, the point that should be discarded is the **farthest** point.

A Max Heap allows us to remove the largest distance in `O(log k)` time.

**Mental Question:** Why do we use a Max Heap for the `k` smallest distances, whereas we used a Min Heap for the `k` largest elements?

**Answer:** The heap always removes the element we no longer want to keep. Here, we want to discard the farthest point, so the heap must expose the largest distance at its root.

---

## Pattern Learned

This problem extends the fixed-size heap pattern by changing the priority.

```text
Point

↓

Compute Distance

↓

Fixed Size Max Heap

↓

Answer
```