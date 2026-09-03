# LeetCode 92 - Reverse Linked List II

**Pattern:** Linked List + Partial Reversal + Pointer Reconnection

---

## Problem

Given the head of a singly linked list and two integers `left` and `right`, reverse the nodes from position `left` to position `right`.

The rest of the linked list must remain unchanged.

### Example

```text
Input:

1 → 2 → 3 → 4 → 5

left = 2
right = 4
```

Reverse:

```text
2 → 3 → 4
```

Result:

```text
1 → 4 → 3 → 2 → 5
```

---

## Approach

We already know how to reverse a complete linked list.

This problem uses the same reversal pattern, but only for a specific portion.

```text
Find left
   ↓
Save important nodes
   ↓
Reverse left → right
   ↓
Reconnect both sides
```

The important nodes are:

```text
beforeLeft
leftNode
```

### Why save `beforeLeft`?

It is the node immediately before the section we want to reverse.

Example:

```text
1 → 2 → 3 → 4 → 5
↑   ↑
beforeLeft
    leftNode
```

After reversal:

```text
1 → 4 → 3 → 2 → 5
```

So `beforeLeft.next` must point to the new beginning of the reversed section.

---

### Why save `leftNode`?

`leftNode` is the original first node of the section.

After reversal, it becomes the **last node of the reversed section**.

Before:

```text
2 → 3 → 4 → 5
↑
leftNode
```

After:

```text
4 → 3 → 2 → 5
        ↑
     leftNode
```

Therefore:

```text
leftNode.next = node after right
```

---

## Step 1 — Find `left`

Start with:

```csharp
ListNode current = head;
ListNode prev = null;
int c = 1;
```

Move until `current` reaches position `left`:

```csharp
while(c != left) {
    prev = current;
    current = current.next;
    c++;
}
```

After this:

```text
prev    → node before left
current → node at left
```

---

## Step 2 — Save Important Nodes

```csharp
ListNode beforeLeft = prev;
ListNode leftNode = current;
```

We need both because they are required when reconnecting the list after reversal.

---

## Step 3 — Reverse From `left` to `right`

Use the standard linked-list reversal pattern:

```csharp
while(c <= right) {
    ListNode next = current.next;

    current.next = prev;
    prev = current;
    current = next;

    c++;
}
```

The important pattern is:

```text
SAVE
 ↓
REVERSE
 ↓
MOVE prev
 ↓
MOVE current
```

Specifically:

```csharp
ListNode next = current.next;
current.next = prev;
prev = current;
current = next;
```

We must save `current.next` before changing it.

Otherwise, after:

```csharp
current.next = prev;
```

the original forward connection is lost.

---

## Step 4 — Reconnect the Left Side

After reversal:

```text
prev → new head of reversed section
```

So:

```csharp
beforeLeft.next = prev;
```

Example:

```text
1 → 2 → 3 → 4 → 5
```

After reversing `2 → 4`:

```text
1       4 → 3 → 2       5
↑       ↑
before  prev
```

Reconnect:

```text
1.next = 4
```

giving:

```text
1 → 4 → 3 → 2
```

---

## Step 5 — Reconnect the Right Side

After reversal:

```text
current → node immediately after right
```

And:

```text
leftNode
```

is now the last node of the reversed section.

Therefore:

```csharp
leftNode.next = current;
```

Example:

```text
4 → 3 → 2       5
        ↑       ↑
     leftNode  current
```

Reconnect:

```text
2 → 5
```

Final:

```text
1 → 4 → 3 → 2 → 5
```

---

## Special Case — `left == 1`

If the reversal starts from the first node:

```text
1 → 2 → 3 → 4 → 5
↑
left
```

then:

```csharp
beforeLeft = null;
```

There is no node before `left`.

Therefore:

```csharp
if(beforeLeft != null)
    beforeLeft.next = prev;
else
    head = prev;
```

If `left == 1`, the new head is `prev`.

---

## Success Solution

```csharp
public class Solution {
    public ListNode ReverseBetween(ListNode head, int left, int right) {

        ListNode current = head;
        ListNode prev = null;

        int c = 1;

        // Find the node at position left
        while(c != left) {
            prev = current;
            current = current.next;
            c++;
        }

        // Save important nodes
        ListNode beforeLeft = prev;
        ListNode leftNode = current;

        // Reverse left → right
        while(c <= right) {
            ListNode next = current.next;

            current.next = prev;
            prev = current;
            current = next;

            c++;
        }

        // Connect the node before left
        if(beforeLeft != null)
            beforeLeft.next = prev;
        else
            head = prev;

        // Connect the end of reversed section
        leftNode.next = current;

        return head;
    }
}
```

---

## Complete Example

Given:

```text
1 → 2 → 3 → 4 → 5

left = 2
right = 4
```

### Before Reversal

```text
beforeLeft
    ↓
    1 → 2 → 3 → 4 → 5
        ↑           ↑
     leftNode      right
```

### During Reversal

Reverse:

```text
2 → 3 → 4
```

using:

```csharp
next = current.next;
current.next = prev;
prev = current;
current = next;
```

Eventually:

```text
4 → 3 → 2

prev → 4
        ↓
       3 → 2

current → 5
```

### Reconnect

First:

```csharp
beforeLeft.next = prev;
```

```text
1 → 4 → 3 → 2
```

Then:

```csharp
leftNode.next = current;
```

```text
1 → 4 → 3 → 2 → 5
```

---

## Pointer State After Reversal

This is the most important picture to remember:

```text
1       4 → 3 → 2       5
↑       ↑       ↑       ↑
before  prev   leftNode current
```

Therefore:

```csharp
beforeLeft.next = prev;
leftNode.next = current;
```

That's the entire reconnection logic.

---

## Complexity

**Time Complexity:** `O(n)`

We traverse the list to reach `left` and then reverse the required portion.

```text
O(n)
```

**Space Complexity:** `O(1)`

Only a constant number of pointers are used.

```text
current
prev
next
beforeLeft
leftNode
```

---

## Revision Notes

### Partial Reversal Pattern

```text
Find left
    ↓
Save beforeLeft
    ↓
Save leftNode
    ↓
Reverse left → right
    ↓
prev = new beginning
current = node after right
    ↓
beforeLeft.next = prev
leftNode.next = current
```

---

### Standard Reversal Pattern

The actual reversal is exactly what we learned in LeetCode 206:

```csharp
ListNode next = current.next;
current.next = prev;
prev = current;
current = next;
```

Remember:

```text
SAVE → REVERSE → MOVE → MOVE
```

---

### The Key Trick

You cannot move backwards in a singly linked list.

So we don't try to move `right` backwards.

Instead:

```text
Move forward
    +
Reverse links while moving
```

This is how we reverse:

```text
2 → 3 → 4
```

into:

```text
4 → 3 → 2
```

---

## Pattern Learned

```text
Reverse Linked List II

        ↓

Find the section
        ↓
Save beforeLeft + leftNode
        ↓
Reverse the section
        ↓
Reconnect both sides
```

The deeper lesson:

> **When modifying only part of a linked list, always think about the connections entering and leaving the modified section.**
