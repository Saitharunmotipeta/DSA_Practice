# 1482. Minimum Number of Days to Make m Bouquets

## Problem

You are given an integer array `bloomDay`, where `bloomDay[i]` represents the day on which the `i-th` flower blooms.

You need to make exactly `m` bouquets.

Each bouquet must contain exactly `k` **adjacent** flowers.

A flower can only be used once.

Return the **minimum number of days** required to make `m` bouquets.

If it is impossible to make the required bouquets, return `-1`.

---

## Brute Force Approach

Try every possible day from

```
Minimum bloom day
```

to

```
Maximum bloom day
```

For every day:

1. Mark all flowers that have bloomed.
2. Count consecutive bloomed flowers.
3. Whenever `k` consecutive flowers are found, form one bouquet.
4. Continue until all flowers are processed.
5. If at least `m` bouquets are formed, return that day.

### Time Complexity

```
O(n × (max(bloomDay) - min(bloomDay)))
```

This becomes inefficient when the day range is large.

---

## Optimized Approach (Binary Search on Answer)

Instead of checking every day individually, Binary Search is performed on the possible answer.

The answer represents the **minimum day** required to make all bouquets.

For every candidate day (`mid`):

- Consider every flower with

```csharp
bloomDay[i] <= mid
```

as bloomed.

- Count consecutive bloomed flowers.

- Whenever consecutive flowers become equal to `k`,
  - One bouquet is formed.
  - Reset the consecutive count because flowers cannot be reused.

- If an unbloomed flower is encountered,
  - Reset the consecutive count because adjacency is broken.

If at least `m` bouquets can be formed, try finding a smaller day.

Otherwise, search larger days.

---

## Search Space

Minimum possible day:

```
Minimum value in bloomDay
```

Maximum possible day:

```
Maximum value in bloomDay
```

Reason:

Before the minimum bloom day, no flowers are available.

After the maximum bloom day, all flowers have already bloomed.

---

## Mental Model

```
Possible Days

1 2 3 4 5 6 7 8 9 10

❌ ❌ ❌ ❌ ✅ ✅ ✅ ✅ ✅ ✅
```

We are searching for the **first day** on which at least `m` bouquets can be formed.

Once a day becomes valid, every larger day is also valid because more flowers continue blooming.

---

## Validation Logic

Maintain two variables:

```csharp
bouquets
consecutiveFlowers
```

For every flower:

If

```csharp
bloomDay[i] <= mid
```

then

```csharp
consecutiveFlowers++;
```

If

```csharp
consecutiveFlowers == k
```

then

```csharp
bouquets++;
consecutiveFlowers = 0;
```

because flowers cannot be reused.

If

```csharp
bloomDay[i] > mid
```

then

```csharp
consecutiveFlowers = 0;
```

because adjacency is broken.

Finally,

```csharp
return bouquets >= m;
```

---

## Interview Explanation

"We are not searching the bloomDay array.

We are searching for the minimum day required to make `m` bouquets.

The search space ranges from the earliest bloom day to the latest bloom day.

For every candidate day, we simulate which flowers have bloomed and count the maximum number of bouquets that can be formed using consecutive flowers.

Since increasing the number of days only increases the number of bloomed flowers, the answers follow a monotonic pattern:

```
❌ ❌ ❌ ✅ ✅ ✅
```

Therefore, Binary Search on Answer can be applied."

---

## Accepted Solution

```csharp
public class Solution
{
    bool isValid(int[] bloomDay, int mid, int k, int m)
    {
        int bouquets = 0;
        int consecutive = 0;

        for (int i = 0; i < bloomDay.Length; i++)
        {
            if (bloomDay[i] <= mid)
            {
                consecutive++;

                if (consecutive == k)
                {
                    bouquets++;
                    consecutive = 0;
                }
            }
            else
            {
                consecutive = 0;
            }
        }

        return bouquets >= m;
    }

    public int MinDays(int[] bloomDay, int m, int k)
    {
        if ((long)m * k > bloomDay.Length)
            return -1;

        int left = bloomDay.Min();
        int right = bloomDay.Max();
        int answer = -1;

        while (left <= right)
        {
            int mid = left + (right - left) / 2;

            if (isValid(bloomDay, mid, k, m))
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
Minimum bloom day

O(n)
```

Finding:

```
Maximum bloom day

O(n)
```

Binary Search:

```
O(log(maxDay - minDay))
```

Validation:

```
O(n)
```

Overall:

```
Time : O(n log(maxDay - minDay))

Space : O(1)
```

---

## Mistakes I Made While Learning

### 1. Used `< mid` instead of `<= mid`

A flower blooms **on** its bloom day.

Correct:

```csharp
bloomDay[i] <= mid
```

---

### 2. Returned

```csharp
bouquets <= m
```

The requirement is to make **at least** `m` bouquets.

Correct:

```csharp
bouquets >= m
```

---

### 3. Updated the answer before validation

Initially wrote:

```csharp
answer = mid;
```

before checking whether the current day was valid.

The answer should only be updated after `isValid()` returns `true`.

---

### 4. Forgot the impossible case

Initially skipped checking whether enough flowers even existed.

Correct:

```csharp
if ((long)m * k > bloomDay.Length)
    return -1;
```

---

### 5. Initially forgot why the consecutive counter resets

After forming one bouquet:

```csharp
consecutive = 0;
```

Flowers cannot be reused.

When an unbloomed flower appears:

```csharp
consecutive = 0;
```

Adjacency is broken.

---

## Revision Notes

- Binary Search on Answer.
- Search over possible days, not array indices.
- Search space is `min(bloomDay)` to `max(bloomDay)`.
- Validation is based on consecutive bloomed flowers.
- Reset consecutive count after forming a bouquet.
- Reset consecutive count when an unbloomed flower breaks adjacency.
- Return the first valid day.