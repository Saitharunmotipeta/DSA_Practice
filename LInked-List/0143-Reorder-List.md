# LeetCode 143 - Reorder List

**Pattern:** Fast & Slow Pointers + Split + Reverse Linked List + Merge

---

## Problem

Given the head of a singly linked list, reorder the list in the following form:

```text
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
```

The reordering must be done **in-place** without changing the values inside the nodes.

### Test Case 1

```text
Input:

1 → 2 → 3 → 4

Output:

1 → 4 → 2 → 3
```

### Test Case 2

```text
Input:

1 → 2 → 3 → 4 → 5

Output:

1 → 5 → 2 → 4 → 3
```

---

## Approach

The solution combines three linked-list patterns:

```text
Fast & Slow Pointers
        ↓
Find the end of the first half
        ↓
Split the list
        ↓
Reverse the second half
        ↓
Merge both halves alternately
```

The important idea is that we do not need to move backward through the original singly linked list.

Instead, we reverse the second half so that the nodes we need are already in the required order.

---

## Step 1 — Find the End of the First Half

Use two pointers:

```text
slow → moves 1 node
fast → moves 2 nodes
```

```csharp
ListNode slow = head;
ListNode fast = head;

while(fast.next != null && fast.next.next != null){
    slow = slow.next;
    fast = fast.next.next;
}
```

For:

```text
1 → 2 → 3 → 4
```

`slow` stops at `2`:

```text
1 → 2 | 3 → 4
    ↑
   slow
```

We deliberately stop at the **end of the first half**, not the beginning of the second half.

---

## Step 2 — Split the List

Save the beginning of the second half:

```csharp
ListNode second = slow.next;
```

Then break the connection:

```csharp
slow.next = null;
```

Now:

```text
First half:

1 → 2 → null


Second half:

3 → 4 → null
```

This is important because the two halves need to become independent before we reverse and merge them.

---

## Step 3 — Reverse the Second Half

Use the standard linked-list reversal pattern:

```csharp
ListNode prev = null;
ListNode current = second;

while(current != null){
    ListNode next = current.next;
    current.next = prev;
    prev = current;
    current = next;
}
```

Before:

```text
3 → 4 → null
```

After:

```text
4 → 3 → null
↑
prev
```

The reversal pattern is:

```text
SAVE → REVERSE → MOVE
```

```csharp
ListNode next = current.next; // SAVE
current.next = prev;          // REVERSE
prev = current;               // MOVE prev
current = next;               // MOVE current
```

---

## Step 4 — Merge the Two Halves

Now we have:

```text
First:

1 → 2


Second:

4 → 3
```

We want:

```text
1 → 4 → 2 → 3
```

Initialize:

```csharp
ListNode first = head;
second = prev;
```

Before changing any links, save the next nodes:

```csharp
ListNode firstNext = first.next;
ListNode secondNext = second.next;
```

Then connect:

```csharp
first.next = second;
second.next = firstNext;
```

Then move:

```csharp
first = firstNext;
second = secondNext;
```

The important sequence is:

```text
SAVE → CONNECT → MOVE
```

---

## Interview Explanation

I solve the problem in three stages.

First, I use Fast & Slow Pointers to find the end of the first half.

Then I split the list and reverse the second half using the standard linked-list reversal technique.

Finally, I merge the first half and the reversed second half alternately. Before modifying any links, I save the next pointers so I don't lose access to the remaining nodes.

This gives an `O(n)` time and `O(1)` extra-space solution.

---

## Success Solution

```csharp
public class Solution {
    public void ReorderList(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        // Find the end of the first half
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        // Split the list
        ListNode second = slow.next;
        slow.next = null;

        // Reverse the second half
        ListNode prev = null;
        ListNode current = second;

        while(current != null){
            ListNode next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }

        // Merge the two halves
        ListNode first = head;
        second = prev;

        while(second != null){
            ListNode firstNext = first.next;
            ListNode secondNext = second.next;

            first.next = second;
            second.next = firstNext;

            first = firstNext;
            second = secondNext;
        }
    }
}
```

