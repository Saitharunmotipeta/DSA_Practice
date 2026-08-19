# LeetCode 21 - Merge Two Sorted Lists

**Pattern:** Linked List + Two Pointers + Dummy Node

---

## Problem

You are given the heads of two sorted singly linked lists.

Merge the two lists into one sorted linked list and return the head of the merged list.

The merged list should be created by reusing the existing nodes.

### Test Case 1

```text
Input:

list1 = 1 → 3 → 5 → null
list2 = 2 → 4 → 6 → null

Output:

1 → 2 → 3 → 4 → 5 → 6 → null
```

### Test Case 2

```text
Input:

list1 = 1 → 2 → 4 → null
list2 = 1 → 3 → 4 → null

Output:

1 → 1 → 2 → 3 → 4 → 4 → null
```

---

## Approach

Use a **Dummy Node** and a `tail` pointer.

```text
dummy → null
  ↑
 tail
```

The dummy node acts as a placeholder before the actual merged list.

While both lists contain nodes:

1. Compare `list1.val` and `list2.val`.
2. Attach the smaller node to `tail.next`.
3. Move the selected list pointer forward.
4. Move `tail` forward.

When one list becomes `null`, the remaining list is already sorted.

Attach the remaining list directly to `tail.next`.

Finally, return:

```text
dummy.next
```

because `dummy` itself is only a placeholder.

---

## Interview Explanation

I use two pointers, `list1` and `list2`, to traverse the two sorted lists.

A dummy node is used as the starting point of the result list, and `tail` tracks the last node of the merged list.

At each step, I compare the values of the current nodes. I attach the smaller node to `tail.next` and advance that list pointer.

Once either list is exhausted, all remaining nodes in the other list are already sorted, so I attach the entire remaining list directly.

The dummy node simplifies handling the first node of the merged list, and I return `dummy.next` because the dummy itself is not part of the result.

---

## Success Solution

```csharp
public class Solution {
    public ListNode MergeTwoLists(ListNode list1, ListNode list2) {

        ListNode dummy = new ListNode();
        ListNode tail = dummy;

        while(list1 != null && list2 != null){

            if(list1.val <= list2.val){
                tail.next = list1;
                list1 = list1.next;
            }
            else{
                tail.next = list2;
                list2 = list2.next;
            }

            tail = tail.next;
        }

        if(list1 != null){
            tail.next = list1;
        }

        if(list2 != null){
            tail.next = list2;
        }

        return dummy.next;
    }
}
```

---

## Success Template

```text
Create dummy node

tail = dummy

While list1 AND list2 are not null

    Compare list1 and list2

    If list1 is smaller/equal

        tail.next = list1
        list1 = list1.next

    Else

        tail.next = list2
        list2 = list2.next

    tail = tail.next


If list1 remains

    tail.next = list1

If list2 remains

    tail.next = list2


Return dummy.next
```

---

## Complexity

**Time Complexity:** `O(n + m)`

Where:

- `n` = number of nodes in `list1`
- `m` = number of nodes in `list2`

Every node is processed once.

**Space Complexity:** `O(1)`

No new nodes are created for the merged result. Existing nodes are reused.

---

## Revision Notes

### Dummy Node

A dummy node is a temporary placeholder used before the actual result list.

```text
dummy → 1 → 2 → 3 → ...
  ↑
```

`tail` starts at `dummy` and moves through the merged list.

At the end:

```text
dummy → 1 → 2 → 3 → ...
         ↑
       actual head
```

Therefore:

```csharp
return dummy.next;
```

not:

```csharp
return dummy;
```

---

### Tail Pointer

`tail` always points to the last node in the merged list.

When a node is selected:

```csharp
tail.next = list1;
```

or:

```csharp
tail.next = list2;
```

Then:

```csharp
tail = tail.next;
```

moves `tail` to the newly attached node.

---

### Why Can We Attach the Remaining List Directly?

Suppose:

```text
list1: null

list2: 7 → 8 → 9 → null
```

There is no need to compare anything anymore.

Because `list2` was already sorted:

```text
7 → 8 → 9
```

we can simply do:

```csharp
tail.next = list2;
```

The same applies if `list2` becomes `null`.

---

### Why Do We Reuse Existing Nodes?

We don't create new nodes for every value.

For example, when:

```csharp
tail.next = list1;
```

we connect the existing `list1` node directly to the result.

Then:

```csharp
list1 = list1.next;
```

moves the list pointer forward.

We are **rearranging existing `next` references**, not copying the nodes.

---

### Important Pointer Pattern

Every comparison follows:

```text
Compare

↓

Attach selected node

↓

Move selected list pointer

↓

Move tail
```

Code:

```csharp
tail.next = list1;
list1 = list1.next;
tail = tail.next;
```

or:

```csharp
tail.next = list2;
list2 = list2.next;
tail = tail.next;
```

**Mental Question:** Why do we use a dummy node instead of directly choosing the first node?

**Answer:** It removes the special case for determining the first node of the merged list. Every selected node can be attached using the same `tail.next` operation.

---

## Pattern Learned

```text
Two Sorted Linked Lists

↓

Two Pointers

↓

Compare Current Nodes

↓

Attach Smaller Node

↓

Move Selected Pointer

↓

Move Tail

↓

Attach Remaining List

↓

Return dummy.next
```

---

## Key Takeaway

The important part is not the code itself.

The core operation is:

```text
Take the smaller current node
        ↓
Connect it to tail
        ↓
Move that list forward
        ↓
Move tail forward
```

This is the foundation for more advanced problems involving **merging linked lists**, including heap-based linked-list problems.