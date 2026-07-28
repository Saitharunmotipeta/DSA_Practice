# LeetCode 84 - Largest Rectangle in Histogram

**Pattern:** Monotonic Increasing Stack

---

## Problem

Given an array `heights` representing the height of histogram bars where each bar has a width of `1`, return the area of the largest rectangle that can be formed.

### Test Case 1

```text
Input:
heights = [2,1,5,6,2,3]

Output:
10
```

### Test Case 2

```text
Input:
heights = [2,4]

Output:
4
```

---

## Approach

### Brute Force

For every bar:

* Consider the current bar as the rectangle height.
* Expand towards the left until a smaller bar is found.
* Expand towards the right until a smaller bar is found.
* Calculate

```
Area = Height × Width
```

* Maintain the maximum area.

Time Complexity:

**O(n²)**

---

### Optimized Approach

Use a **Monotonic Increasing Stack** that stores **indices**.

The stack always keeps the bar indices in increasing order of heights.

Whenever the current bar is shorter than the bar at the top of the stack:

* The taller bar can no longer extend further to the right.
* Pop the taller bar.
* The popped bar becomes the rectangle height.
* The current index becomes the first smaller element on the right.
* The new stack top becomes the first smaller element on the left.
* Compute the width and area immediately.

A dummy bar of height `0` is processed at the end to force all remaining bars to be popped.

---

## Interview Explanation

I maintain a Monotonic Increasing Stack containing indices of histogram bars.

Whenever a smaller bar is encountered, every taller bar on the stack has found its right boundary.

After popping a bar:

* The popped height becomes the rectangle height.
* The current index represents the first smaller element on the right.
* The new top of the stack represents the first smaller element on the left.
* Using these two boundaries, I calculate the maximum width possible for that height and update the maximum area.

To ensure every remaining bar is processed, I iterate one extra time using a virtual bar of height `0`.

---

## Success Solution

```csharp
public class Solution {
    public int LargestRectangleArea(int[] heights) {

        Stack<int> stack = new Stack<int>();

        int maxArea = 0;
        int height, width;

        for(int i = 0; i <= heights.Length; i++){

            int current = (i == heights.Length)
                ? 0
                : heights[i];

            while(stack.Count > 0 &&
                  heights[stack.Peek()] > current){

                height = heights[stack.Pop()];

                if(stack.Count == 0){
                    width = i;
                }
                else{
                    width = i - stack.Peek() - 1;
                }

                maxArea = Math.Max(maxArea, height * width);
            }

            stack.Push(i);
        }

        return maxArea;
    }
}
```

---

## Success Template

```text
Create Stack (stores indices)

maxArea = 0

Traverse from 0 to n (inclusive)

    currentHeight =
    current index height

    If current index == n

        currentHeight = 0

    While stack is not empty
    AND
    currentHeight <
    height at stack top

        Pop index

        Height =
        popped bar height

        If stack becomes empty

            Width = current index

        Else

            Width =
            current index
            -
            previous smaller index
            -
            1

        Area =
        Height × Width

        Update maxArea

    Push current index

Return maxArea
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

## Revision Notes

* Monotonic **Increasing** Stack
* Stack stores **indices**, not heights
* Every bar is pushed once
* Every bar is popped once
* Area is calculated only when a bar is popped
* Current index is the first smaller element on the right
* Stack top after popping is the first smaller element on the left
* Width = `Right Smaller Index - Left Smaller Index - 1`
* Add one extra iteration with a virtual height `0`
* Mental Question: **When a bar is popped, have I discovered both of its boundaries?**