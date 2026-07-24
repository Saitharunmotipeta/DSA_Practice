# 643. Maximum Average Subarray I

## Problem

You are given an integer array `nums` consisting of `n` elements and an integer `k`.

Find the contiguous subarray of length exactly `k` that has the maximum average value.

Return the maximum average.

---

## Brute Force Approach

Generate every possible subarray of size `k`.

For each subarray:

1. Calculate its sum.
2. Compute its average.
3. Keep track of the maximum average.

### Time Complexity

```
O(n × k)
```

Since the sum is recalculated for every window, the solution becomes inefficient for large arrays.

---

## Optimized Approach (Fixed Sliding Window)

Instead of recalculating every window sum, reuse the previous window.

### Steps

1. Compute the sum of the first `k` elements.
2. Treat it as the first window.
3. For every new window:
   - Remove the leftmost element.
   - Add the next element entering the window.
4. Compute the average.
5. Update the maximum average.

This avoids recalculating the entire window every time.

---

## Sliding Window

Window Size

```
k
```

Initial Window

```
[1 12 -5 -6 50 3]

k = 4

Window

1 12 -5 -6
```

Slide Once

```
12 -5 -6 50
```

Instead of calculating the sum again,

```
New Sum

=

Previous Sum

-

Outgoing Element

+

Incoming Element
```

---

## Mental Model

```
nums

1 12 -5 -6 50 3

Window

[1 12 -5 -6]

↓

[12 -5 -6 50]

↓

[-5 -6 50 3]
```

The window size never changes.

Only one element leaves and one element enters.

---

## Key Formula

Current Window Sum

```csharp
sum = sum - nums[left];
sum = sum + nums[right];
```

Average

```csharp
average = sum / k;
```

---

## Interview Explanation

"We need the maximum average among all contiguous subarrays of fixed length `k`.

Instead of recalculating the sum for every subarray, we compute the first window once.

While sliding the window, we subtract the outgoing element and add the incoming element.

This reduces the complexity from `O(n × k)` to `O(n)`."

---

## Accepted Solution

```csharp
public class Solution
{
    public double FindMaxAverage(int[] nums, int k)
    {
        int i = 0, j = k - 1;
        double sum = 0, maxAvg = double.MinValue;

        for (i = 0; i < k; i++)
        {
            sum += nums[i];
        }

        i = 0;

        while (j < nums.Length)
        {
            double avg = (double)sum / k;

            if (avg > maxAvg)
            {
                maxAvg = avg;
            }

            if (j + 1 < nums.Length)
            {
                sum -= nums[i];
                i++;
                sum += nums[j + 1];
            }

            j++;
        }

        return maxAvg;
    }
}
```

---

## Success Template

```
Build first window

Process current window

while(window can slide)

    Remove left element

    Add right element

    Process new window

Return answer
```

---

## Complexity

Building First Window

```
O(k)
```

Sliding Window

```
O(n-k)
```

Overall

```
Time : O(n)

Space : O(1)
```

---

## Mistakes I Made While Learning

### 1. Initially thought every window sum had to be recalculated.

Instead,

```
Reuse the previous window.
```

---

### 2. Forgot to build the initial window first.

The Sliding Window starts only after the first `k` elements are processed.

---

### 3. Confused Sliding Window with Two Pointers.

Sliding Window maintains a window over **contiguous** elements.

The window moves together while preserving its size.

---

### 4. Calculated the average for every window.

Since `k` is constant, we could also compare window sums and divide only once at the end.

Although both approaches are accepted, comparing sums avoids repeated division.

---

### 5. Needed an extra boundary check.

```csharp
if(j + 1 < nums.Length)
```

to prevent accessing an invalid index.

---

## Revision Notes

- Fixed Sliding Window.
- Window size remains constant.
- Compute the first window once.
- Reuse the previous window sum.
- Remove the outgoing element.
- Add the incoming element.
- Fixed Sliding Window reduces repeated computation from `O(n × k)` to `O(n)`.