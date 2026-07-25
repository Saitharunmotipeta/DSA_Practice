# 496. Next Greater Element I

**Difficulty:** Easy

**Pattern:** Monotonic Stack | HashMap (Dictionary)

**LeetCode:** https://leetcode.com/problems/next-greater-element-i/

---

# Problem Statement

You are given two arrays:

- `nums1`
- `nums2`

Every element in `nums1` appears exactly once in `nums2`.

For every element in `nums1`, find the **first greater element to its right** in `nums2`.

If no greater element exists, return `-1`.

---

# Example

```
nums1 = [4,1,2]

nums2 = [1,3,4,2]
```

Answer

```
[-1,3,-1]
```

Explanation

```
4 → No greater element → -1

1 → First greater element is 3

2 → No greater element → -1
```

---

# Brute Force Idea

For every element in `nums1`

- Find its position in `nums2`
- Traverse towards the right
- Return the first greater element

Example

```
nums2

1 3 4 2
```

Finding next greater for `1`

```
3 4 2
```

First greater

```
3
```

Repeat this for every element.

---

# Why Brute Force Fails

For every element

```
Search

↓

Traverse remaining array
```

Worst Case

```
O(n²)
```

This becomes slow for large inputs.

---

# Key Observation

Suppose

```
nums2

2 1 5
```

When we reach

```
5
```

We instantly know

```
1 → 5

2 → 5
```

One number can determine the answers for multiple previous elements.

Instead of letting every element search to its right,

let every new element resolve the previous smaller elements.

This leads to the **Monotonic Stack** approach.

---

# Monotonic Stack

The stack stores

> **Elements whose next greater element has not been found yet.**

Example

```
nums2

4 2 6 3
```

Read

```
4
```

Stack

```
4
```

---

Read

```
2
```

Stack

```
Top
↓

2
4
```

---

Read

```
6
```

Current

```
6 > 2
```

So

```
2 → 6
```

Pop.

Again

```
6 > 4
```

So

```
4 → 6
```

Pop.

Push

```
6
```

Stack

```
6
```

Dictionary

```
{
2 → 6
4 → 6
}
```

---

Read

```
3
```

Push

```
Top
↓

3
6
```

End of traversal.

Remaining stack

```
3
6
```

These never found a greater element.

Therefore

```
3 → -1

6 → -1
```

---

# Mental Model

Think of the stack as a queue of **waiting elements**.

Every element inside the stack is waiting for a larger number.

Whenever a larger number arrives

- Resolve everyone smaller than it.
- Store the answer.
- Push the current number to wait for its own answer.

---

# Algorithm

### Step 1

Create

- Stack
- Dictionary

---

### Step 2

Traverse `nums2`

While

```
Current > Stack Top
```

Pop the stack.

Store

```
Popped Element → Current Element
```

After resolving all smaller elements,

Push the current element.

---

### Step 3

Traversal finished.

Every element still inside the stack has no greater element.

Assign

```
Element → -1
```

---

### Step 4

Traverse `nums1`

Replace every value using the dictionary.

Return the result.

---

# Accepted Solution

```csharp
public class Solution {
    public int[] NextGreaterElement(int[] nums1, int[] nums2) {
        Stack<int> stack = new Stack<int>();
        Dictionary<int, int> dict = new Dictionary<int, int>();

        for (int i = 0; i < nums2.Length; i++) {
            while (stack.Count != 0 && nums2[i] > stack.Peek()) {
                dict[stack.Pop()] = nums2[i];
            }

            stack.Push(nums2[i]);
        }

        while (stack.Count != 0) {
            dict[stack.Pop()] = -1;
        }

        for (int i = 0; i < nums1.Length; i++) {
            nums1[i] = dict[nums1[i]];
        }

        return nums1;
    }
}
```

---

# Dry Run

Input

```
nums1 = [4,1,2]

nums2 = [1,3,4,2]
```

Initially

```
Stack = []

Dictionary = {}
```

---

Read

```
1
```

Stack

```
1
```

---

Read

```
3
```

```
3 > 1
```

Pop

```
1 → 3
```

Stack

```
3
```

Dictionary

```
{
1 → 3
}
```

---

Read

```
4
```

```
4 > 3
```

Pop

```
3 → 4
```

Stack

```
4
```

Dictionary

```
{
1 → 3
3 → 4
}
```

---

Read

```
2
```

Push

```
Top
↓

2
4
```

---

Traversal finished.

Remaining stack

```
2
4
```

Assign

```
2 → -1

4 → -1
```

Final Dictionary

```
{
1 → 3
3 → 4
2 → -1
4 → -1
}
```

Now build answer

```
4 → -1

1 → 3

2 → -1
```

Answer

```
[-1,3,-1]
```

---

# Why Time Complexity is O(n)

At first glance

```
for

↓

while
```

looks like

```
O(n²)
```

But every element is

- Pushed exactly once
- Popped exactly once

Example

```
1 2 3 4 5
```

Operations

```
Push 1

Pop 1

Push 2

Pop 2

Push 3

Pop 3

...
```

Each element participates in at most

```
One Push

One Pop
```

Total stack operations

```
2n
```

Therefore

```
O(n)
```

This is known as **Amortized Analysis**.

---

# Time Complexity

Building Dictionary

```
O(n)
```

Building Answer

```
O(m)
```

Overall

```
O(n + m)
```

where

- `n = nums2.Length`
- `m = nums1.Length`

---

# Space Complexity

Dictionary

```
O(n)
```

Stack

```
O(n)
```

Overall

```
O(n)
```

---

# Key Learning

- First Monotonic Stack problem.
- Learn to preprocess information using a dictionary.
- The stack stores **waiting elements**, not processed elements.
- One larger element can resolve multiple previous elements.
- A `while` loop inside a `for` loop does **not** necessarily mean `O(n²)`.

---

# Interview Explanation

"I use a Monotonic Decreasing Stack to keep track of elements whose next greater element hasn't been found yet.

As I traverse `nums2`, whenever the current element is greater than the stack's top, I repeatedly pop smaller elements and record the current element as their next greater value in a dictionary.

After processing the entire array, any elements left in the stack have no greater element, so I map them to `-1`.

Finally, I iterate through `nums1` and replace each value with its corresponding next greater element from the dictionary.

Since every element is pushed and popped at most once, the solution runs in O(n + m) time."

---

# Common Mistakes

❌ Using `if` instead of `while`

Only one element gets resolved.

---

❌ Pushing only inside `else`

The current element must always be pushed after resolving smaller elements.

---

❌ Forgetting to assign `-1` to remaining stack elements.

---

❌ Assuming `for + while` means `O(n²)`.

Use amortized analysis.

---

# Revision Notes

Remember

```
Current Number Arrives

↓

Resolve everyone smaller than it

↓

Store answers in Dictionary

↓

Push Current Number

↓

Wait for its own answer
```

---

# Pattern Learned

**Monotonic Decreasing Stack**

Recognition Rule

> If a new element can determine the answer for one or more previous elements, consider using a Monotonic Stack.
