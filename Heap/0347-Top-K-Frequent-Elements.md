# LeetCode 347 - Top K Frequent Elements

**Pattern:** HashMap + Min Heap

---

## Problem

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

You may return the answer in any order.

### Test Case 1

```text
Input:
nums = [1,1,1,2,2,3]

k = 2

Output:
[1,2]
```

### Test Case 2

```text
Input:
nums = [1]

k = 1

Output:
[1]
```

---

## Approach

### Brute Force

- Traverse the array and count the frequency of every element.
- Store all unique elements along with their frequencies.
- Sort them based on frequency in descending order.
- Return the first `k` elements.

Time Complexity:

**O(n log n)**

---

### Optimized Approach

Use a combination of a **HashMap** and a **Min Heap**.

First, build a frequency map where:

- Key → Number
- Value → Frequency

Then iterate through the frequency map.

For every `(Number, Frequency)` pair:

- Insert it into a Min Heap.
- Use the frequency as the priority.
- If the heap size exceeds `k`, remove the smallest frequency.

At the end, the heap contains exactly the `k` most frequent elements.

Extract every remaining element from the heap and return them.

---

## Interview Explanation

I first count the frequency of every element using a Dictionary.

Instead of sorting all unique elements, I maintain a Min Heap of size `k`.

Each heap node stores the number as the element and its frequency as the priority.

Whenever the heap grows larger than `k`, I remove the element with the smallest frequency.

This guarantees that the heap always contains the `k` most frequent elements.

Finally, I remove every remaining element from the heap and store it in the answer array.

---

## Success Solution

```csharp
public class Solution {
    public int[] TopKFrequent(int[] nums, int k) {
        Dictionary<int,int> freq = new Dictionary<int,int>();
        PriorityQueue<int,int> pq = new PriorityQueue<int,int>();

        int[] ans = new int[k];

        int i;

        for(i = 0; i < nums.Length; i++){
            if(freq.ContainsKey(nums[i])){
                freq[nums[i]]++;
            }
            else{
                freq.Add(nums[i],1);
            }
        }

        var enumerator = freq.GetEnumerator();

        while(enumerator.MoveNext()){
            pq.Enqueue(enumerator.Current.Key, enumerator.Current.Value);

            if(pq.Count > k){
                pq.Dequeue();
            }
        }

        for(int j = 0; j < k; j++){
            ans[j] = pq.Dequeue();
        }

        return ans;
    }
}
```

---

## Success Template

```text
Create Frequency Map

Traverse Array

    Update Frequency

Create Min Heap

Traverse Frequency Map

    Insert (Number, Frequency)

    If Heap Size > k

        Remove Smallest Frequency

Create Answer Array

Remove every remaining element

Return Answer
```

---

## Complexity

**Time Complexity:** `O(n log k)`

**Space Complexity:** `O(n)`

---

## Revision Notes

- Combine **Dictionary** and **Min Heap**
- Dictionary stores `(Number → Frequency)`
- Heap stores `(Element = Number, Priority = Frequency)`
- Heap size never exceeds `k`
- Remove the smallest frequency whenever heap size exceeds `k`
- The heap always contains the `k` most frequent elements
- Extract every remaining element using `Dequeue()`
- Answer order does **not** matter

### Understanding Enumerator

`Dictionary` is **not index-based**, so we cannot access elements using:

```csharp
freq[i]    // ❌ Invalid
```

Instead, we use an **Enumerator** to visit each key-value pair one by one.

```csharp
var enumerator = freq.GetEnumerator();
```

Move to the next element:

```csharp
enumerator.MoveNext();
```

Access the current element:

```csharp
enumerator.Current.Key
enumerator.Current.Value
```

Equivalent `foreach` loop:

```csharp
foreach(var term in freq)
{
    // term.Key
    // term.Value
}
```

Enumerator simply gives us manual control over the iteration process, whereas `foreach` performs the same steps internally.

**Mental Question:** Why do we iterate over the **Dictionary** instead of the original array after building the frequency map?

**Answer:** Because the Dictionary already contains each unique element exactly once along with its frequency. Processing the original array would insert duplicate elements into the heap.

---

## Pattern Learned

This problem combines two patterns:

```text
Array

↓

Dictionary (Frequency Count)

↓

Fixed Size Min Heap

↓

Answer
```