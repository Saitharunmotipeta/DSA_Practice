# LeetCode 11 - Container With Most Water

**Pattern:** Two Pointers (Opposite Direction)

---

## Problem

Given an array `height`, where each element represents the height of a vertical line, find the maximum amount of water a container can store.

### Test Case 1

```text
Input:
height = [1,8,6,2,5,4,8,3,7]

Output:
49
```

### Test Case 2

```text
Input:
height = [1,1]

Output:
1
```

---

## Approach

### Brute Force

Try every possible pair of lines.

For each pair:

* Width = `j - i`
* Height = `min(height[i], height[j])`
* Area = Width × Height

Keep updating the maximum area.

**Time Complexity:** `O(n²)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Use two pointers.

* `i` = Left Pointer
* `j` = Right Pointer

At every iteration:

* Calculate the current area.
* Update the maximum area.
* Move the pointer having the smaller height.

Reason:

The width always decreases.

The only possibility of obtaining a larger area is by finding a taller minimum height.

Moving the taller pointer can never increase the area while the shorter height remains the limiting factor.

---

## Interview Explanation

I place one pointer at the beginning and another at the end of the array.

The current container area depends on the smaller of the two heights and the distance between them.

After calculating the area, I move the pointer pointing to the shorter line because the shorter height limits the current container.

Although the width decreases, moving the shorter pointer gives a chance to find a taller line and potentially increase the overall area.

---

## Success Solution

```csharp
public class Solution {
    public int MaxArea(int[] height) {
        int i=0,j=height.Length-1,maxarea=0;
        while(i<j){
            int area = Math.Min(height[i],height[j])*(j-i);
            if(area>maxarea){
                maxarea=area;
            }
            if(height[i]>height[j]){
                j--;
            }
            else{
                i++;
            }
        }
        return maxarea;
    }
}
```

---

## Success Template

```text
Initialize:

left = 0
right = n - 1
maxArea = 0

While left < right

    area = min(leftHeight, rightHeight) × width

    Update maximum area

    Move the pointer having the smaller height

Return maximum area
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Opposite-direction pointers
* Width = `right - left`
* Height = `min(leftHeight, rightHeight)`
* Area = Width × Height
* Update maximum every iteration
* Move the pointer with the smaller height
* Width always decreases
* Hope lies in finding a taller limiting height
* Mental Question: **Which pointer limits the current area?**