---

## Success Template

```text
slow = head
fast = head

while fast.next is not null
AND fast.next.next is not null

    slow = slow.next
    fast = fast.next.next


second = slow.next
slow.next = null


prev = null
current = second

while current is not null

    next = current.next
    current.next = prev
    prev = current
    current = next


first = head
second = prev

while second is not null

    firstNext = first.next
    secondNext = second.next

    first.next = second
    second.next = firstNext

    first = firstNext
    second = secondNext
```

---

## Complexity

**Time Complexity:** `O(n)`

```text
Find middle → O(n)
Split        → O(1)
Reverse      → O(n)
Merge        → O(n)
```

Therefore:

```text
O(n)
```

**Space Complexity:** `O(1)`

Only a constant number of pointers are used.

---

## Revision Notes

### 1. Why Do We Find the End of the First Half?

For:

```text
1 → 2 → 3 → 4
```

we want:

```text
1 → 2 | 3 → 4
    ↑
   slow
```

The condition:

```csharp
while(fast.next != null && fast.next.next != null)
```

makes `slow` stop at `2`.

Then:

```csharp
ListNode second = slow.next;
slow.next = null;
```

creates the clean split.

---

### 2. Why Do We Reverse the Second Half?

A singly linked list only provides:

```text
node.val
node.next
```

There is no `node.prev`.

But the required order needs the last node first:

```text
1 → 4 → 2 → 3
```

So instead of trying to move backward, we reverse:

```text
3 → 4
```

into:

```text
4 → 3
```

Now both pointers can move forward.

---

### 3. Reversal Pattern

Always remember:

```text
SAVE → REVERSE → MOVE
```

```csharp
ListNode next = current.next;
current.next = prev;
prev = current;
current = next;
```

If we change `current.next` before saving it, we can lose the rest of the list.

---

### 4. Merge Pattern

For:

```text
First:

1 → 2 → 3


Second:

5 → 4
```

we need:

```text
1 → 5 → 2 → 4 → 3
```

Each iteration follows:

```text
SAVE
 ↓
CONNECT
 ↓
MOVE
```

```csharp
ListNode firstNext = first.next;
ListNode secondNext = second.next;

first.next = second;
second.next = firstNext;

first = firstNext;
second = secondNext;
```

---

### 5. Important Mistake to Avoid

This is wrong:

```csharp
first.next = second;
second.next = first.next;
```

After:

```csharp
first.next = second;
```

we already have:

```text
first.next → second
```

Therefore:

```csharp
second.next = first.next;
```

becomes:

```text
second.next = second;
```

which creates a self-loop.

The correct version is:

```csharp
second.next = firstNext;
```

This is why we save:

```csharp
ListNode firstNext = first.next;
```

before modifying the connection.

---

### 6. Complete Example

Start:

```text
1 → 2 → 3 → 4 → 5
```

Find the first-half end:

```text
1 → 2 → 3 | 4 → 5
          ↑
         slow
```

Split:

```text
1 → 2 → 3

4 → 5
```

Reverse second half:

```text
1 → 2 → 3

5 → 4
```

Merge:

```text
1 → 5
```

then:

```text
1 → 5 → 2 → 4
```

finally:

```text
1 → 5 → 2 → 4 → 3
```

---

## Pattern Learned

```text
Reorder List

Fast + Slow
     ↓
Find first-half end
     ↓
Split
     ↓
Reverse second half
     ↓
Merge alternately
```

The main new technique is:

```text
Alternating Merge
```

where nodes from two linked lists are connected one after another.

---

## Key Takeaway

The biggest lesson from this problem is **pattern composition**.

We already knew how to:

```text
Find Middle
    +
Reverse Linked List
```

Now we combine those techniques with:

```text
Merge Two Lists Alternately
```

The core pointer rules are:

```text
Reversal:
SAVE → REVERSE → MOVE

Merge:
SAVE → CONNECT → MOVE
```

Whenever you modify a linked-list pointer, ask:

> "Am I about to overwrite a connection that I still need?"

If yes, save it first.