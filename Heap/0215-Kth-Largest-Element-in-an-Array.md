# LeetCode 215 - Kth Largest Element in an Array

**Pattern:** Min Heap

---

## Problem

Given an integer array `nums` and an integer `k`, return the **kth largest element** in the array.

Note:

- It is the kth largest element in the sorted order.
- It is **not** the kth distinct element.

### Test Case 1

```text
Input:
nums = [3,2,1,5,6,4]

k = 2

Output:
5
```

### Test Case 2

```text
Input:
nums = [3,2,3,1,2,4,5,5,6]

k = 4

Output:
4
```

---

## Approach

### Brute Force

- Sort the array in ascending order.
- Return the element at index `nums.Length - k`.

Time Complexity:

**O(n log n)**

---

### Optimized Approach

Use a **Min Heap** of size `k`.

The heap always stores the **k largest elements** encountered so far.

For every number:

- Insert the current element into the heap.
- If the heap size becomes greater than `k`, remove the smallest element.

Removing the smallest ensures that only the `k` largest candidates remain inside the heap.

After processing every element, the root of the Min Heap represents the **kth largest element**.

---

## Interview Explanation

Instead of sorting the complete array, I maintain a **Min Heap** containing only the `k` largest elements seen so far.

Every incoming element is inserted into the heap.

If the heap size exceeds `k`, I remove the smallest element because it can never become the kth largest element.

At the end of the traversal, the heap contains exactly the `k` largest elements, and since it is a Min Heap, its root is the kth largest element.

---

## Success Solution

```csharp
public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        int i = 0;

        PriorityQueue<int, int> pq = new PriorityQueue<int, int>();

        while (i < nums.Length) {
            pq.Enqueue(nums[i], nums[i]);

            if (pq.Count > k) {
                pq.Dequeue();
            }

            i++;
        }

        return pq.Peek();
    }
}
```

---

## Success Template

```text
Create Min Heap

Traverse every element

    Insert current element

    If Heap Size > k

        Remove Smallest Element

Return Heap Root
```

---

## Complexity

**Time Complexity:** `O(n log k)`

**Space Complexity:** `O(k)`

---

## Revision Notes

- Min Heap of size `k`
- Heap stores only the `k` largest elements
- Insert every element into the heap
- Remove the smallest when heap size exceeds `k`
- Root of the Min Heap is the answer
- No need to sort the complete array
- More efficient than sorting when `k << n`
- Mental Question: **Why can the smallest element be safely removed whenever the heap size becomes greater than `k`?**