# LeetCode 19 - Remove Nth Node From End of List

**Pattern:** Linked List + Two Pointers + Dummy Node

---

## Problem

Given the head of a singly linked list, remove the `n`th node from the end of the list and return its head.

### Test Case 1

```text
Input:

head = 1 → 2 → 3 → 4 → 5 → null
n = 2

Output:

1 → 2 → 3 → 5 → null
```

The 2nd node from the end is `4`.

---

### Test Case 2

```text
Input:

head = 1 → 2 → null
n = 1

Output:

1 → null
```

The last node (`2`) is removed.

---

### Test Case 3

```text
Input:

head = 1 → 2 → null
n = 2

Output:

2 → null
```

The head node (`1`) is removed.

---

## Approach

Use two pointers:

- `fast`
- `slow`

Also create a **dummy node** before the head.

```text
dummy → 1 → 2 → 3 → 4 → 5 → null
  ↑
slow
fast
```

The dummy node is important because the node that needs to be removed could be the original head.

### Step 1 — Create Dummy Node

```csharp
ListNode dummy = new ListNode();

dummy.next = head;
```

Then:

```csharp
ListNode fast = dummy;
ListNode slow = dummy;
```

---

### Step 2 — Move Fast `n` Steps

Move `fast` forward exactly `n` times.

This creates a gap of `n` nodes between `fast` and `slow`.

---

### Step 3 — Move Both Pointers

Move both pointers one step at a time until:

```text
fast.next == null
```

At this point, `slow` is positioned immediately before the node that needs to be removed.

---

### Step 4 — Delete the Node

The node to remove is:

```text
slow.next
```

Skip it using:

```csharp
slow.next = slow.next.next;
```

---

### Step 5 — Return the Result

Return:

```csharp
dummy.next
```

The dummy node itself is not part of the actual linked list.

---

## Interview Explanation

I use a dummy node and two pointers to maintain a fixed gap of `n` nodes.

Both `fast` and `slow` start at the dummy node. I first move `fast` `n` steps ahead.

Then I move both pointers together until `fast.next` becomes `null`.

Because the pointers maintain a gap of `n`, `slow` will be positioned immediately before the node that needs to be removed.

I remove that node by setting:

```csharp
slow.next = slow.next.next;
```

Finally, I return `dummy.next`.

The dummy node handles the edge case where the node being removed is the original head.

---

## Success Solution

```csharp
public class Solution {

    public ListNode RemoveNthFromEnd(ListNode head, int n) {

        ListNode dummy = new ListNode();

        dummy.next = head;

        ListNode fast = dummy;
        ListNode slow = dummy;

        int i = 0;

        while(i < n){
            fast = fast.next;
            i++;
        }

        while(fast.next != null){
            slow = slow.next;
            fast = fast.next;
        }

        slow.next = slow.next.next;

        return dummy.next;
    }
}
```

---

## Success Template

```text
Create dummy

dummy.next = head

fast = dummy
slow = dummy

Move fast n steps

While fast.next is not null

    Move slow one step
    Move fast one step

slow.next = slow.next.next

Return dummy.next
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

The list is traversed using only two pointers and a dummy node.

---

## Revision Notes

### Why Use a Dummy Node?

Consider:

```text
1 → 2 → 3
```

If `n = 3`, we need to remove `1`.

There is no node before `1`.

The dummy node gives us:

```text
dummy → 1 → 2 → 3
```

Now `slow` can point to `dummy`, allowing:

```csharp
slow.next = slow.next.next;
```

to remove the original head.

---

### Fixed Gap Between Pointers

After moving `fast` `n` steps:

```text
slow

↓

dummy → 1 → 2 → 3 → 4 → 5

                    ↑
                   fast
```

The gap between them remains constant while both move together.

When `fast` reaches the end:

```text
dummy → 1 → 2 → 3 → 4 → 5 → null
              ↑
             slow
```

The target is:

```text
slow.next
```

So:

```csharp
slow.next = slow.next.next;
```

removes it.

---

### Why `fast.next != null`?

We want `slow` to stop at the node **before** the target.

Using:

```csharp
while(fast.next != null)
```

ensures that when the loop finishes:

```text
fast → last node
```

and:

```text
slow → node before target
```

---

### Why Return `dummy.next`?

The dummy node is only a placeholder:

```text
dummy → 1 → 2 → 3
```

The actual linked list begins at:

```text
dummy.next
```

Therefore:

```csharp
return dummy.next;
```

---

### Important Pointer Operation

Deleting a node in a singly linked list:

```csharp
slow.next = slow.next.next;
```

means:

```text
Before:

slow → target → next


After:

slow ─────────→ next
```

The target node is removed from the chain.

---

## Pattern Learned

```text
Linked List

↓

Dummy Node

↓

Two Pointers

↓

Create n-node Gap

↓

Move Both Together

↓

slow = Node Before Target

↓

slow.next = slow.next.next

↓

return dummy.next
```

---

## Key Takeaway

The core trick is:

> **To remove the nth node from the end, keep `fast` exactly `n` steps ahead of `slow`.**

When `fast` reaches the end, `slow` is exactly where we need it.

This turns what could require finding the list length first into a **single-pass O(n) solution**.