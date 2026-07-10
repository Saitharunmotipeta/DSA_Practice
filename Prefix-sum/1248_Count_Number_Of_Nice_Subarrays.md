# LeetCode 1248 - Count Number of Nice Subarrays

**Pattern:** Prefix Sum + Dictionary

---

## Problem

Find the number of contiguous subarrays containing exactly `k` odd numbers.

### Test Case 1

```text
Input:
nums = [1,1,2,1,1]
k = 3

Output:
2
```

### Test Case 2

```text
Input:
nums = [2,4,6]
k = 1

Output:
0
```

---

## Approach

### Brute Force

Try every possible contiguous subarray using nested loops.

Maintain the number of odd elements in the current subarray.

If the odd count is equal to `k`, increment the count.

**Time Complexity:** `O(n²)`

### Optimized Approach

Categorize the numbers as:

```text
Odd  -> 1
Even -> 0
```

Now the problem becomes finding the number of subarrays whose sum is exactly `k`.

Maintain `odd`, which represents the number of odd elements seen from the beginning to the current index.

Use:

```text
odd - req = k
```

Therefore:

```text
req = odd - k
```

`req` represents the previous odd-prefix count needed so that the subarray between the previous prefix and current prefix contains exactly `k` odd numbers.

Use a Dictionary to store:

```text
odd prefix count -> frequency
```

Search for `req`.

If `req` exists, add its frequency to `count`.

After searching, store or increment the current `odd` prefix frequency.

---

## Interview Explanation

I treat every odd number as `1` and every even number as `0`.

This converts the problem into counting subarrays whose sum is exactly `k`.

I maintain a running odd-prefix count.

For every current odd count, I calculate `odd - k` to find the previous odd-prefix count required to leave exactly `k` odd numbers in the subarray.

The Dictionary stores the frequency of each odd-prefix count.

If the required prefix exists, every occurrence represents a valid starting prefix, so I add its frequency to the answer.

Then I store the current odd-prefix count for future indices.

---

## Success Solution

```csharp
public class Solution {
    public int NumberOfSubarrays(int[] nums, int k) {
        Dictionary<int,int> freq = new Dictionary<int,int>();
        int i=0,odd=0,count=0;
        freq[0]=1;
        while(i<nums.Length){
            if(nums[i]%2!=0){
                odd++;
            }
            int req = odd -k;
            if(freq.ContainsKey(req)){
                count = count +freq[req];
            }
            if(freq.ContainsKey(odd)){
                freq[odd]++;
            }
            else{
                freq[odd]=1;
            }
            i++;
        }
        return count;
    }
}
```

---

## Success Template

```text
Transform the required condition into a count.

Maintain the current prefix count.

required previous prefix:

req = currentPrefix - k

Dictionary stores:

prefix count -> frequency

SEARCH req.

If req exists:
    count += frequency of req

STORE currentPrefix.

If currentPrefix exists:
    increment frequency
Else:
    initialize frequency as 1
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

## Revision Notes

* Odd -> `1`, Even -> `0`
* `odd` = current odd-prefix count
* `req` = previous odd-prefix count needed
* `odd - req = k`
* Therefore `req = odd - k`
* `freq[req]` counts valid previous prefix boundaries
* `freq[odd]` counts how many prefixes have seen exactly `odd` odd numbers
* Even numbers can cause `freq[odd]++` because `odd` stays unchanged
* `freq[0] = 1` -> empty prefix with `0` odd numbers
* **SEARCH `req`, STORE `odd`**
* Mental question: **What previous odd-prefix count do I need to leave exactly `k` odds?**
