# LeetCode 901 - Online Stock Span

**Pattern:** Monotonic Decreasing Stack

---

## Problem

Design an algorithm that collects daily stock prices and returns the **span** of the stock's price for the current day.

The span of today's price is defined as the maximum number of consecutive days (starting from today and going backwards) for which the stock price was **less than or equal to** today's price.

Implement the `StockSpanner` class:

* `StockSpanner()` Initializes the object.
* `int Next(int price)` Returns the span of the current day's price.

### Test Case 1

```text
Input:
["StockSpanner","next","next","next","next","next","next","next"]

[[],[100],[80],[60],[70],[60],[75],[85]]

Output:
[null,1,1,1,2,1,4,6]
```

### Explanation

```text
100 -> Span = 1

80 -> Span = 1

60 -> Span = 1

70 -> Span = 2

60 -> Span = 1

75 -> Span = 4

85 -> Span = 6
```

---

## Approach

### Brute Force

For every new stock price:

* Traverse previous prices one by one.
* Count consecutive prices less than or equal to the current price.
* Stop when a greater price is found.

Time Complexity: **O(n)** per query.

If there are `n` queries, the overall complexity becomes:

**O(n²)**

---

### Optimized Approach

Use a **Monotonic Decreasing Stack**.

Instead of storing only prices, store:

```
(price, span)
```

For every new price:

* Initialize span as `1`.
* While the current price is greater than or equal to the price on the top of the stack:
  * Pop the previous element.
  * Add its stored span to the current span.
* Push the current `(price, span)` pair into the stack.
* Return the calculated span.

Each element is pushed once and popped once.

---

## Interview Explanation

I use a Monotonic Decreasing Stack where each stack element stores both the stock price and the span already calculated for that price.

Whenever a new price arrives, I continuously pop all smaller or equal prices and accumulate their spans into the current span.

This avoids recounting previous days because each popped element already represents multiple consecutive days.

Finally, I push the current `(price, span)` pair into the stack and return the calculated span.

---

## Success Solution

```csharp
public class StockSpanner {

    Stack<(int price ,int span)> stack;

    public StockSpanner() {
        stack = new Stack<(int,int)>();
    }

    public int Next(int price) {
        int span = 1;

        while(stack.Count > 0 && stack.Peek().price <= price){
            span += stack.Pop().span;
        }

        stack.Push((price, span));

        return span;
    }
}
```

---

## Success Template

```text
Initialize:

Create Stack storing

(price, span)

For every new price:

span = 1

While stack is not empty

AND

top.price <= current price

    span += top.span

    Pop

Push

(current price, span)

Return span
```

---

## Complexity

**Time Complexity:** `O(1)` Amortized

**Space Complexity:** `O(n)`

---

## Revision Notes

* Use a Monotonic Decreasing Stack
* Store `(price, span)` instead of only price
* Initialize span as `1`
* Pop all prices less than or equal to the current price
* Add the popped span instead of incrementing by `1`
* Push `(current price, current span)`
* Every element is pushed once
* Every element is popped once
* Mental Question: **Can today's price absorb the span of previous smaller prices?**