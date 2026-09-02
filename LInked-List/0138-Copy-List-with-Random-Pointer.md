# LeetCode 138 - Copy List with Random Pointer

**Pattern:** HashMap / Dictionary + Deep Copy + Two Passes

---

## Problem

Given a linked list where each node contains:

```text
val
next
random
```

Create a **deep copy** of the linked list.

The `next` pointer points to the next node, while the `random` pointer can point to any node in the list or `null`.

---

## Example

Original:

```text
1 → 2 → 3
```

Random pointers:

```text
1.random → 3
2.random → 1
3.random → 2
```

We need:

```text
1' → 2' → 3'
```

with:

```text
1'.random → 3'
2'.random → 1'
3'.random → 2'
```

The copied nodes must not point to the original nodes.

---

## Approach

The main problem is translating relationships from the original list to the copied list.

We use a Dictionary:

```text
Original Node → Copied Node
```

For example:

```text
Original       Copy

1       →      1'
2       →      2'
3       →      3'
```

Then we use this mapping to connect the `next` and `random` pointers of the copied nodes.

The solution uses **two passes**:

```text
FIRST PASS
    ↓
Create every copied node
    ↓
Store Original → Copy
    ↓
SECOND PASS
    ↓
Connect next and random
    ↓
Return copied head
```

---

## Step 1 — Handle Empty List

```csharp
if(head == null)
    return null;
```

If the list is empty:

```text
head = null
```

there is nothing to copy.

---

## Step 2 — Create the Dictionary

```csharp
Dictionary<Node,Node> map = new();
Node current = head;
```

The dictionary stores:

```text
original node → copied node
```

`current` starts at `head` so we can traverse the original list.

---

## Step 3 — First Pass: Create All Nodes

```csharp
while(current != null){
    map[current] = new Node(current.val);
    current = current.next;
}
```

For:

```text
1 → 2 → 3
```

we create:

```text
1'   2'   3'
```

and store:

```text
map[1] = 1'
map[2] = 2'
map[3] = 3'
```

At this point, all copied nodes exist.

But their `next` and `random` pointers are not connected yet.

---

## Step 4 — Reset `current`

```csharp
current = head;
```

We start from the original head again for the second pass.

---

## Step 5 — Connect the `random` Pointer

```csharp
map[current].random =
    current.random == null
        ? null
        : map[current.random];
```

Suppose:

```text
1.random → 3
```

The dictionary contains:

```text
map[1] → 1'
map[3] → 3'
```

Therefore:

```text
map[1].random = map[3]
```

which gives:

```text
1'.random → 3'
```

This is the key reason we need the Dictionary.

---

## Step 6 — Connect the `next` Pointer

```csharp
map[current].next =
    current.next == null
        ? null
        : map[current.next];
```

Suppose:

```text
1.next → 2
```

Then:

```text
map[1].next → map[2]
```

so:

```text
1' → 2'
```

---

## Step 7 — Move to the Next Node

```csharp
current = current.next;
```

Continue until every original node has been processed.

---

## Step 8 — Return the Copied Head

```csharp
return map[head];
```

`map[head]` is the copied version of the original head.

---

## Interview Explanation

I use a Dictionary that maps every original node to its corresponding copied node.

In the first pass, I create a new node for every original node and store the mapping.

In the second pass, I connect the `next` and `random` pointers. Whenever an original node points to another original node, I use the dictionary to find the corresponding copied node.

Finally, I return the copied version of the original head.

This gives `O(n)` time and `O(n)` extra space.

---

## Success Solution

```csharp
public class Solution {
    public Node CopyRandomList(Node head) {

        if(head == null)
            return null;

        Dictionary<Node,Node> map = new();
        Node current = head;

        // First pass: create copies
        while(current != null){
            map[current] = new Node(current.val);
            current = current.next;
        }

        // Second pass: connect pointers
        current = head;

        while(current != null){
            map[current].random =
                current.random == null
                    ? null
                    : map[current.random];

            map[current].next =
                current.next == null
                    ? null
                    : map[current.next];

            current = current.next;
        }

        return map[head];
    }
}
```

