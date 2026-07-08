# LeetCode 525 - Contiguous Array

**Pattern:** Prefix Sum + Dictionary

---

## Problem

Find the maximum length of a contiguous subarray containing an equal number of `0`s and `1`s.

### Test Case 1

```text
Input: nums = [0,1]
Output: 2
```

### Test Case 2

```text
Input: nums = [0,0,1]
Output: 2
```

---

## Approach

### Brute Force

Try every possible contiguous subarray using nested loops.

Maintain `zeroCount` and `oneCount`.

If both counts are equal, calculate the current subarray length and update `maxLength`.

**Time Complexity:** `O(n²)`

### Optimized Approach

Convert:

```text
0 -> -1
1 -> +1
```

Equal numbers of `0`s and `1`s now cancel each other and produce a sum of `0`.

Maintain a running prefix sum.

If the same prefix sum appears again:

```text
currentPrefix - previousPrefix = 0
```

Therefore, the subarray between those two indices has equal numbers of `0`s and `1`s.

Use a Dictionary to store:

```text
prefix sum -> first index
```

If the current sum already exists, calculate the length using the stored index.

If the current sum does not exist, store its current index.

Keep only the first occurrence to get the maximum possible length.

---

## Interview Explanation

I convert every `0` to `-1` and every `1` to `+1`. This converts the problem into finding the longest zero-sum subarray.

I maintain a running prefix sum and store the first index where each prefix sum appears.

If the same prefix sum appears again, the elements between those two indices have a net sum of `0`, meaning they contain an equal number of zeros and ones.

I calculate the distance between the current index and the first stored index and update the maximum length.

---

## Success Solution

```csharp
public class Solution {
    public int FindMaxLength(int[] nums) {
        Dictionary<int,int> res = new Dictionary<int,int>();
        int len=0,maxlen=0,sum=0;
        res[0]=-1;
        for(int i=0;i<nums.Length;i++){
            if(nums[i]==0){
                sum--;
            }
            else{
                sum++;
            }
            if(res.ContainsKey(sum)){
                len=i-res[sum];
                if(len>maxlen){
                    maxlen=len;
                }
            }
            else{
                res[sum]=i;
            }

        }
        return maxlen;
    }
}
```

---

## Success Template

```text
Transform the condition into a target sum.

Maintain a running prefix sum.

Store:
prefix sum -> first index

If the prefix sum already exists:
    calculate current index - first index
    update maximum length

Else:
    store prefix sum -> current index

Never overwrite the first occurrence.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

## Revision Notes

* `0 -> -1`, `1 -> +1`
* Equal counts cancel to `0`
* Same prefix sum again -> subarray sum is `0`
* Dictionary stores `sum -> first index`
* Keep the earliest index for maximum length
* `res[0] = -1` -> empty prefix before index `0`
* `sum == 0` alone only catches valid subarrays starting at index `0`
* Mental question: **Have I seen this same prefix sum before?**
