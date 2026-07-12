# LeetCode 905 - Sort Array By Parity

**Pattern:** Two Pointers (Read Pointer & Write Pointer)

---

## Problem

Given an integer array `nums`, move all even integers to the beginning of the array followed by all odd integers.

Return the modified array.

### Test Case 1

```text
Input:
nums = [3,1,2,4]

Output:
[2,4,3,1]
```

### Test Case 2

```text
Input:
nums = [0]

Output:
[0]
```

---

## Approach

### Brute Force

Create a new array.

Traverse once and store all even elements.

Traverse again and store all odd elements.

Time Complexity: **O(n)**

Space Complexity: **O(n)**

---

### Optimized Approach

Use two pointers.

* `j` = Read Pointer
* `i` = Write Pointer

Traverse the array using `j`.

Whenever an even element is found:

* Swap `nums[i]` and `nums[j]`
* Increment `i`

Always move `j`.

At the end, all even numbers are placed before the odd numbers.

---

## Interview Explanation

I use two pointers.

The read pointer scans every element exactly once.

Whenever an even number is found, I swap it with the element at the write pointer and move the write pointer forward.

Odd numbers are skipped.

This partitions the array into even numbers followed by odd numbers while performing the operation in-place.

---

## Success Solution

```csharp
public class Solution {
    public int[] SortArrayByParity(int[] nums) {
        int i=0,j=0;
        while(j<nums.Length){
            if(nums[j]%2==0){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
                j++;
            }
            else{
                j++;
            }
        }
        return nums;
    }
}
```

---

## Success Template

```text
Initialize:

read = 0
write = 0

Traverse using read pointer.

If current element satisfies the required condition:

    Swap(nums[write], nums[read])

    write++

Always move read.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Read Pointer → scans every element
* Write Pointer → next position for an even number
* Swap instead of copy
* Read always moves
* Write moves only when an even number is found
* Self-swap (`i == j`) is safe
* Partition the array in-place
* Mental Question: **Should this even element be moved to the front?**
