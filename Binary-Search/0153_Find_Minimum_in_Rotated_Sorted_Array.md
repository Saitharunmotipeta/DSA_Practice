# LeetCode 153 - Find Minimum in Rotated Sorted Array

**Pattern:** Binary Search - Rotated Sorted Array

---

# Problem

Suppose an array of distinct integers sorted in ascending order is rotated between `1` and `n` times.

Return the minimum element in the array.

You must solve it in `O(log n)` time.

---

# Example

### Example 1

```text
Input:
nums = [3,4,5,1,2]

Output:
1
```

### Example 2

```text
Input:
nums = [4,5,6,7,0,1,2]

Output:
0
```

### Example 3

```text
Input:
nums = [11,13,15,17]

Output:
11
```

---

# Brute Force Approach

Traverse the array while maintaining the smallest value seen so far.

```text
minimum = nums[0]

for each element

    if(nums[i] < minimum)

        minimum = nums[i]

return minimum
```

### Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

# Optimized Approach

Instead of searching for a target, we search for the position of the minimum element.

The important observation is:

**At every iteration, compare `nums[mid]` with `nums[right]`.**

* If `nums[mid] > nums[right]`, the minimum must be in the right half, so discard the left half.
* Otherwise, the right half is sorted, and the minimum is either `mid` itself or somewhere to its left. Since `mid` could be the minimum, keep it in the search space.

Continue shrinking the search space until `left == right`.

That index is the minimum element.

---

# Interview Explanation

The array was originally sorted and then rotated once around a pivot.

Instead of finding the pivot directly, I compare the middle element with the rightmost element.

If the middle element is greater than the rightmost element, the rotation point must lie to the right.

Otherwise, the right half is sorted, so the minimum cannot be after `mid`. However, `mid` itself might be the minimum, so I keep it by moving `right = mid`.

The search space keeps shrinking until only one element remains.

That element is the minimum.

---

# Success Solution

```csharp
public class Solution {
    public int FindMin(int[] nums) {

        int left = 0;
        int right = nums.Length - 1;

        while(left < right){

            int mid = left + (right - left) / 2;

            if(nums[mid] > nums[right]){
                left = mid + 1;
            }
            else{
                right = mid;
            }
        }

        return nums[left];
    }
}
```

---

# Mental Flow

```text
Compare nums[mid] and nums[right]

        │
        ▼

nums[mid] > nums[right] ?

        │
   ┌────┴────┐
   │         │
 Yes        No
   │         │
Minimum     Right half is sorted
must be     Minimum is at mid
to right    or to its left
   │         │
left=mid+1 right=mid
```

Repeat until:

```text
left == right
```

Return:

```text
nums[left]
```

---

# Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

# Mistakes I Made While Learning

## 1. Initially thought I needed to find the rotation count

I realized that the problem never asks for the number of rotations.

It only asks for the minimum element.

Finding the minimum automatically identifies the rotation point.

---

## 2. Thought of it like searching for a target

Unlike Binary Search problems such as LeetCode 704 or 33, there is no target value.

Instead, I repeatedly eliminate the half that cannot contain the minimum.

---

## 3. Wanted to use `right = mid - 1`

This was the biggest mistake.

If:

```text
nums[mid] < nums[right]
```

then `mid` itself may be the minimum.

Example:

```text
6 7 0 1 2 3 4
    ↑
   mid
```

Discarding `mid` would remove the correct answer.

Therefore:

```text
right = mid
```

is required.

---

## 4. Learned when `mid` can be discarded

In previous Binary Search problems, `mid` was discarded because it had already been proven not to be the answer.

Here, `mid` is never proven to be not the minimum.

Therefore, it must remain in the search space whenever it is still a valid candidate.

---

# Revision Notes

* This problem does **not** search for a target.
* Compare `nums[mid]` with `nums[right]`.
* If `nums[mid] > nums[right]`, search the right half.
* Otherwise, keep `mid` because it could be the minimum.
* Use `right = mid`, not `right = mid - 1`.
* Stop when `left == right`.
* Return `nums[left]`.
* Mental Question: **"Can I safely discard `mid`, or could it still be the minimum?"**
