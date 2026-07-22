# 1283. Find the Smallest Divisor Given a Threshold

## Problem

Given an array of positive integers `nums` and an integer `threshold`, choose a positive integer divisor.

For every element:

- Divide the number by the divisor.
- Round the result **up** to the nearest integer.
- Sum all the rounded values.

Return the **smallest divisor** such that the total sum is **less than or equal to** `threshold`.

---

## Brute Force Approach

Try every possible divisor.

The smallest possible divisor is:

```
1
```

The largest useful divisor is:

```
Maximum value in nums
```

For every divisor:

1. Divide every number.
2. Round each division upward.
3. Calculate the total sum.
4. If the total is less than or equal to the threshold, return that divisor.

### Time Complexity

```
O(n × max(nums))
```

Since every possible divisor is tested, this approach becomes inefficient for large values.

---

## Optimized Approach (Binary Search on Answer)

Instead of checking every divisor, Binary Search is performed on the possible divisors.

### Search Space

Minimum possible divisor:

```
1
```

Maximum possible divisor:

```
Maximum value in nums
```

For every candidate divisor (`mid`):

- Divide every number.
- Compute the ceiling value.
- Add all results.
- If the total is within the threshold,
  - Save the divisor.
  - Try a smaller divisor.
- Otherwise,
  - Increase the divisor.

---

## Mental Model

```
Possible Divisors

1 2 3 4 5 6 7 8 9

❌ ❌ ❌ ❌ ✅ ✅ ✅ ✅ ✅
```

Small divisors produce larger sums.

Large divisors produce smaller sums.

Once a divisor becomes valid, every larger divisor is also valid.

We are searching for the **first valid divisor**.

---

## Validation Logic

Maintain:

```csharp
sum
```

For every number:

```csharp
sum += Ceiling(num / divisor)
```

Finally,

```csharp
return sum <= threshold;
```

---

## Why We Use

```csharp
(nums[i] + mid - 1) / mid
```

Instead of writing

```csharp
Math.Ceiling((double)nums[i] / mid)
```

we use integer arithmetic.

This formula computes

```
Ceiling(nums[i] / mid)
```

without using floating-point numbers.

Example:

```
9 / 5

= 1.8

Ceiling = 2
```

Formula:

```
(9 + 5 - 1) / 5

13 / 5

= 2
```

Another example:

```
2 / 5

= 0.4

Ceiling = 1
```

Formula:

```
(2 + 5 - 1) / 5

6 / 5

= 1
```

Another example:

```
10 / 5

= 2
```

Formula:

```
(10 + 5 - 1) / 5

14 / 5

= 2
```

This trick works because adding `(divisor - 1)` pushes every non-exact division into the next integer before integer division occurs.

This is a standard interview technique whenever positive integers require ceiling division.

---

## Interview Explanation

"We are not searching inside the array.

We are searching for the smallest possible divisor.

For every candidate divisor, we calculate the sum of the ceiling divisions.

As the divisor increases, the total sum never increases.

Therefore the answers follow a monotonic pattern:

```
❌ ❌ ❌ ❌ ✅ ✅ ✅ ✅
```

Binary Search can be used to find the first valid divisor."

---

## Accepted Solution

```csharp
public class Solution {

    bool isValid(int[] nums, int mid, int threshold)
    {
        int sum = 0;

        for(int i = 0; i < nums.Length; i++)
        {
            int r = (nums[i] + mid - 1) / mid;
            sum += r;
        }

        return sum <= threshold;
    }

    public int SmallestDivisor(int[] nums, int threshold)
    {
        int left = 1;
        int right = nums.Max();
        int answer = 0;

        while(left <= right)
        {
            int mid = left + (right - left) / 2;

            if(isValid(nums, mid, threshold))
            {
                answer = mid;
                right = mid - 1;
            }
            else
            {
                left = mid + 1;
            }
        }

        return answer;
    }
}
```

---

## Success Template

```
Search Space

left = minimum possible answer

right = maximum possible answer

while(left <= right)

    mid = candidate answer

    if(candidate is valid)

        save answer

        search left half

    else

        search right half

return answer
```

---

## Complexity

Finding:

```
Maximum value in nums

O(n)
```

Binary Search:

```
O(log(max(nums)))
```

Validation:

```
O(n)
```

Overall:

```
Time : O(n log(max(nums)))

Space : O(1)
```

---

## Mistakes I Made While Learning

### 1. Initially thought the search space should be

```
nums.Min() → nums.Max()
```

Actually, the smallest possible divisor is always

```
1
```

---

### 2. Tried using

```csharp
Math.Ceiling(nums[i] / mid)
```

Integer division happens before `Math.Ceiling()`, producing incorrect results.

---

### 3. Forgot the ceiling division trick

Instead of floating-point arithmetic, use

```csharp
(nums[i] + mid - 1) / mid
```

---

### 4. Initially wrote

```csharp
right = mid;
```

Correct:

```csharp
right = mid - 1;
```

Otherwise Binary Search may not terminate correctly.

---

### 5. Compared using

```csharp
sum < threshold
```

The problem requires

```csharp
sum <= threshold
```

---

### 6. Forgot parentheses while implementing the ceiling formula

Incorrect:

```csharp
nums[i] + mid - 1 / mid
```

Correct:

```csharp
(nums[i] + mid - 1) / mid
```

---

## Revision Notes

- Binary Search on Answer.
- Search over possible divisors, not indices.
- Search space is `1` to `max(nums)`.
- Validation computes the sum of ceiling divisions.
- Use integer ceiling formula instead of `Math.Ceiling()`.
- Search for the first valid divisor.
- Remember the pattern:

```
❌ ❌ ❌ ❌ ✅ ✅ ✅
```

This indicates Binary Search on Answer.
