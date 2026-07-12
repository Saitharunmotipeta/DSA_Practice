# LeetCode 167 - Two Sum II (Input Array Is Sorted)

**Pattern:** Two Pointers (Opposite Direction)

---

## Problem

Given a **sorted** array of integers and a target value, return the **1-based indices** of the two numbers whose sum equals the target.

Exactly one valid solution exists.

### Test Case 1

```text id="r1o3ql"
Input:
numbers = [2,7,11,15]
target = 9

Output:
[1,2]
```

### Test Case 2

```text id="8bkm2q"
Input:
numbers = [2,3,4]
target = 6

Output:
[1,3]
```

---

## Approach

### Brute Force

Try every possible pair using nested loops.

If the pair sum equals the target, return the indices.

**Time Complexity:** `O(n²)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Since the array is sorted, use two pointers.

* `i` = Left Pointer
* `j` = Right Pointer

Calculate:

```text id="u2fz7z"
sum = numbers[i] + numbers[j]
```

* If `sum == target`, return the indices.
* If `sum < target`, move the left pointer to increase the sum.
* If `sum > target`, move the right pointer to decrease the sum.

Continue until the pointers meet.

---

## Interview Explanation

The array is already sorted, so I place one pointer at the beginning and another at the end.

If the current sum is smaller than the target, I move the left pointer because it points to a larger value in the sorted array.

If the current sum is larger than the target, I move the right pointer because it points to a smaller value.

Each iteration eliminates impossible pairs while shrinking the search space, resulting in a linear-time solution.

---

## Success Solution

```csharp id="4lkjlwm"
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        int i=0,j=numbers.Length-1;
        while(i<j){
            if(numbers[i]+numbers[j]==target){
                return [i+1,j+1];
            }
            else if(numbers[i]+numbers[j]<target){
                i++;
            }
            else{
                j--;
            }
        }
        return [-1,-1];
    }
}
```

---

## Success Template

```text id="7pq5d4"
Initialize:

left = 0
right = n - 1

While left < right:

    sum = nums[left] + nums[right]

    If sum == target:
        return answer

    If sum < target:
        left++

    Else:
        right--

Return default value.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Array is already sorted
* Use opposite-direction pointers
* `sum < target` → move left pointer
* `sum > target` → move right pointer
* Search space shrinks every iteration
* Each pointer moves only in one direction
* Mental Question: **How can I make the current sum closer to the target?**
