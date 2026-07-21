# 1011. Capacity To Ship Packages Within D Days

## Problem

A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `i-th` package has weight `weights[i]`.

Rules:

- Packages **must be shipped in order**.
- The ship has a fixed capacity.
- The total weight loaded in one day cannot exceed the ship's capacity.

Return the **minimum ship capacity** required to ship all packages within the given number of days.

---

## Brute Force Approach

Try every possible ship capacity.

The smallest possible capacity is:

```
Maximum value in weights
```

The largest possible capacity is:

```
Sum of all weights
```

For every capacity:

1. Simulate shipping all packages.
2. Count how many days are required.
3. If the packages can be shipped within the given days, return that capacity.

### Time Complexity

```
O(n × (sum(weights) - max(weights)))
```

Since every possible capacity is tested, this becomes inefficient for large inputs.

---

## Optimized Approach (Binary Search on Answer)

Instead of checking every capacity, Binary Search is performed on the possible ship capacities.

### Search Space

Minimum possible capacity:

```
Maximum value in weights
```

Maximum possible capacity:

```
Sum of all weights
```

For every candidate capacity (`mid`):

- Simulate shipping.
- Count the number of days required.
- If the packages can be shipped within the allowed days,
  - Save the capacity.
  - Try a smaller capacity.
- Otherwise,
  - Increase the capacity.

---

## Mental Model

```
Possible Capacities

10 11 12 13 14 15
❌ ❌ ❌ ✅ ✅ ✅
```

We are looking for the **first valid capacity**.

This is the same Binary Search on Answer pattern used in **Koko Eating Bananas (875)**.

---

## Simulating Shipping

Maintain:

```csharp
currentLoad
currentDays
```

For every package:

If the package fits:

```csharp
currentLoad += weights[i];
```

Otherwise:

- Ship leaves.
- Start a new day.
- Load the current package into the new ship.

```csharp
currentDays++;
currentLoad = weights[i];
```

Finally,

```csharp
return currentDays <= days;
```

---

## Interview Explanation

"We are not searching the array.

We are searching for the minimum ship capacity.

For every candidate capacity, we simulate shipping all packages while preserving their order.

Since the answers follow the monotonic pattern:

```
❌ ❌ ❌ ✅ ✅ ✅
```

Binary Search can be used to find the first valid capacity."

---

## Accepted Solution

```csharp
public class Solution
{
    bool isValid(int[] weights,int days, int mid)
    {
        int currdays = 1, sum = 0;

        for(int i = 0; i < weights.Length; i++)
        {
            sum += weights[i];

            if(sum > mid)
            {
                currdays++;
                sum = weights[i];
            }
        }

        return currdays <= days;
    }

    public int ShipWithinDays(int[] weights, int days)
    {
        int left = weights.Max();
        int right = weights.Sum();
        int answer = 0;

        while(left <= right)
        {
            int mid = left + (right - left) / 2;

            if(isValid(weights, days, mid))
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
Maximum weight

O(n)
```

Finding:

```
Total weight

O(n)
```

Binary Search:

```
O(log(sum(weights)))
```

Validation:

```
O(n)
```

Overall:

```
Time : O(n log(sum(weights)))

Space : O(1)
```

---

## Mistakes I Made While Learning

### 1. Initially thought the brute force could use the average weight.

The packages must remain in order, so average weight does not determine the required capacity.

---

### 2. Tried to stop (`break`) when capacity overflowed.

Actually, the current package becomes the first package of the next day.

Correct:

```csharp
currdays++;
sum = weights[i];
```

---

### 3. Started counting days from 0.

A shipment always starts on Day 1.

Correct:

```csharp
currdays = 1;
```

---

### 4. Used

```csharp
right = mid;
```

This can cause Binary Search to get stuck.

Correct:

```csharp
right = mid - 1;
```

---

### 5. Initially misunderstood Binary Search.

Binary Search is not performed on package indices.

It is performed on the **possible ship capacities**.

---

## Revision Notes

- Binary Search on Answer.
- Search over possible capacities, not indices.
- Search space is `max(weights)` to `sum(weights)`.
- Simulate shipping to validate a capacity.
- Count required days while preserving package order.
- Search for the first valid capacity.