# LeetCode 278 - First Bad Version

**Pattern:** Binary Search - Boundary Search

---

## Problem

Suppose you have `n` versions of a product. Versions are numbered from `1` to `n`.

Once a bad version appears, all versions after it are also bad.

Find the **first bad version** by minimizing the number of API calls to `IsBadVersion()`.

### Test Case 1

```text
Input:
n = 5
firstBadVersion = 4

Output:
4
```

### Test Case 2

```text
Input:
n = 1
firstBadVersion = 1

Output:
1
```

---

## Approach

### Brute Force

Start from version `1` and check every version using `IsBadVersion()`.

Return the first version where the API returns `true`.

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`

---

### Optimized Approach

The versions are ordered:

```text
Good Good Good Bad Bad Bad ...
```

This creates a boundary.

Use Binary Search to find the first version where `IsBadVersion()` becomes `true`.

* If `mid` is bad:

  * Save it as a possible answer.
  * Search the left half for an earlier bad version.
* Otherwise:

  * Search the right half.

Continue until the search space becomes empty.

---

## Interview Explanation

The versions form two regions: good versions followed by bad versions.

Since the transition happens only once, Binary Search can efficiently locate the boundary.

Whenever I find a bad version, I store it because it is a valid candidate. However, there may be an earlier bad version, so I continue searching the left half.

If the current version is good, then all earlier versions are also good, so I search the right half.

At the end, the stored answer is the first bad version.

---

## Success Solution

```csharp
public class Solution : VersionControl {
    public int FirstBadVersion(int n) {
        int i = 1, j = n, answer = n;

        while(i <= j){
            int mid = i + (j - i) / 2;

            if(IsBadVersion(mid)){
                answer = mid;
                j = mid - 1;
            }
            else{
                i = mid + 1;
            }
        }

        return answer;
    }
}
```

---

## Success Template

```text
left = first possible version
right = last possible version
answer = default valid answer

while(left <= right)

    mid = left + (right - left) / 2

    if(mid is bad)

        answer = mid
        right = mid - 1

    else

        left = mid + 1

return answer
```

---

## Complexity

**Time Complexity:** `O(log n)`

**Space Complexity:** `O(1)`

---

## Revision Notes

* Versions are sorted into two regions: Good → Bad.
* Search for the **first valid boundary**.
* Save every valid candidate before moving left.
* If the current version is good, move right.
* If the current version is bad, move left to check for an earlier bad version.
* Use `left + (right - left) / 2` to calculate the middle safely.
* Mental Question: **Have I found the first bad version, or could there be an earlier one?**
