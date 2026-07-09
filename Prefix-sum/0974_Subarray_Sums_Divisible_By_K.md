# LeetCode 974 - Subarray Sums Divisible by K

**Pattern:** Prefix Sum + Modulo + Dictionary

---

## Problem

Find the number of contiguous subarrays whose sum is divisible by `k`.

A subarray is valid when:

```text
subarraySum % k == 0
```

### Test Case 1

```text
Input:
nums = [4,5,0,-2,-3,1]
k = 5

Output:
7
```

### Test Case 2

```text
Input:
nums = [5]
k = 9

Output:
0
```

---

## Approach

### Brute Force

Try every possible contiguous subarray using nested loops.

Maintain the subarray sum while expanding the inner loop.

If:

```text
sum % k == 0
```

increment the count.

**Time Complexity:** `O(n²)`

### Optimized Approach

For a subarray:

```text
subarray sum = current prefix - previous prefix
```

If two prefix sums have the same remainder when divided by `k`, their difference is divisible by `k`.

Example:

```text
23 % 5 = 3
8 % 5 = 3

23 - 8 = 15

15 % 5 = 0
```

So maintain a running prefix sum and calculate its normalized remainder.

Use a Dictionary to store:

```text
remainder -> frequency
```

If the same remainder appeared before, every previous occurrence creates a valid subarray with the current prefix.

Therefore:

```text
count += frequency of remainder
```

Then increment the current remainder frequency.

---

## Interview Explanation

I use prefix sum and modulo.

A subarray sum is the difference between two prefix sums.

If two prefix sums have the same remainder when divided by `k`, their difference is divisible by `k`.

I maintain the running prefix sum and calculate its normalized remainder.

The Dictionary stores the frequency of every remainder seen before.

When the same remainder appears again, I add its frequency to the count because every previous occurrence represents a valid starting prefix.

---

## Success Solution

```csharp
public class Solution {
    public int SubarraysDivByK(int[] nums, int k) {
        Dictionary<int,int> res = new Dictionary<int,int>();
        int c=0,sum=0,r=0;
        res[0]=1;
        for(int i=0;i<nums.Length;i++){
                sum=sum+nums[i];
                r=((sum%k)+k)%k;
                if(res.ContainsKey(r)){
                    c+=res[r];
                    res[r]++;
                }
                else{
                    res[r]=1;
                }
        }
        return c;
    }
}
```

---

## Success Template

```text
Maintain running prefix sum.

Calculate normalized remainder.

Dictionary stores:
remainder -> frequency

If remainder exists:
    count += frequency
    increment frequency

Else:
    store remainder with frequency 1

Return count.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(k)` because only normalized remainders from `0` to `k - 1` can be stored.

---

## Revision Notes

* Same remainder -> prefix difference divisible by `k`
* Dictionary stores `remainder -> frequency`
* Counting problem -> store frequency
* `res[0] = 1` -> empty prefix has remainder `0`
* `((sum % k) + k) % k` -> normalized non-negative remainder
* `-2` and `3` belong to the same modulo group when `k = 5`
* Add frequency before incrementing current remainder
* Mental question: **Have I seen this same remainder before?**
