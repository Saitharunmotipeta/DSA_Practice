# LeetCode 283 - Move Zeroes

**Pattern:** Two Pointers (Read Pointer & Write Pointer)

---

## Problem

Move all `0`s to the end of the array while maintaining the relative order of the non-zero elements.

The operation must be performed **in-place**.

### Test Case 1

```text
Input:
nums = [0,1,0,3,12]

Output:
[1,3,12,0,0]
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

Traverse the array and identify all zero elements.

Shift every non-zero element to the left.

Finally, fill the remaining positions with zeros.

**Time Complexity:** `O(n²)` (multiple shifts)

---

### Optimized Approach

Use two pointers.

* `j` = Read Pointer
* `i` = Write Pointer (next position where a non-zero element should be placed)

Traverse the array using `j`.

Whenever a non-zero element is found:

* Swap `nums[i]` and `nums[j]`
* Increment `i`

Always move `j`.

Since every non-zero element is swapped to the earliest available position, all zeros automatically move to the end.

---

## Interview Explanation

I use two pointers.

The read pointer scans every element exactly once.

Whenever a non-zero element is encountered, I swap it with the element at the write pointer and move the write pointer forward.

Zeros are skipped during traversal, so they naturally accumulate at the end of the array.

The algorithm performs the operation in-place without using additional space.

---

## Success Solution

```csharp
public class Solution {
    public void MoveZeroes(int[] nums) {
        int i=0,j=0;
        while(j<nums.Length){
            if(nums[j]!=0){
                int temp = nums[i];
                nums[i]=nums[j];
                nums[j] = temp;
                i++;
                j++;
            }
            else{
                j++;
            }
        }
        return ;
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

If current element is non-zero:

    Swap(nums[write], nums[read])

    write++

Always move read.

Zeros automatically move behind the write pointer.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Read Pointer → visits every element
* Write Pointer → next position for a non-zero element
* Swap instead of copy
* Swapping moves zero backward automatically
* Read pointer always moves
* Write pointer moves only when a non-zero element is found
* Self-swap (`i == j`) is completely safe
* Mental Question: **Should this non-zero element be moved to the current write position?**
