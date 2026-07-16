# LeetCode 704 - Binary Search

**Pattern:** Binary Search

---

## Problem

Given a sorted array of integers `nums` and an integer `target`, return the index of `target` if it exists in the array. Otherwise, return `-1`.

### Test Case 1

```text
Input:
nums = [-1,0,3,5,9,12]
target = 9

Output:
4
```

### Test Case 2

```text
Input:
nums = [-1,0,3,5,9,12]
target = 2

Output:
-1
```

---

## Approach

### Brute Force

Traverse the array from left to right.

If the current element equals the target, return its index.

If the target is not found after traversing the entire array, return `-1`.

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Since the array is sorted, use Binary Search.

Maintain two pointers:

* `i` = Left Boundary
* `j` = Right Boundary

While the search space is valid:

* Calculate the middle index.
* If the middle element equals the target, return its index.
* If the middle element is greater than the target, search the left half.
* Otherwise, search the right half.

Repeat until the target is found or the search space becomes empty.

---

## Interview Explanation

The array is already sorted, so I can eliminate half of the search space after every comparison.

I calculate the middle index using:

```text
mid = left + (right - left) / 2
```

If the middle element is the target, I return its index.

If the target is smaller, I continue searching in the left half.

If the target is larger, I continue searching in the right half.

The process continues until the target is found or the search space is exhausted.

---

## Success Solution

```csharp
public class Solution {
    public int Search(int[] nums, int target) {
        int i=0,j=nums.Length-1,mid=nums.Length/2;
        while(i<=j){
            mid = i+(j-i)/2;
            if(nums[mid]==target){
                return mid;
            }
            else if(nums[mid]>target){
                j=mid-1;
            }
            else{
                i=mid+1;
            }
        }
        return -1;
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

    If nums[mid] == target

        return mid

    Else if nums[mid] > target

        right = mid - 1

    Else

        left = mid + 1

Return -1
```

---

## Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Array must be sorted
* Maintain left and right boundaries
* Search space is `[left, right]`
* Use `while(left <= right)`
* Calculate middle using `left + (right - left) / 2`
* If target is smaller → search left half
* If target is larger → search right half
* Each iteration removes half of the remaining search space
* Mental Question: **Which half can I safely discard?**
