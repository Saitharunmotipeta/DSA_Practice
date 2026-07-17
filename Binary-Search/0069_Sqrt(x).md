# LeetCode 69 - Sqrt(x)

**Pattern:** Binary Search on Answer

---

## Problem

Given a non-negative integer `x`, return the integer square root of `x`.

The returned integer should be the **floor** of the square root, meaning the largest integer whose square is less than or equal to `x`.

### Test Case 1

```text
Input:
x = 4

Output:
2
```

### Test Case 2

```text
Input:
x = 8

Output:
2

Explanation:
2² = 4
3² = 9
Return 2 because 2 is the largest integer whose square is less than or equal to 8.
```

---

## Approach

### Brute Force

Start from `0` and keep checking:

```text
0²
1²
2²
3²
...
```

Stop when `i² > x`.

Return `i - 1`.

**Time Complexity:** `O(√x)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

The possible answers lie in the range:

```text
0 ... x
```

Instead of checking every value, use Binary Search on this range.

For each middle value:

* If `mid² <= x`, then `mid` is a valid answer.

  * Save it.
  * Search the right half to see if a larger valid answer exists.
* Otherwise, `mid` is too large.

  * Search the left half.

Continue until the search space becomes empty.

---

## Interview Explanation

This problem is an example of **Binary Search on the Answer**.

I am not searching for an element in an array. Instead, I am searching the range of all possible answers.

If `mid² <= x`, then `mid` is a valid square root candidate. However, there might be a larger valid answer, so I store `mid` and continue searching to the right.

If `mid² > x`, then `mid` and every larger value are invalid, so I continue searching to the left.

At the end of the search, the stored answer is the largest integer whose square is less than or equal to `x`.

---

## Success Solution

```csharp
public class Solution {
    public int MySqrt(int x) {
        int left=0,right=x,answer=0;
        while(left<=right){
            int mid = left+(right-left)/2;
            long square = (long)mid * mid;
            if(square <= x){
                answer=mid;
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        return answer;
    }
}
```

---

## Success Template

```text
left = minimum possible answer
right = maximum possible answer
answer = default value

while(left <= right)

    mid = left + (right - left) / 2

    if(mid is a valid answer)

        answer = mid
        left = mid + 1

    else

        right = mid - 1

return answer
```

---

## Complexity

**Time Complexity:** `O(log x)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Search the range of possible answers instead of an array.
* Store the current valid answer before searching for a better one.
* If `mid² <= x`, move right to look for a larger valid answer.
* If `mid² > x`, move left because `mid` is too large.
* Use `long` when calculating `mid * mid` to avoid integer overflow.
* Return the stored `answer`, not `left` or `right`.
* Mental Question: **Is `mid` a valid answer? If yes, can there be a larger valid one?**
