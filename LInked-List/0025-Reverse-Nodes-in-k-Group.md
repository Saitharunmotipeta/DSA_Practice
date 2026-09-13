# LeetCode 25 — Reverse Nodes in k-Group

## Pattern

**Linked List — Grouped Reversal / Pointer Manipulation**

The core operation is the same reversal pattern used in **Reverse Linked List** and **Reverse Linked List II**, but now we apply it repeatedly to groups of `k` nodes.

---

## Problem

Given the head of a linked list and an integer `k`, reverse the nodes of the list `k` at a time.

* Reverse every complete group of `k` nodes.
* If fewer than `k` nodes remain at the end, leave them unchanged.
* Do not change the values inside the nodes; only change the pointers.

### Example

```text
Input:
1 → 2 → 3 → 4 → 5
k = 2

Output:
2 → 1 → 4 → 3 → 5
```

Another example:

```text
Input:
1 → 2 → 3 → 4 → 5 → 6 → 7
k = 3

Output:
3 → 2 → 1 → 6 → 5 → 4 → 7
```

---

## Test Cases

### Test Case 1

```text
Input:
1 → 2 → 3 → 4 → 5
k = 2

Output:
2 → 1 → 4 → 3 → 5
```

### Test Case 2

```text
Input:
1 → 2 → 3 → 4 → 5
k = 3

Output:
3 → 2 → 1 → 4 → 5
```

### Test Case 3

```text
Input:
1 → 2 → 3 → 4 → 5 → 6
k = 3

Output:
3 → 2 → 1 → 6 → 5 → 4
```

### Test Case 4

```text
Input:
1 → 2 → 3
k = 4

Output:
1 → 2 → 3
```

### Test Case 5

```text
Input:
1 → 2 → 3
k = 1

Output:
1 → 2 → 3
```

---

# Brute Force

One possible brute-force idea is to:

1. Copy the linked list values into an array.
2. Reverse every group of `k` values.
3. Write the values back into the linked list.

This works logically but violates the spirit of the problem because we are supposed to manipulate the linked-list structure rather than simply rearranging values.

### Complexity

```text
Time:  O(n)
Space: O(n)
```

The goal is to achieve **O(1) extra space** by manipulating pointers directly.

---

# Optimized Approach

The important realization is:

> We already know how to reverse a linked list.

The only new problem is doing it **group by group** while reconnecting the groups.

For every group we track:

```text
beforehead → grouphead → ... → kth → nexthead
```

Where:

* `beforehead` = node immediately before the current group
* `grouphead` = first node of the current group
* `kth` = last node of the current group
* `nexthead` = node immediately after the current group

---

## Step 1 — Find the kth Node

Start from `grouphead` and move `k - 1` times.

```text
grouphead
   ↓
   1 → 2 → 3 → 4 → 5
             ↑
            kth
```

If `kth == null`, there aren't enough nodes for a complete group.

Therefore:

```csharp
if(kth == null){
    break;
}
```

The remaining nodes stay unchanged.

---

## Step 2 — Save the Next Group

Before reversing, save:

```csharp
ListNode nexthead = kth.next;
```

For:

```text
1 → 2 → 3 → 4 → 5
```

with `k = 3`:

```text
1 → 2 → 3 | 4 → 5
          ↑
       nexthead
```

This is important because reversal will change the pointers inside the current group.

---

## Step 3 — Reverse the Current Group

Use the same reversal pattern we already learned:

```text
SAVE → REVERSE → MOVE
```

```csharp
ListNode next = curr.next;
curr.next = prev;
prev = curr;
curr = next;
```

The difference is that we initialize:

```csharp
ListNode prev = nexthead;
```

instead of `null`.

That means when the reversal finishes, the old group head automatically points to the next group.

Example:

```text
Before:

1 → 2 → 3 → 4 → 5
          ↑
       nexthead = 4
```

After reversal:

```text
3 → 2 → 1 → 4 → 5
          ↑
       grouphead
```

The original `1` has become the tail of the reversed group.

---

## Step 4 — Connect the Previous Group

There are two cases.

### First Group

There is no `beforehead`.

Therefore, `kth` becomes the new head:

```csharp
newhead = kth;
```

Example:

```text
3 → 2 → 1 → 4 → 5
↑
newhead
```

### Later Groups

Now `beforehead` exists.

Connect it to the new head of the reversed group:

```csharp
beforehead.next = kth;
```

Example:

```text
3 → 2 → 1 → 6 → 5 → 4
          ↑
      beforehead
```

---

## Step 5 — Move to the Next Group

After reversal:

```text
kth → ... → grouphead → nexthead
```

