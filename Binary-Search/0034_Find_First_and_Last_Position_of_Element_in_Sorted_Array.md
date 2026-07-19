# LeetCode 34 - Find First and Last Position of Element in Sorted Array

**Pattern:** Binary Search - Boundary Search

---

## Problem

Given a sorted array of integers `nums` and a target value, return the starting and ending position of the target.

If the target is not present, return `[-1, -1]`.

### Test Case 1

```text
Input:
nums = [5,7,7,8,8,10]
target = 8

Output:
[3,4]
```

### Test Case 2

```text
Input:
nums = [5,7,7,8,8,10]
target = 6

Output:
[-1,-1]
```

---

## Approach

### Brute Force

Traverse the array from left to right to find the first occurrence.

Traverse again (or from the right) to find the last occurrence.

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Since the array is sorted, perform **two Binary Searches**.

1. First Binary Search:

   * Find the first occurrence of the target.
   * Whenever the target is found, store its index and continue searching the left half.

2. Second Binary Search:

   * Find the last occurrence of the target.
   * Whenever the target is found, store its index and continue searching the right half.

Finally, return both indices.

---

## Interview Explanation

The array is sorted, so Binary Search can be used.

Instead of stopping when I find the target, I continue searching because there may be another occurrence.

For the first occurrence, I continue searching to the left after storing the current index.

For the last occurrence, I continue searching to the right after storing the current index.

This gives the leftmost and rightmost positions of the target in `O(log n)` time.

---

## Success Solution

```csharp
public class Solution {
    public int[] SearchRange(int[] nums, int target) {

        int i=0,j=nums.Length-1,first=-1,last=-1;

        while(i<=j){
            int mid = i+(j-i)/2;

            if(nums[mid]==target){
                first = mid;
                j=mid-1;
            }
            else if(nums[mid]<target){
                i=mid+1;
            }
            else{
                j=mid-1;
            }
        }

        i=0;
        j=nums.Length-1;

        while(i<=j){
            int mid = i+(j-i)/2;

            if(nums[mid]==target){
                last = mid;
                i=mid+1;
            }
            else if(nums[mid]<target){
                i=mid+1;
            }
            else{
                j=mid-1;
            }
        }

        return new int[]{first,last};
    }
}
```

---

## Success Template

```text
Find First Occurrence

left = 0
right = n - 1
answer = -1

while(left <= right)

    mid

    if(nums[mid] == target)

        answer = mid
        right = mid - 1

    else if(nums[mid] < target)

        left = mid + 1

    else

        right = mid - 1

-------------------------------------

Find Last Occurrence

left = 0
right = n - 1
answer = -1

while(left <= right)

    mid

    if(nums[mid] == target)

        answer = mid
        left = mid + 1

    else if(nums[mid] < target)

        left = mid + 1

    else

        right = mid - 1
```

---

## Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* The array is sorted, making Binary Search applicable.
* Perform **two independent Binary Searches**.
* First search finds the leftmost occurrence.
* Second search finds the rightmost occurrence.
* Store every matching index before continuing the search.
* Search left for the first occurrence and right for the last occurrence.
* Mental Question: **Am I searching for the left boundary or the right boundary?**
