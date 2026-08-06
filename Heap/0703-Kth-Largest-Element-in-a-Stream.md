# LeetCode 703 - Kth Largest Element in a Stream

**Pattern:** Min Heap

---

## Problem

Design a class to find the **kth largest element** in a stream.

Implement the `KthLargest` class:

- `KthLargest(int k, int[] nums)` initializes the object with the integer `k` and the stream of integers `nums`.
- `int Add(int val)` appends the integer `val` to the stream and returns the element representing the **kth largest** element in the stream.

### Test Case 1

```text
Input:

["KthLargest","add","add","add","add","add"]

[[3,[4,5,8,2]],[3],[5],[10],[9],[4]]

Output:

[null,4,5,5,8,8]
```

---

## Approach

### Brute Force

For every `Add()` operation:

- Insert the new element into a collection.
- Sort the complete collection.
- Return the kth largest element.

Time Complexity per Add:

**O(n log n)**

---

### Optimized Approach

Maintain a **Min Heap** of size `k`.

During construction:

- Insert every element from the initial array.
- If the heap size exceeds `k`, remove the smallest element.

During every `Add()` operation:

- Insert the new element into the heap.
- If the heap size exceeds `k`, remove the smallest element.
- Return the root of the Min Heap.

The heap always stores the **k largest elements** seen so far.

Since it is a **Min Heap**, the root represents the **kth largest** element.

---

## Interview Explanation

I maintain a Min Heap whose size never exceeds `k`.

Whenever a new value arrives, I insert it into the heap.

If the heap grows larger than `k`, I remove the smallest element because it can never become part of the `k` largest elements.

This guarantees that after every insertion, the heap contains exactly the `k` largest elements seen so far.

Since the root of a Min Heap is the smallest element inside the heap, it represents the kth largest element in the stream.

To avoid duplicating logic, the constructor simply calls the `Add()` method for every element in the initial array.

---

## Success Solution

```csharp
public class KthLargest {

    PriorityQueue<int,int> pq = new PriorityQueue<int,int>();
    int k;

    public KthLargest(int k, int[] nums) {
        int i = 0;
        this.k = k;

        while(i < nums.Length){
            Add(nums[i]);
            i++;
        }
    }

    public int Add(int val) {
        pq.Enqueue(val, val);

        if(pq.Count > k){
            pq.Dequeue();
        }

        return pq.Peek();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.Add(val);
 */
```

---

## Success Template

```text
Create Min Heap

Store k

Constructor

    Traverse initial array

        Call Add()

Add()

    Insert current element

    If Heap Size > k

        Remove Smallest Element

    Return Heap Root
```

---

## Complexity

**Constructor**

Time Complexity:

```
O(n log k)
```

---

**Add()**

Time Complexity:

```
O(log k)
```

---

**Space Complexity**

```
O(k)
```

---

## Revision Notes

- Min Heap of size `k`
- Heap stores only the `k` largest elements
- Constructor initializes the heap
- Constructor reuses the `Add()` method
- Avoid duplicate heap logic
- Insert every new value into the heap
- Remove the smallest when heap size exceeds `k`
- Root of the heap is always the kth largest element
- Heap invariant: **Heap always contains exactly the k largest elements seen so far**
- Mental Question: **Why can the constructor simply call `Add()` instead of implementing separate heap logic?**