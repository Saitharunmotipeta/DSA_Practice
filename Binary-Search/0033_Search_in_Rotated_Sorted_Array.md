# LeetCode 33 - Search in Rotated Sorted Array

**Pattern:** Binary Search - Rotated Sorted Array

---

# Problem

There is an integer array `nums` sorted in ascending order and then rotated at an unknown pivot.

Given the array and a target value, return its index if it exists. Otherwise, return `-1`.

You must solve it in `O(log n)` time.

---

# Example

### Example 1

```text
Input:
nums = [4,5,6,7,0,1,2]
target = 0

Output:
4
```

### Example 2

```text
Input:
nums = [4,5,6,7,0,1,2]
target = 3

Output:
-1
```

---

# Brute Force Approach

Traverse the array from left to right.

If the target is found, return its index.

Otherwise, return `-1`.

```text
for each element

    if(nums[i] == target)

        return i

return -1
```

### Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

# Optimized Approach

Even though the array is rotated, one important property always remains true.

**At every iteration, one half of the current search space is always sorted.**

1. Calculate `mid`.
2. If `nums[mid]` is the target, return `mid`.
3. Determine whether the left half or the right half is sorted.
4. Check whether the target lies inside the sorted half.
5. If yes, continue searching inside that half.
6. Otherwise, discard it and search the other half.

This allows us to eliminate half of the search space every iteration.

---

# Interview Explanation

Since the array was originally sorted, rotating it only breaks the ordering at one pivot.

During Binary Search, at least one side (`left → mid` or `mid → right`) always remains sorted.

Instead of searching both halves, I first determine which half is sorted.

Then I check whether the target lies within that sorted range.

If it does, I search that half.

Otherwise, I search the opposite half.

Repeating this process reduces the search space by half every iteration, giving an `O(log n)` solution.

---

# Success Solution

```csharp
public class Solution {
    public int Search(int[] nums, int target) {

        int left = 0;
        int right = nums.Length - 1;

        while(left <= right){

            int mid = left + (right - left) / 2;

            if(nums[mid] == target){
                return mid;
            }

            else if(nums[mid] < nums[right]){

                if(target > nums[mid] && nums[right] >= target){
                    left = mid + 1;
                }
                else{
                    right = mid - 1;
                }

            }

            else{

                if(target >= nums[left] && nums[mid] > target){
                    right = mid - 1;
                }
                else{
                    left = mid + 1;
                }

            }
        }

        return -1;
    }
}
```

---

# Mental Flow

```text
Is nums[mid] the target?

        │
        ▼

Return mid

        │
        ▼

Is the right half sorted?

        │
   ┌────┴────┐
   │         │
 Yes        No
   │         │
Check      Left half
Target     is sorted
Inside?       │
   │           │
 ┌─┴─┐      ┌──┴──┐
 │   │      │     │
Yes No     Yes    No
 │   │      │      │
Go  Go     Go     Go
Right Left Left  Right
```

---

# Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

# Mistakes I Made While Learning

## 1. Assumed the array was always rotated `n/2` times

Initially, I thought I could determine the search side simply by comparing the target with the last element.

I learned that the array can be rotated any number of times, so this idea does not work in general.

---

## 2. Tried to determine the side only using the last element

I realized that after each Binary Search iteration, the search boundaries change.

The correct approach is to determine **which half of the current search space is sorted**, not where the original pivot is.

---

## 3. Used `while(left < right)`

This skips the final remaining element.

Correct:

```text
while(left <= right)
```

---

## 4. Forgot to move a pointer in every possible branch

An earlier version could leave both `left` and `right` unchanged, causing an infinite loop.

Every Binary Search iteration must eliminate half of the remaining search space.

---

## 5. Incorrect boundary conditions

Earlier I wrote conditions similar to:

```text
target < nums[right]
```

and

```text
target > nums[left]
```

This incorrectly excluded the boundary elements.

Correct conditions are:

```text
nums[mid] < target && target <= nums[right]
```

and

```text
nums[left] <= target && target < nums[mid]
```

Reason:

* `mid` has already been checked.
* `left` and `right` have not.

---

# Revision Notes

* Rotated array does **not** mean two sorted arrays.
* Every iteration has **one guaranteed sorted half**.
* First identify the sorted half.
* Then determine whether the target belongs to that sorted half.
* Search inside it if it does.
* Otherwise search the opposite half.
* Always use `while(left <= right)`.
* Every iteration must move either `left` or `right`.
* Carefully handle boundary conditions using `<` and `<=`.