The original `grouphead` is now the **tail** of the reversed group.

Therefore:

```csharp
beforehead = grouphead;
grouphead = nexthead;
```

Example:

```text
3 → 2 → 1 | 6 → 5 → 4
          ↑
      beforehead
```

Now we process:

```text
6 → 5 → 4
```

---

# Mental Model

Remember the algorithm as:

```text
FIND → SAVE → REVERSE → CONNECT → MOVE
```

### FIND

Find the `kth` node.

```csharp
ListNode kth = grouphead;
```

### SAVE

Save the node after the group.

```csharp
ListNode nexthead = kth.next;
```

### REVERSE

Use the standard linked-list reversal pattern.

```csharp
ListNode next = curr.next;
curr.next = prev;
prev = curr;
curr = next;
```

### CONNECT

Either:

```csharp
newhead = kth;
```

or:

```csharp
beforehead.next = kth;
```

### MOVE

```csharp
beforehead = grouphead;
grouphead = nexthead;
```

---

# Interview Explanation

> "I process the linked list one group of `k` nodes at a time. First I find the kth node to make sure a complete group exists. If it doesn't, I leave the remaining nodes unchanged. Before reversing, I save the node after the group. Then I reverse the group using the standard three-pointer linked-list reversal technique, starting `prev` at the node after the group so the old group head automatically connects to the next group. Finally, I connect the previous group's tail to the new head of the reversed group and move to the next group."

The key pointer relationship is:

```text
beforehead → grouphead → ... → kth → nexthead
```

After reversal:

```text
beforehead → kth → ... → grouphead → nexthead
```

---

# Success Solution

```csharp
public class Solution {
    public ListNode ReverseKGroup(ListNode head, int k) {
        ListNode beforehead = null;
        ListNode newhead = head;
        ListNode grouphead = head;

        while(grouphead != null){
            ListNode kth = grouphead;
            int c = 1;

            // Find kth node
            while(c < k && kth != null){
                kth = kth.next;
                c++;
            }

            // Fewer than k nodes remain
            if(kth == null){
                break;
            }

            // Save node after current group
            ListNode nexthead = kth.next;

            // Reverse current group
            ListNode prev = nexthead;
            ListNode curr = grouphead;

            while(curr != nexthead){
                ListNode next = curr.next;
                curr.next = prev;
                prev = curr;
                curr = next;
            }

            // Connect reversed group to previous part
            if(beforehead != null){
                beforehead.next = kth;
            }
            else{
                newhead = kth;
            }

            // Move to next group
            beforehead = grouphead;
            grouphead = nexthead;
        }

        return newhead;
    }
}
```

---

# Success Template

```text
beforehead = node before current group
grouphead  = first node of current group

while(grouphead != null):

    FIND kth node

    if fewer than k nodes:
        stop

    SAVE nexthead = kth.next

    REVERSE:
        prev = nexthead
        curr = grouphead

        while curr != nexthead:
            save next
            reverse pointer
            move prev
            move curr

    CONNECT:
        if first group:
            newhead = kth
        else:
            beforehead.next = kth

    MOVE:
        beforehead = grouphead
        grouphead = nexthead
```

---

# Complexity

Let `n` be the number of nodes.

### Time

```text
O(n)
```

Every node is visited a constant number of times.

### Space

```text
O(1)
```

Only a constant number of pointers are used.

---

# Revision Notes

### 1. Don't reverse before checking `k`

Always determine whether a complete group exists first.

```csharp
if(kth == null)
    break;
```

---

### 2. `grouphead` changes meaning after reversal

Before reversal:

```text
grouphead → 2 → 3
```

After reversal:

```text
3 → 2 → grouphead
             ↑
             1
```

The original `grouphead` becomes the **tail**.

That's why:

```csharp
beforehead = grouphead;
```

works.

---

### 3. `kth` becomes the new group head

After reversing:

```text
1 → 2 → 3
```

becomes:

```text
3 → 2 → 1
↑       ↑
kth     grouphead
```

So:

```csharp
beforehead.next = kth;
```

---

### 4. Why `prev = nexthead`?

Instead of:

```csharp
prev = null;
```

we use:

```csharp
prev = nexthead;
```

This makes the final reversal automatically produce:

```text
kth → ... → grouphead → nexthead
```

So we don't need a separate:

```csharp
grouphead.next = nexthead;
```

afterwards.

---

### Remember It Like This

**Reverse Linked List:**

```text
SAVE → REVERSE → MOVE
```

**Reverse in k-Group:**

```text
FIND → SAVE → REVERSE → CONNECT → MOVE
```

That's the evolution of the linked-list pointer pattern.
