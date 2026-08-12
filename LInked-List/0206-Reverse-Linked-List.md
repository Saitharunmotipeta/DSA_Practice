# LeetCode 206 - Reverse Linked List

**Pattern:** Linked List + Pointer Manipulation

---

## Problem

Given the head of a singly linked list, reverse the list and return the reversed list.

### Test Case 1

```text
Input:
1 → 2 → 3 → 4 → 5 → null

Output:
5 → 4 → 3 → 2 → 1 → null
```

### Test Case 2

```text
Input:
1 → 2 → null

Output:
2 → 1 → null
```

---

## Approach

Use two pointers:

- `current` → node currently being processed
- `prev` → previous node in the reversed portion

For every node:

1. Save the next node before changing the current node's pointer.
2. Reverse `current.next` so it points to `prev`.
3. Move `prev` to `current`.
4. Move `current` to the saved `next` node.

Repeat until `current` becomes `null`.

At the end, `prev` points to the new head of the reversed list.

---

## Interview Explanation

I use two pointers, `prev` and `current`, to reverse the linked list in place.

Before changing `current.next`, I save the next node because changing the pointer would otherwise lose access to the remaining list.

Then I reverse the current pointer, move `prev` forward, and move `current` forward.

When the traversal finishes, `current` is `null` and `prev` points to the new head, so I return `prev`.

---

## Success Solution

```csharp
public class Solution {
    public ListNode ReverseList(ListNode head) {

        ListNode current = head;
        ListNode prev = null;

        while(current != null){

            ListNode next = current.next;

            current.next = prev;

            prev = current;
            current = next;
        }

        return prev;
    }
}
```

---

## Success Template

```text
prev = null
current = head

while current != null

    next = current.next

    current.next = prev

    prev = current

    current = next

return prev
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

- `head` points to the original first node.
- `current` is the node currently being processed.
- `prev` represents the already-reversed portion.
- Always save `current.next` before changing it.
- `current.next = prev` reverses one link.
- `prev = current` moves the reversed portion forward.
- `current = next` moves to the remaining list.
- At the end:
  - `current = null`
  - `prev = new head`
- Return `prev`, **not `head`**.

### Why Do We Save `next` First?

Consider:

```text
1 → 2 → 3 → null
```

If we immediately execute:

```csharp
current.next = prev;
```

while `current` is `1`, the connection from `1` to `2` is lost.

So we first save:

```csharp
ListNode next = current.next;
```

Now we can safely reverse the pointer because we still have a reference to the remaining list.

### Pointer Movement

Each iteration follows exactly four steps:

```text
1. Save next
2. Reverse current.next
3. Move prev
4. Move current
```

```csharp
ListNode next = current.next;
current.next = prev;
prev = current;
current = next;
```

**Mental Question:** When the loop finishes, which pointer points to the new head?

**Answer:** `prev`.

---

## Pattern Learned

```text
Linked List

↓

Two Pointers

↓

Save Next

↓

Reverse Pointer

↓

Move Pointers

↓

Return New Head
```