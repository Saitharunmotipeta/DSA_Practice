# LeetCode 35 - Search Insert Position

**Pattern:** Binary Search

---

## Problem

Given a sorted array of distinct integers and a target value, return the index if the target is found.

If not, return the index where it would be inserted while maintaining the sorted order.

### Test Case 1

```text
Input:
nums = [1,3,5,6]
target = 5

Output:
2
```

### Test Case 2

```text
Input:
nums = [1,3,5,6]
target = 2

Output:
1
```

---

## Approach

### Brute Force

Traverse the array.

Return the first index where the current element is greater than or equal to the target.

If no such element exists, return the array length.

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Since the array is sorted, use Binary Search.

Maintain two boundaries:

* `left`
* `right`

If the middle element is greater than or equal to the target, continue searching on the left.

Otherwise, search on the right.

When the search space becomes empty, `left` represents the correct insertion position.

---

## Interview Explanation

I use Binary Search because the array is sorted.

Instead of checking every element, I repeatedly eliminate half of the search space.

If the middle element is greater than or equal to the target, the insertion position must be at the middle or to its left.

Otherwise, it must be to the right.

When the loop finishes, the left pointer indicates the first valid insertion index.

---

## Success Solution

```csharp
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        int left = 0, right = nums.Length - 1;

        while(left <= right){
            int mid = left + (right - left) / 2;

            if(nums[mid] >= target){
                right = mid - 1;
            }
            else{
                left = mid + 1;
            }
        }

        return left;
    }
}
```

---

## Success Template

```text
Initialize:

left = 0
right = n - 1

While left <= right

    mid = left + (right - left) / 2

    If nums[mid] >= target

        right = mid - 1

    Else

        left = mid + 1

Return left
```

---

## Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Array must be sorted
* Binary Search on boundaries
* Search for the first position where the value is greater than or equal to the target
* Return `left` after the loop
* Use `mid - 1` and `mid + 1` to eliminate half the search space
* Mental Question: **Which half cannot contain the insertion position?**
