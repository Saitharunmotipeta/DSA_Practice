# LeetCode 141 - Linked List Cycle

**Pattern:** Linked List + Fast & Slow Pointers (Floyd's Cycle Detection)

---

## Problem

Given the head of a singly linked list, determine whether the linked list contains a cycle.

A cycle exists when some node's `next` pointer points back to a previous node in the list.

Return `true` if a cycle exists, otherwise return `false`.

### Test Case 1

```text
Input:

3 → 2 → 0 → -4
    ↑         |
    └─────────┘

Output:

true
```

The last node points back to the node containing `2`.

---

### Test Case 2

```text
Input:

1 → 2
    ↑
    └───

Output:

true
```

The second node points back to itself.

---

### Test Case 3

```text
Input:

1 → 2 → null

Output:

false
```

There is no cycle because the list eventually reaches `null`.

---

## Approach

### Approach 1 — HashSet

A straightforward solution is to store every node that has already been visited.

During traversal:

- If the current node already exists in the `HashSet`, a cycle exists.
- Otherwise, add the current node and continue.
- If `current` becomes `null`, there is no cycle.

Time Complexity:

**O(n)**

Space Complexity:

**O(n)**

---

### Optimized Approach — Fast & Slow Pointers

Use two pointers:

```text
slow → moves 1 node at a time

fast → moves 2 nodes at a time
```

Start both pointers at `head`.

```csharp
ListNode slow = head;
ListNode fast = head;
```

During each iteration:

```csharp
slow = slow.next;
fast = fast.next.next;
```

Then compare:

```csharp
if(slow == fast)
    return true;
```

If there is a cycle, the faster pointer eventually catches the slower pointer.

If there is no cycle, `fast` eventually reaches `null`.

---

## Interview Explanation

I use Floyd's Fast & Slow Pointer algorithm.

I initialize both `slow` and `fast` at the head of the linked list.

The slow pointer moves one node at a time while the fast pointer moves two nodes at a time.

If there is no cycle, the fast pointer eventually reaches `null`.

If there is a cycle, both pointers eventually enter the cycle. Since the fast pointer moves faster, it will eventually catch the slow pointer, meaning both references point to the same node.

Therefore, if `slow == fast`, a cycle exists.

I use reference equality rather than comparing node values because different nodes can contain the same value.

---

## Success Solution

```csharp
public class Solution {
    public bool HasCycle(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null){

            slow = slow.next;
            fast = fast.next.next;

            if(slow == fast){
                return true;
            }
        }

        return false;
    }
}
```

---

## Success Template

```text
slow = head
fast = head

while fast is not null
AND fast.next is not null

    slow = slow.next

    fast = fast.next.next

    if slow == fast

        return true

return false
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

Only two references are maintained regardless of the size of the linked list.

---

## Revision Notes

### Fast & Slow Pointer

```text
slow → 1 step

fast → 2 steps
```

The two pointers start at the same node.

```text
slow
fast
 ↓
1 → 2 → 3 → 4 → null
```

For every iteration:

```csharp
slow = slow.next;
fast = fast.next.next;
```

---

### Why Does This Detect a Cycle?

Without a cycle:

```text
1 → 2 → 3 → 4 → null
```

`fast` eventually reaches:

```text
null
```

Therefore:

```csharp
fast != null && fast.next != null
```

becomes false.

With a cycle:

```text
1 → 2 → 3 → 4
    ↑       |
    └───────┘
```

Both pointers eventually enter the cycle.

Because `fast` moves two steps for every one step of `slow`, it eventually catches `slow`.

```text
slow == fast
```

Therefore a cycle is detected.

---

### Why `fast != null && fast.next != null`?

The fast pointer moves two nodes:

```csharp
fast = fast.next.next;
```

Therefore we must make sure:

```text
fast
```

exists and:

```text
fast.next
```

also exists.

Otherwise, attempting to access `fast.next.next` could cause a null reference error.

---

### Why `slow == fast` Instead of `slow.val == fast.val`?

Cycle detection is about whether both pointers reference the **same node**.

Two different nodes can contain the same value.

For example:

```text
Node A
value = 5

Node B
value = 5
```

These are different nodes:

```text
A != B
```

even though:

```text
A.val == B.val
```

Therefore we compare:

```csharp
slow == fast
```

---

### HashSet vs Fast & Slow

HashSet:

```text
Time:  O(n)
Space: O(n)
```

Fast & Slow:

```text
Time:  O(n)
Space: O(1)
```

Fast & Slow is the optimal solution when constant extra space is required.

---

### Important Insight

We don't need to remember every node.

Instead:

```text
Fast pointer
      ↓
moves faster
      ↓
enters the cycle
      ↓
eventually catches slow
      ↓
slow == fast
```

**Mental Question:** If there is no cycle, what eventually stops the loop?

**Answer:** `fast` or `fast.next` becomes `null`.

**Mental Question:** If there is a cycle, what eventually stops the loop?

**Answer:** `slow == fast`.

---

## Pattern Learned

```text
Linked List

↓

Two Pointers

↓

slow = 1 step

fast = 2 steps

↓

No Cycle
fast reaches null

OR

Cycle
slow meets fast
```

---

## Key Takeaway

The core pattern is:

```csharp
ListNode slow = head;
ListNode fast = head;

while(fast != null && fast.next != null)
{
    slow = slow.next;
    fast = fast.next.next;

    if(slow == fast)
        return true;
}

return false;
```

This is **Floyd's Cycle Detection Algorithm** and is one of the most important fast-and-slow pointer patterns in linked-list problems.