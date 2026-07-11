# LeetCode 27 - Remove Element

**Pattern:** Two Pointers (Read Pointer & Write Pointer)

---

## Problem

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in-place and return the number of remaining elements.

The order of elements beyond the returned length does not matter.

### Test Case 1

```text
Input:
nums = [3,2,2,3]
val = 3

Output:
2

nums = [2,2,_,_]
```

### Test Case 2

```text
Input:
nums = [0,1,2,2,3,0,4,2]
val = 2

Output:
5

nums = [0,1,3,0,4,_,_,_]
```

---

## Approach

### Brute Force

Traverse the array and mark every occurrence of `val`.

Traverse again and shift all remaining elements to the left.

Time Complexity: **O(2n) = O(n)**

---

### Optimized Approach

Use two pointers.

* `j` = Read Pointer (reads every element)
* `i` = Write Pointer (stores only valid elements)

If the current element is not equal to `val`:

* Copy it to index `i`
* Increment `i`

Otherwise ignore it.

Return `i` because it represents the number of valid elements.

---

## Interview Explanation

I use two pointers.

The read pointer visits every element exactly once.

Whenever a valid element is found, I copy it to the write pointer and move the write pointer forward.

Invalid elements are skipped.

The write pointer always represents the next position where a valid element should be placed.

This performs the removal in-place without using extra space.

---

## Success Solution

```csharp
public class Solution {
    public int RemoveElement(int[] nums, int val) {
        int i=0,j=0;
        while(j<nums.Length){
            if(nums[j]!=val){
                nums[i]=nums[j];
                i++;
            }
            j++;
        }
        return i;
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

If current element is valid:

    nums[write] = nums[read]
    write++

Always move read.

Return write.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Read Pointer → reads every element
* Write Pointer → stores only valid elements
* Read always moves
* Write moves only for valid elements
* Ignore invalid elements
* Array is modified in-place
* Return write pointer
* Mental Question: **Should this element be copied to the front?**
