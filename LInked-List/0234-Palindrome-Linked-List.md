# LeetCode 234 - Palindrome Linked List

**Pattern:** Fast & Slow Pointers + Reverse Linked List

---

## Problem

Given the head of a singly linked list, determine whether the linked list is a palindrome.

A palindrome reads the same forward and backward.

### Test Case 1

```text
Input:

1 → 2 → 2 → 1 → null

Output:

true
```

### Test Case 2

```text
Input:

1 → 2 → 3 → null

Output:

false
```

### Test Case 3

```text
Input:

1 → 2 → 3 → 2 → 1 → null

Output:

true
```

---

## Approach

The solution combines two previously learned linked-list patterns:

```text
Fast & Slow Pointers
        ↓
Find the middle
        ↓
Reverse the second half
        ↓
Compare both halves
```

### Step 1 — Find the Middle

Use two pointers:

```text
slow → moves 1 node
fast → moves 2 nodes
```

Start both at `head`.

```csharp
ListNode fast = head;
ListNode slow = head;
```

Move them until `fast` reaches the end:

```csharp
while(fast != null && fast.next != null)
{
    slow = slow.next;
    fast = fast.next.next;
}
```

When the loop finishes, `slow` is at the middle / beginning of the second portion of the list.

---

### Step 2 — Reverse the Second Half

Use the same linked-list reversal technique learned in LeetCode 206.

Start the reversal from `slow`:

```csharp
ListNode prev = null;
ListNode current = slow;
```

Then:

```csharp
while(current != null)
{
    ListNode next = current.next;
    current.next = prev;
    prev = current;
    current = next;
}
```

After reversal, `prev` becomes the head of the reversed second half.

---

### Step 3 — Compare Both Halves

Create two pointers:

```csharp
ListNode left = head;
ListNode right = prev;
```

Then compare values:

```csharp
while(right != null)
{
    if(left.val != right.val)
        return false;

    left = left.next;
    right = right.next;
}
```

If every corresponding value matches, the list is a palindrome.

Return:

```csharp
true
```

---

## Interview Explanation

I solve the problem in three steps.

First, I use Fast & Slow Pointers to find the middle of the linked list.

Then I reverse the second half of the linked list using the standard linked-list reversal technique.

Finally, I compare the first half with the reversed second half node by node.

If any pair of values is different, the list is not a palindrome and I return `false`.

If all corresponding values match, I return `true`.

This gives an `O(n)` time and `O(1)` extra-space solution.

---

## Success Solution

```csharp
public class Solution {
    public bool IsPalindrome(ListNode head) {

        ListNode fast = head;
        ListNode slow = head;

        // Find the middle
        while(fast != null && fast.next != null)
        {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Reverse the second half
        ListNode prev = null;
        ListNode current = slow;

        while(current != null)
        {
            ListNode next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }

        // Compare first half with reversed second half
        ListNode left = head;
        ListNode right = prev;

        while(right != null)
        {
            if(left.val != right.val)
                return false;

            left = left.next;
            right = right.next;
        }

        return true;
    }
}
```

---

## Success Template

```text
slow = head
fast = head

While fast != null
AND fast.next != null

    slow = slow.next
    fast = fast.next.next


prev = null
current = slow

While current != null

    next = current.next
    current.next = prev
    prev = current
    current = next


left = head
right = prev

While right != null

    If left.val != right.val

        return false

    left = left.next
    right = right.next


return true
```

---

## Complexity

**Time Complexity:** `O(n)`

The list is traversed a constant number of times:

```text
Find middle  → O(n)
Reverse      → O(n)
Compare      → O(n)
```

Therefore:

```text
O(n) + O(n) + O(n) = O(n)
```

**Space Complexity:** `O(1)`

Only a constant number of pointers are used.

---

## Revision Notes

### Pattern Combination

This problem is important because it combines three techniques already learned:

```text
141 - Linked List Cycle
        ↓
Fast & Slow Pointers


206 - Reverse Linked List
        ↓
Reverse a linked list


234 - Palindrome Linked List
        ↓
Combine both patterns
```

---

### Finding the Middle

```text
slow → 1 step
fast → 2 steps
```

For:

```text
1 → 2 → 3 → 2 → 1
```

Eventually:

```text
1 → 2 → 3 → 2 → 1
        ↑
       slow
```

---

### Reversing

Start reversal from `slow`:

```text
3 → 2 → 1
```

After reversal:

```text
1 → 2 → 3
↑
prev
```

`prev` is now the head of the reversed portion.

---

### Comparing

```text
First portion:

1 → 2 → 3
↑
left


Reversed second portion:

1 → 2 → 3
↑
right
```

Compare:

```text
1 == 1  ✓
2 == 2  ✓
3 == 3  ✓
```

Therefore:

```text
true
```

---

### Why Use `prev` After Reversal?

During reversal:

```csharp
prev = current;
```

When the loop finishes:

```text
current = null
```

and:

```text
prev
```

points to the new head of the reversed list.

Therefore we use:

```csharp
ListNode right = prev;
```

not:

```csharp
ListNode right = slow;
```

because `slow` still references the original node where reversal started.

---

### Why Compare Until `right != null`?

The reversed second half is the portion we need to compare.

Therefore:

```csharp
while(right != null)
```

is enough.

We don't need to continue after the second half has been completely compared.

---

### Important Reversal Pattern

The reversal itself is exactly the same pattern from LeetCode 206:

```csharp
ListNode next = current.next;
current.next = prev;
prev = current;
current = next;
```

Remember:

> **Save → Reverse → Move `prev` → Move `current`**

```text
Save next
   ↓
Reverse current.next
   ↓
Move prev
   ↓
Move current
```

---

### Important Edge Cases

#### One Node

```text
1 → null
```

A single node is always a palindrome.

---

#### Two Nodes

```text
1 → 1
```

Palindrome:

```text
true
```

But:

```text
1 → 2
```

is not:

```text
false
```

---

#### Odd Length

```text
1 → 2 → 3 → 2 → 1
```

The middle element does not affect whether the list is a palindrome.

---

#### Even Length

```text
1 → 2 → 2 → 1
```

The two halves can be compared directly.

---

## Pattern Learned

```text
Palindrome Linked List

        ↓

Fast & Slow Pointers
        ↓
Find Middle
        ↓
Reverse Second Half
        ↓
Compare
        ↓
Palindrome?
```

---

## Key Takeaway

The important lesson isn't just how to solve 234.

It's recognizing that a new problem can be built from patterns we already know:

```text
Find Middle
    +
Reverse Linked List
    +
Two-Pointer Comparison
    =
Palindrome Linked List
```

**Mental Question:**

> Can I split the linked list into two parts, transform one part, and compare them?

If yes, this pattern may be useful.

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
```

---

## Next Problem

**LeetCode 143 - Reorder List**

**Pattern:**

```text
Find Middle
    ↓
Reverse Second Half
    ↓
Merge Two Lists Alternately
```

This is the perfect next problem because **234 just taught us the first two-thirds of 143**. Now we'll learn how to actually **merge the two halves back together**.