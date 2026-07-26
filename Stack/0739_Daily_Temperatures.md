# LeetCode 739 - Daily Temperatures

**Pattern:** Monotonic Decreasing Stack

---

## Problem

Given an integer array `temperatures` representing the daily temperatures, return an array `answer` such that:

* `answer[i]` is the number of days you have to wait after the `i-th` day to get a warmer temperature.
* If there is no future warmer temperature, return `0` for that day.

### Test Case 1

```text
Input:
temperatures = [73,74,75,71,69,72,76,73]

Output:
[1,1,4,2,1,1,0,0]
```

### Test Case 2

```text
Input:
temperatures = [30,40,50,60]

Output:
[1,1,1,0]
```

---

## Approach

### Brute Force

For every temperature:

* Traverse all future temperatures.
* Stop when a warmer temperature is found.
* Store the number of days waited.

Time Complexity: **O(n²)**

---

### Optimized Approach

Use a **Monotonic Decreasing Stack** that stores **indices**.

* Push indices whose warmer temperature has not been found yet.
* Whenever the current temperature is greater than the temperature at the top index of the stack:
  * Pop the previous index.
  * Calculate the waiting days using:

```
Current Index - Previous Index
```

* Push the current index.
* Remaining indices automatically have answer `0`.

---

## Interview Explanation

I use a Monotonic Decreasing Stack that stores indices of unresolved temperatures.

While traversing the array, whenever the current temperature is greater than the temperature at the index on the top of the stack, I pop that index and calculate the waiting days as:

```
Current Index - Previous Index
```

Then I push the current index into the stack.

Since every index is pushed exactly once and popped exactly once, the solution runs in linear time.

---

## Success Solution

```csharp
public class Solution {
    public int[] DailyTemperatures(int[] temperatures) {

        Stack<int> stack = new Stack<int>();
        int i = 1, j = 0;
        int[] answer = new int[temperatures.Length];

        stack.Push(0);

        while (i < temperatures.Length) {
            while (stack.Count > 0 &&
                   temperatures[i] > temperatures[stack.Peek()]) {

                j = stack.Pop();
                answer[j] = i - j;
            }

            stack.Push(i);
            i++;
        }

        return answer;
    }
}
```

---

## Success Template

```text
Initialize:

Create Stack (stores indices)

Push first index

Traverse remaining indices

While current temperature >
temperature at stack top

    previousIndex = stack.Pop()

    answer[previousIndex] =
    currentIndex - previousIndex

Push current index

Return answer
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

## Revision Notes

* Store **indices**, not temperatures
* Stack contains unresolved days
* Current warmer temperature can resolve multiple previous days
* Use `while`, not `if`
* Waiting Days = `Current Index - Previous Index`
* Every index is pushed once
* Every index is popped once
* Remaining indices automatically remain `0`
* Mental Question: **Can today's temperature resolve any previous waiting days?**