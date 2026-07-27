# LeetCode 503 - Next Greater Element II

**Pattern:** Monotonic Decreasing Stack

---

## Problem

Given a circular integer array `nums`, return the **next greater element** for every element.

The next greater element of a number is the first greater number traversing **circularly**.

If no greater element exists, return `-1`.

### Test Case 1

```text
Input:
nums = [1,2,1]

Output:
[2,-1,2]
```

### Test Case 2

```text
Input:
nums = [1,2,3,4,3]

Output:
[2,3,4,-1,4]
```

---

## Approach

### Brute Force

For every element:

* Search from the next index until the end of the array.
* If a greater element is found, store it and stop.
* Otherwise continue searching from index `0` up to the current index.
* If no greater element is found, store `-1`.

Time Complexity:

**O(2n²) = O(n²)**

---

### Optimized Approach

Use a **Monotonic Decreasing Stack** that stores **indices**.

Since the array is circular:

* Traverse the array **twice**.
* Use

```
index = i % n
```

to simulate the circular traversal.

Whenever the current value is greater than the value at the index on the top of the stack:

* Pop the previous index.
* Store the current value as its next greater element.

Only push indices during the **first traversal**.

Initialize every answer with `-1` because some elements may never find a greater element.

---

## Interview Explanation

I use a Monotonic Decreasing Stack that stores indices of elements whose next greater element has not been found yet.

Since the array is circular, I iterate from `0` to `2 * n - 1` and use modulo (`i % n`) to simulate revisiting the array.

Whenever the current element is greater than the element represented by the index on the top of the stack, I pop that index and store the current value as its next greater element.

I push indices only during the first traversal so that every index enters the stack exactly once.

---

## Success Solution

```csharp
public class Solution {
    public int[] NextGreaterElements(int[] nums) {

        Stack<int> stack = new Stack<int>();
        int n = nums.Length;
        int[] answer = new int[n];

        Array.Fill(answer, -1);

        int i = 0;

        while(i < 2 * n){

            int value = i % n;

            while(stack.Count > 0 &&
                  nums[value] > nums[stack.Peek()]){

                answer[stack.Pop()] = nums[value];
            }

            if(i < n){
                stack.Push(value);
            }

            i++;
        }

        return answer;
    }
}
```

---

## Success Template

```text
Initialize:

Create Stack (stores indices)

Create Answer Array

Fill Answer with -1

Traverse 2 * n times

    index = i % n

    While stack is not empty

    AND

    current value >
    value at stack top

        answer[poppedIndex] =
        current value

    Push index only during
    first traversal

Return Answer
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

## Revision Notes

* Array is circular
* Traverse `2 * n` times
* Use `index = i % n`
* Stack stores **indices**
* Compare using `nums[index]`
* Store current value as next greater element
* Push indices only during the first traversal
* Initialize answer with `-1`
* Every index is pushed once
* Every index is popped once
* Mental Question: **Can the current element become the next greater element for previous unresolved indices?**