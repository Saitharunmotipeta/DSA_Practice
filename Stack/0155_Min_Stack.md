# 155. Min Stack

**Difficulty:** Medium

**Pattern:** Stack | Design

**LeetCode:** https://leetcode.com/problems/min-stack/

---

# Problem Statement

Design a stack that supports the following operations in constant time.

- Push
- Pop
- Top
- Get Minimum

Every operation must run in **O(1)** time.

---

# Brute Force Idea

Maintain a normal stack.

Whenever `getMin()` is called, traverse the entire stack to find the minimum element.

Example

```
Top
↓

8
2
5
1
7
```

Traverse every element.

Minimum = 1

---

# Why Brute Force Fails

Push

```
O(1)
```

Pop

```
O(1)
```

Top

```
O(1)
```

Get Minimum

```
O(n)
```

The problem explicitly requires **O(1)** for every operation.

---

# Optimal Idea

Maintain **two stacks**.

## Main Stack

Stores every value.

```
Top
↓

8
2
5
```

---

## Min Stack

Stores only the minimum values encountered so far.

Example

```
Push(5)

Min Stack

5
```

```
Push(2)

Min Stack

2
5
```

```
Push(8)

Min Stack

2
5
```

8 is not a new minimum.

Nothing changes.

---

Whenever a new value is **less than or equal to** the current minimum, push it into the Min Stack as well.

This preserves duplicate minimum values.

---

# Why Two Stacks?

Suppose we push

```
5
2
2
8
```

Main Stack

```
Top
↓

8
2
2
5
```

Min Stack

```
Top
↓

2
2
5
```

Now

```
Pop()
```

removes one `2`.

The second `2` still exists.

Since both were stored, `GetMin()` still correctly returns `2`.

Without storing duplicate minimum values, the minimum would incorrectly become `5`.

---

# Mental Model

The Main Stack stores every element.

The Min Stack stores the history of minimum values.

Whenever the current minimum disappears, simply remove it from the Min Stack.

The previous minimum is immediately available.

---

# Algorithm

## Push(x)

- Push into Main Stack.
- If Min Stack is empty
  OR
- x <= current minimum

Push x into Min Stack.

---

## Pop()

Pop from Main Stack.

If the removed value equals the current minimum,

Pop from Min Stack as well.

---

## Top()

Return Main Stack Top.

---

## GetMin()

Return Min Stack Top.

---

# Accepted Solution

```csharp
public class MinStack {

    Stack<int> stack;
    Stack<int> minstack;

    public MinStack() {
        stack = new Stack<int>();
        minstack = new Stack<int>();
    }

    public void Push(int value) {
        stack.Push(value);

        if (minstack.Count == 0 || minstack.Peek() >= value) {
            minstack.Push(value);
        }
    }

    public void Pop() {
        int res = stack.Pop();

        if (minstack.Peek() == res) {
            minstack.Pop();
        }
    }

    public int Top() {
        return stack.Peek();
    }

    public int GetMin() {
        return minstack.Peek();
    }
}
```

---

# Dry Run

Operations

```
Push(5)
Push(2)
Push(8)
Push(1)
```

Main Stack

```
Top
↓

1
8
2
5
```

Min Stack

```
Top
↓

1
2
5
```

---

Call

```
Pop()
```

Removed

```
1
```

Main Stack

```
Top
↓

8
2
5
```

Min Stack

```
Top
↓

2
5
```

Current Minimum

```
2
```

Returned instantly.

---

# Time Complexity

| Operation | Complexity |
|----------|------------|
| Push | O(1) |
| Pop | O(1) |
| Top | O(1) |
| GetMin | O(1) |

---

# Space Complexity

Worst Case

```
O(n)
```

Example

```
5
4
3
2
1
```

Every element becomes the new minimum.

Both stacks grow to size `n`.

---

# Key Learning

- Expensive computations can often be maintained incrementally.
- Use an auxiliary data structure to cache useful information.
- Preserve duplicate minimum values.
- Never recompute the minimum by traversing the stack.

---

# Interview Explanation

"I maintain two stacks.

The first stack stores every element.

The second stack stores only the minimum values encountered so far.

Whenever a value smaller than or equal to the current minimum is pushed, I also push it into the Min Stack.

During a pop operation, if the removed element equals the current minimum, I remove it from the Min Stack as well.

This ensures that the current minimum is always available at the top of the Min Stack, making every operation O(1)."

---

# Common Mistakes

❌ Using `<` instead of `<=`

This fails for duplicate minimum values.

---

❌ Forgetting to update the Min Stack during `Pop()`

The minimum becomes stale.

---

❌ Traversing the stack inside `GetMin()`

Violates the required O(1) complexity.

---

# Revision Notes

Remember

```
Push

↓

Main Stack

↓

If value <= current minimum

↓

Push into Min Stack
```

During Pop

```
Remove from Main Stack

↓

Was it the current minimum?

↓

Yes

↓

Remove from Min Stack
```

---

# Design Pattern Learned

This problem introduces a common interview design principle:

> **Maintain auxiliary state instead of recomputing expensive information.**

This idea appears in many advanced data structures and algorithms.