---

## Success Template

```text
if head == null
    return null


map = Dictionary<Original, Copy>

current = head

while current != null

    map[current] = new Copy(current.val)
    current = current.next


current = head

while current != null

    copy = map[current]

    copy.random =
        current.random == null
        ? null
        : map[current.random]

    copy.next =
        current.next == null
        ? null
        : map[current.next]

    current = current.next


return map[head]
```

---

## Complexity

**Time Complexity:** `O(n)`

```text
First pass  → O(n)
Second pass → O(n)

Total → O(n)
```

**Space Complexity:** `O(n)`

The Dictionary stores one mapping for every node.

```text
n original nodes
↓
n mappings
↓
O(n)
```

---

## Revision Notes

### The Dictionary Is NOT

Do not think:

```text
node → random
```

The Dictionary is:

```text
original → copy
```

That is the most important concept in this problem.

---

### Why Two Passes?

Consider:

```text
1.random → 3
```

We need:

```text
1'.random → 3'
```

Before connecting the random pointer, we need to know which copied node represents `3`.

So first create every copy:

```text
1 → 1'
2 → 2'
3 → 3'
```

Then we can safely connect:

```text
1'.random → 3'
```

using the Dictionary.

---

### Why Can't We Do This?

Wrong:

```csharp
map[current].random = current.random;
```

If:

```text
current = 1
current.random = 3
```

then:

```text
1'.random → 3
```

The copied node points to an **original** node.

❌ Wrong.

We need:

```text
1'.random → 3'
```

So we use:

```csharp
map[current.random]
```

---

### Why Check for `null`?

Both can be `null`:

```text
current.next
current.random
```

So we use:

```csharp
current.random == null
    ? null
    : map[current.random]
```

and:

```csharp
current.next == null
    ? null
    : map[current.next]
```

---

### Empty List

```text
head = null
```

Immediately:

```csharp
return null;
```

---

### One Node

Suppose:

```text
1
```

and:

```text
1.random → 1
```

The copy must have:

```text
1'.random → 1'
```

The Dictionary handles this naturally:

```text
map[1] = 1'
```

Therefore:

```text
map[1.random]
=
map[1]
=
1'
```

---

## Complete Example

Original:

```text
1 → 2 → 3
```

Random:

```text
1.random → 3
2.random → 1
3.random → 2
```

### First Pass

Create:

```text
1'   2'   3'
```

Dictionary:

```text
map[1] = 1'
map[2] = 2'
map[3] = 3'
```

### Second Pass

Translate:

```text
1.next → 2
```

into:

```text
1'.next → 2'
```

Translate:

```text
1.random → 3
```

into:

```text
1'.random → 3'
```

Then:

```text
2'.next → 3'
2'.random → 1'

3'.next → null
3'.random → 2'
```

Final:

```text
1' → 2' → 3'
│    │    │
↓    ↓    ↓
3'   1'   2'
```

---

## Pattern Learned

```text
Copy Complex Linked List

Original Nodes
      ↓
Create Copies
      ↓
Dictionary:
Original → Copy
      ↓
Translate Relationships
      ↓
Return Copy Head
```

The deeper pattern is:

> **Use a mapping when you need to create copies of objects while preserving relationships between them.**

---

## Key Takeaway

The difficult part isn't creating the nodes.

It's preserving the **relationships**.

Original:

```text
A → B
A.random → C
```

Copied:

```text
A' → B'
A'.random → C'
```

The Dictionary provides the translation:

```text
A → A'
B → B'
C → C'
```

So whenever the original says:

```text
A → C
```

we translate it to:

```text
map[A] → map[C]
```

Remember:

```text
FIRST PASS:
Create + Map

SECOND PASS:
Connect
```

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

138 - Copy List with Random Pointer
     ↓
Dictionary + Deep Copy + Relationship Mapping
```
