# 20. Valid Parentheses

**Difficulty:** Easy

**Pattern:** Stack

**LeetCode:** https://leetcode.com/problems/valid-parentheses/

---

# Problem Statement

Given a string `s` containing just the characters:

- '('
- ')'
- '{'
- '}'
- '['
- ']'

Determine if the input string is valid.

A string is valid if:

1. Every opening bracket has a corresponding closing bracket.
2. Brackets are closed in the correct order.
3. Every closing bracket closes the most recently opened bracket.

---

# Brute Force Idea

Whenever a closing bracket is encountered, search backwards to find its matching opening bracket.

This approach becomes inefficient because multiple searches may be required.

### Time Complexity

O(n²)

---

# Optimal Idea

Whenever an opening bracket is encountered, store it inside a stack.

Whenever a closing bracket is encountered:

- If the stack is empty → Invalid
- Otherwise compare it with the top of the stack.
- If both brackets match, pop the stack.
- Otherwise return false.

At the end, if the stack is empty, every opening bracket found its matching closing bracket.

---

# Why Stack?

A stack follows the **Last In First Out (LIFO)** principle.

The most recently opened bracket must always be the first one to close.

Example:

```
{
    [
    ]
}
```

The '[' must close before '{'.

A stack naturally models this behaviour.

---

# Mental Model

Think of opening brackets as unfinished work.

Whenever you see:

```
(
```

You don't know when it will close, so store it.

Whenever you see:

```
)
```

Check whether it closes the latest unfinished opening bracket.

If yes:

Remove it from the stack.

Otherwise:

The string is invalid.

---

# Algorithm

1. Create an empty stack.
2. Traverse every character.
3. If it is an opening bracket, push it.
4. Otherwise:
   - If stack is empty → return false.
   - Peek the top element.
   - If brackets match → Pop.
   - Otherwise → return false.
5. After traversal, return true only if the stack is empty.

---

# Accepted Solution

```csharp
public class Solution {
    public bool IsValid(string s) {
        Stack<char> stack = new Stack<char>();

        foreach(char c in s){
            if(c=='(' || c=='{' || c=='['){
                stack.Push(c);
            }
            else{
                if(stack.Count==0){
                    return false;
                }

                char top = stack.Peek();

                if((c==')' && top=='(') ||
                   (c==']' && top=='[') ||
                   (c=='}' && top=='{')){
                    stack.Pop();
                }
                else{
                    return false;
                }
            }
        }

        return stack.Count==0;
    }
}
```

---

# Dry Run

Input

```
{[]}
```

Stack Progress

```
Read {

Stack
{
```

```
Read [

Stack
[
{
```

```
Read ]

Top = [

Match

Pop

Stack
{
```

```
Read }

Top = {

Match

Pop

Stack Empty
```

Return

```
true
```

---

# Time Complexity

Traversal

```
O(n)
```

Each opening bracket is pushed once.

Each opening bracket is popped once.

Total operations remain linear.

---

# Space Complexity

Worst Case

```
O(n)
```

Example

```
((((((((
```

All opening brackets stay in the stack.

---

# Key Learning

- Stack stores unfinished work.
- Always compare only with the top.
- Never search the whole stack.
- Every element is pushed once and popped once.
- LIFO perfectly models nested structures.

---

# Interview Explanation

"I use a stack to keep track of opening brackets.

Whenever I encounter an opening bracket, I push it into the stack.

Whenever I encounter a closing bracket, I first check if the stack is empty.

If it is empty, the string is invalid because there is no matching opening bracket.

Otherwise I compare the closing bracket with the top of the stack.

If they match, I pop the opening bracket.

If they don't match, I immediately return false.

Finally, if the stack is empty, every opening bracket was matched correctly."

---

# Common Mistakes

❌ Forgetting to check whether the stack is empty before Peek()

❌ Using Contains() instead of Peek()

❌ Forgetting to verify that the stack is empty at the end

❌ Comparing the current character with itself instead of the stack top

---

# Revision Notes

Remember:

Opening Bracket

```
Push
```

Closing Bracket

```
Peek

↓

Compare

↓

Pop
```

No searching.

Only the top matters.

---