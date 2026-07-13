# LeetCode 125 - Valid Palindrome

**Pattern:** Two Pointers (Opposite Direction)

---

## Problem

Determine whether a string is a palindrome after ignoring all non-alphanumeric characters and treating uppercase and lowercase letters as the same.

### Test Case 1

```text id="lmho0d"
Input:
s = "A man, a plan, a canal: Panama"

Output:
true
```

### Test Case 2

```text id="gdvzkr"
Input:
s = "race a car"

Output:
false
```

---

## Approach

### Brute Force

Create a new string containing only alphanumeric characters.

Convert all characters to lowercase.

Create another reversed string and compare both strings.

**Time Complexity:** `O(n)`

**Space Complexity:** `O(n)`

---

### Optimized Approach

Use two pointers.

* `i` = Left Pointer
* `j` = Right Pointer

Move both pointers towards the center.

Skip every non-alphanumeric character.

Convert both valid characters to lowercase before comparing.

If any pair of characters is different, return `false`.

If the loop completes successfully, return `true`.

---

## Interview Explanation

I use two pointers starting from both ends of the string.

Before comparing, I skip all non-alphanumeric characters because they do not affect the palindrome.

I convert both characters to lowercase to perform a case-insensitive comparison.

If a mismatch is found, the string is not a palindrome.

Otherwise, both pointers move inward until they meet.

---

## Success Solution

```csharp id="8jlwm8"
public class Solution {
    public bool IsLetterOrDigit(char c)
    {
        if ((c >= 'A' && c <= 'Z') ||
            (c >= 'a' && c <= 'z') ||
            (c >= '0' && c <= '9'))
        {
            return true;
        }

        return false;
    }

    public bool IsPalindrome(string s) {
        int i=0,j=s.Length-1;
        while(i<j){
            while(i<j && !IsLetterOrDigit(s[i])){
                i++;
            }
            while(i<j && !IsLetterOrDigit(s[j])){
                j--;
            }
            if(Char.ToLower(s[i])!=Char.ToLower(s[j])){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```

---

## Success Template

```text id="2bgbxo"
Initialize:

left = 0
right = n - 1

While left < right:

    Skip invalid characters from left.

    Skip invalid characters from right.

    Compare lowercase characters.

    If mismatch:
        return false

    Move both pointers inward.

Return true.
```

---

## Complexity

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Opposite-direction pointers
* Skip non-alphanumeric characters
* Compare after converting to lowercase
* Left pointer moves right
* Right pointer moves left
* Stop on first mismatch
* `while(i < j)` is sufficient
* Mental Question: **After skipping invalid characters, do the current characters match?**
