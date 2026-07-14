# LeetCode 680 - Valid Palindrome II

**Pattern:** Two Pointers (Opposite Direction)

---

## Problem

Given a string `s`, return `true` if it can become a palindrome after deleting **at most one** character.

### Test Case 1

```text id="jlwm01"
Input:
s = "abca"

Output:
true

Explanation:
Delete 'c' → "aba"
or
Delete 'b' → "aca"
```

### Test Case 2

```text id="jlwm02"
Input:
s = "abc"

Output:
false
```

---

## Approach

### Brute Force

Try deleting every character one by one.

For every deletion, check whether the remaining string is a palindrome.

If any deletion forms a palindrome, return `true`.

Otherwise return `false`.

**Time Complexity:** `O(n²)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

Use two pointers from opposite ends.

Compare both characters.

If they are equal, continue moving inward.

At the first mismatch, try two possibilities:

* Skip the left character.
* Skip the right character.

Use a helper function to verify whether the remaining substring is a palindrome.

If either helper call returns `true`, the answer is `true`.

Otherwise return `false`.

---

## Interview Explanation

I compare characters using two pointers.

As long as both characters are equal, I continue moving inward.

At the first mismatch, I am allowed to delete one character.

So I check whether skipping the left character forms a palindrome or skipping the right character forms a palindrome.

The helper function performs a normal palindrome check on the remaining substring.

If either possibility succeeds, the string is valid.

---

## Success Solution

```csharp id="jlwm03"
public class Solution {
    public bool IsPalindrome(string s,int i , int j){
        while(i<j){
            if(s[i]!=s[j]){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }

    public bool ValidPalindrome(string s) {
        int i=0,j=s.Length-1;
        while(i<j){
            if(s[i]==s[j]){
                i++;
                j--;
            }
            else {
                return IsPalindrome(s,i+1,j)||IsPalindrome(s,i,j-1);
            }
        }
        return true;
    }
}
```

---

## Success Template

```text id="jlwm04"
Initialize:

left = 0
right = n - 1

While left < right

    If characters match

        Move both pointers.

    Else

        return

        IsPalindrome(left + 1, right)

        OR

        IsPalindrome(left, right - 1)

Return true.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Opposite-direction pointers
* First mismatch is the only decision point
* Two possibilities:

  * Skip left
  * Skip right
* Helper checks the remaining substring only
* Already matched characters are never checked again
* `OR` means either deletion is acceptable
* Mental Question: **If I delete one side, does the remaining substring stay a palindrome?**
