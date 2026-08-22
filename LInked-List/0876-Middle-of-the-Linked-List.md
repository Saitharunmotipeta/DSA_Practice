# LeetCode 876 - Middle of the Linked List

**Pattern:** Linked List + Fast & Slow Pointers

---

## Problem

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the **second middle node**.

### Test Case 1

```text
Input:

1 → 2 → 3 → 4 → 5 → null

Output:

3 → 4 → 5 → null
```

### Test Case 2

```text
Input:

1 → 2 → 3 → 4 → 5 → 6 → null

Output:

4 → 5 → 6 → null
```

When there are two middle nodes (`3` and `4`), the second middle node (`4`) is returned.

---

## Approach

Use two pointers:

- `slow` → moves one node at a time.
- `fast` → moves two nodes at a time.

Both pointers start at `head`.

```text
slow = head
fast = head
```

During every iteration:

```csharp
slow = slow.next;
fast = fast.next.next;
```

When `fast` reaches the end of the list, `slow` will be positioned at the middle node.

For an even-length list, this initialization naturally places `slow` at the **second middle node**, which is what the problem requires.

---

## Interview Explanation

I use the Fast & Slow Pointer technique.

Both pointers start at the head. The slow pointer moves one node at a time while the fast pointer moves two nodes at a time.

Because the fast pointer moves twice as quickly, when it reaches the end of the list, the slow pointer has travelled approximately half the distance.

Therefore, `slow` points to the middle node.

For an even-length list, the fast pointer reaches the end after the slow pointer reaches the second of the two middle nodes, so returning `slow` satisfies the problem requirement.

---

## Success Solution

```csharp
public class Solution {

    public ListNode MiddleNode(ListNode head) {

        ListNode fast = head;
        ListNode slow = head;

        while(fast != null && fast.next != null){

            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
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

return slow
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

Only two node references are used.

---

## Revision Notes

### Fast & Slow Movement

```text
slow → 1 step

fast → 2 steps
```

For:

```text
1 → 2 → 3 → 4 → 5
```

Start:

```text
slow = 1
fast = 1
```

After one iteration:

```text
slow = 2
fast = 3
```

After the next iteration:

```text
slow = 3
fast = 5
```

Now:

```text
fast.next == null
```

The loop stops and:

```text
slow = 3
```

So we return `slow`.

---

### Even-Length Lists

For:

```text
1 → 2 → 3 → 4 → 5 → 6
```

The two middle nodes are:

```text
3 and 4
```

The algorithm produces:

```text
slow → 4
```

Therefore:

```csharp
return slow;
```

returns the required second middle node.

---

### Why Not `slow.next`?

`slow` itself is already positioned at the required middle node.

Returning:

```csharp
return slow;
```

is correct.

Returning:

```csharp
return slow.next;
```

would move one node too far.

---

### Why Do We Check `fast.next`?

The fast pointer moves two nodes:

```csharp
fast = fast.next.next;
```

Therefore, we need both:

```text
fast != null
```

and:

```text
fast.next != null
```

before performing the two-step movement.

This prevents accessing a `next` reference through a null node.

---

## Pattern Learned

```text
Fast & Slow Pointers

↓

slow = 1 step

fast = 2 steps

↓

fast reaches end

↓

slow is at middle
```

---

## Key Takeaway

The same Fast & Slow Pointer pattern can solve different linked-list problems:

```text
141 - Linked List Cycle
    ↓
slow and fast eventually meet

876 - Middle of Linked List
    ↓
fast reaches the end
slow is at the middle
```

The movement is the same:

```csharp
slow = slow.next;
fast = fast.next.next;
```

The **condition we care about** is what changes.