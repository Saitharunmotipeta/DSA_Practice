# 875. Koko Eating Bananas

## Problem

Koko loves to eat bananas. There are `n` piles of bananas, where the `i-th` pile has `piles[i]` bananas.

The guards will return in `h` hours.

Koko can decide her eating speed `k` (bananas per hour).

Rules:
- She chooses **one pile** every hour.
- She eats **at most `k` bananas** from that pile.
- If the pile has fewer than `k` bananas, she eats all of them and waits until the next hour.

Return the **minimum integer `k`** such that Koko can finish all the bananas within `h` hours.

---

## Brute Force Approach

Try every possible eating speed from **1** to the **largest pile**.

For each speed:

1. Calculate the total hours needed.
2. If the hours are less than or equal to `h`, return that speed.

### Time Complexity

```
O(n × maxPile)
```

Since every possible speed is tested, this becomes inefficient for large values.

---

## Optimized Approach (Binary Search on Answer)

Instead of searching the array, we search the **possible answers**.

### Search Space

Minimum possible eating speed:

```
1
```

Maximum possible eating speed:

```
Maximum value in piles
```

Now Binary Search over this range.

For every candidate speed (`mid`):

- Calculate how many hours Koko needs.
- If she can finish within `h` hours,
  - Save this speed.
  - Try to find an even smaller valid speed.
- Otherwise,
  - Increase the speed.

---

## Mental Model

```
Possible Speeds

1   2   3   4   5   6   7
❌  ❌  ❌  ✅  ✅  ✅  ✅
```

We are looking for the **first valid speed**.

This is exactly the same Binary Search pattern as **First Bad Version (278)**.

---

## Calculating Hours

For one pile,

```
pile = 11
speed = 4
```

Hours needed:

```
11 / 4 = 2.75

Ceiling = 3
```

Using integer arithmetic:

```csharp
hours += (pile + speed - 1) / speed;
```

Examples:

```
11,4

(11+4-1)/4

14/4

3
```

```
10,5

(10+5-1)/5

14/5

2
```

```
3,4

(3+4-1)/4

6/4

1
```

---

## Interview Explanation

"We are not searching for bananas.

We are searching for the minimum eating speed.

For every candidate speed, we check whether Koko can finish all piles within `h` hours.

Since the answers follow the pattern:

```
❌ ❌ ❌ ✅ ✅ ✅
```

we can Binary Search for the first valid answer."

---

## Accepted Solution

```csharp
public class Solution
{
    public bool IsValid(int[] piles, int h, int k)
    {
        long hours = 0;

        for (int i = 0; i < piles.Length; i++)
        {
            hours += ((long)piles[i] + k - 1) / k;
        }

        return hours <= h;
    }

    public int MinEatingSpeed(int[] piles, int h)
    {
        int left = 1;
        int right = piles.Max();
        int answer = right;

        while (left <= right)
        {
            int mid = left + (right - left) / 2;

            if (IsValid(piles, h, mid))
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

Finding maximum pile:

```
O(n)
```

Binary Search:

```
O(log(maxPile))
```

Validation:

```
O(n)
```

Overall:

```
Time : O(n log(maxPile))

Space : O(1)
```

---

## Mistakes I Made While Learning

### 1. Thought Binary Search was on pile indices.

Actually, Binary Search is on the **possible eating speed**.

---

### 2. Used

```csharp
right = piles.Length;
```

Correct:

```csharp
right = piles.Max();
```

because the answer is a speed, not an index.

---

### 3. Started with

```csharp
left = 0;
```

This caused division by zero.

Minimum possible speed is **1**.

---

### 4. Incorrect hour calculation.

Initially tried:

```csharp
hours += k;
```

Hours depend on the pile size, not the speed itself.

Correct:

```csharp
hours += (pile + k - 1) / k;
```

---

### 5. Integer Overflow

Using

```csharp
int hours
```

failed for:

```
[805306368,805306368,805306368]
```

because the total exceeded `int.MaxValue`.

Fixed by using:

```csharp
long hours = 0;
```

and

```csharp
((long)piles[i] + k - 1) / k;
```

---

## Revision Notes

- Binary Search on Answer.
- Search over possible answers, not indices.
- Identify monotonic validity pattern.
- Use a helper function to validate a candidate answer.
- Search for the first valid answer.
- Watch for integer overflow when accumulating results.