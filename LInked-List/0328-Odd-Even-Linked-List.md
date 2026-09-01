# LeetCode 328 - Odd Even Linked List

**Pattern:** Two Pointers + Linked List Rewiring

---

## Problem

Given the head of a singly linked list, group all nodes at odd positions together followed by the nodes at even positions.

The relative order inside both groups must be maintained.

### Example

```text
Input:

1 → 2 → 3 → 4 → 5

Output:

1 → 3 → 5 → 2 → 4
```

> Odd and even refer to the **position of the node**, not its value.

---

## Approach

Maintain two chains:

```text
Odd:

1 → 3 → 5


Even:

2 → 4
```

We use three pointers:

```text
odd      → current odd-position node
even     → current even-position node
evenHead → beginning of the even chain
```

At the end:

```text
odd.next = evenHead
```

Overall:

```text
Build odd chain
      ↓
Build even chain
      ↓
Connect them
```

---

## Step 1 — Initialize Pointers

```csharp
ListNode odd = head;
ListNode even = head.next;
ListNode evenhead = head.next;
```

For:

```text
1 → 2 → 3 → 4 → 5
↑   ↑
odd even
```

`evenhead` remembers the original beginning of the even chain.

Why?

Because `even` will keep moving, but at the end we need to connect the odd chain to the **beginning** of the even chain.

---

## Step 2 — Connect Odd Nodes

```csharp
odd.next = even.next;
```

This skips the current even node and connects the current odd node to the next odd node.

```text
1 → 2 → 3
```

becomes:

```text
1 → 3
```

Then move:

```csharp
odd = odd.next;
```

Now:

```text
odd → 3
```

---

## Step 3 — Connect Even Nodes

Now:

```csharp
even.next = odd.next;
```

This connects the current even node to the next even node.

```text
2 → 3 → 4
```

becomes:

```text
2 → 4
```

Then:

```csharp
even = even.next;
```

Now:

```text
even → 4
```

---

## Step 4 — Repeat

The core loop is:

```csharp
while(even != null && even.next != null){

    odd.next = even.next;
    odd = odd.next;

    even.next = odd.next;
    even = even.next;
}
```

Think:

```text
CONNECT ODD
    ↓
MOVE ODD
    ↓
CONNECT EVEN
    ↓
MOVE EVEN
```

---

## Step 5 — Connect Both Chains

Eventually:

```text
Odd:

1 → 3 → 5


Even:

2 → 4
```

We saved the beginning of the even chain in:

```csharp
evenhead
```

So:

```csharp
odd.next = evenhead;
```

produces:

```text
1 → 3 → 5 → 2 → 4
```

Finally:

```csharp
return head;
```

---

## Interview Explanation

I maintain two pointers, `odd` and `even`, representing the current odd-position and even-position nodes.

I also save `evenHead` because the `even` pointer moves throughout the algorithm, while I need the original beginning of the even chain at the end.

For every iteration, I connect the current odd node to the next odd node, move `odd`, then connect the current even node to the next even node and move `even`.

Finally, I connect the end of the odd chain to `evenHead`.

This modifies the linked list in-place with `O(n)` time and `O(1)` extra space.

---

## Success Solution

```csharp
public class Solution {

    public ListNode OddEvenList(ListNode head) {

        if(head == null || head.next == null)
            return head;

        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenhead = head.next;

        while(even != null && even.next != null){

            odd.next = even.next;
            odd = odd.next;

            even.next = odd.next;
            even = even.next;
        }

        odd.next = evenhead;

        return head;
    }
}
```

---

## Success Template

```text
if head == null
OR head.next == null

    return head


odd = head
even = head.next
evenHead = head.next


while even != null
AND even.next != null

    odd.next = even.next
    odd = odd.next

    even.next = odd.next
    even = even.next


odd.next = evenHead

return head
```

---

## Complexity

**Time Complexity:** `O(n)`

Every node is processed a constant number of times.

```text
O(n)
```

**Space Complexity:** `O(1)`

Only a constant number of pointers are used.

```text
O(1)
```

---

## Revision Notes

### Why Do We Need `evenHead`?

This is the most important pointer in this problem.

```csharp
ListNode even = head.next;
ListNode evenhead = head.next;
```

They initially point to the same node.

But their purposes are different.

```text
even
 ↓
Moving pointer


evenHead
 ↓
Permanent pointer to beginning of even chain
```

During the algorithm:

```text
even → 2 → 4 → null
        ↑
      moves
```

Eventually `even` reaches `null`.

But we still need:

```text
2 → 4
↑
evenHead
```

Therefore:

```csharp
odd.next = evenhead;
```

---

### Why Is the Loop Condition This?

```csharp
while(even != null && even.next != null)
```

We use `even` because it is the pointer moving through the even-position nodes.

We also access:

```csharp
even.next
```

so we must first make sure:

```text
even != null
```

Otherwise we could get:

```text
NullReferenceException
```

---

### We Are Changing Links, Not Values

We are NOT doing:

```text
1 2 3 4 5
↓
1 3 5 2 4
```

by moving values.

We are changing the `next` references:

```text
1.next = 3
3.next = 5
5.next = 2
2.next = 4
```

The actual nodes remain the same.

---

## Complete Example

Start:

```text
1 → 2 → 3 → 4 → 5
↑   ↑
odd even

evenHead = 2
```

### Iteration 1

```csharp
odd.next = even.next;
```

```text
1 → 3
```

Move:

```text
odd → 3
```

Then:

```csharp
even.next = odd.next;
```

```text
2 → 4
```

Move:

```text
even → 4
```

---

### Iteration 2

Current relevant links:

```text
Odd:

1 → 3 → 5


Even:

2 → 4
```

Connect:

```text
3 → 5
```

Then:

```text
4 → null
```

Now:

```text
Odd:

1 → 3 → 5


Even:

2 → 4
```

Finally:

```csharp
odd.next = evenhead;
```

Result:

```text
1 → 3 → 5 → 2 → 4
```

---

## Key Pointer Rule

Remember:

```text
ODD:

odd.next = even.next
odd = odd.next


EVEN:

even.next = odd.next
even = even.next


FINAL:

odd.next = evenHead
```

Mental model:

> **Build two chains, then connect them.**

---

## Pattern Learned

```text
Odd Even Linked List

        Head
          ↓
    ┌───────────┐
    ↓           ↓
   Odd         Even
    ↓           ↓
  1 → 3 → 5    2 → 4
    └─────┬─────┘
          ↓
       Connect
          ↓
   1 → 3 → 5 → 2 → 4
```

This is an example of **in-place linked-list pointer manipulation**.

---

## Problems Completed in Linked Lists

```text
141 - Linked List Cycle
     ↓
Fast & Slow Pointers

206 - Reverse Linked List
     ↓
Pointer Reversal

876 - Middle of the Linked List
     ↓
Fast & Slow Pointers

19 - Remove Nth Node From End
     ↓
Two Pointers + Dummy Node

234 - Palindrome Linked List
     ↓
Fast & Slow + Reverse + Compare

143 - Reorder List
     ↓
Fast & Slow + Split + Reverse + Merge

328 - Odd Even Linked List
     ↓
Two Chains + Pointer Rewiring
```
