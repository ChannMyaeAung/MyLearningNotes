# Longest Increasing Subsequence (LIS)

A **subsequence** means the elements need not be contiguous, only in the same order.

Given an array `arr` of length `n`, the **Longest Increasing Subsequence (LIS)** is the **longest** subsequence of `arr` in which the elements are **strictly increasing**.

- A **subsequence** is a sequence we can derive from the array by **removing** zero or more elements **without changing the order** of the remaining elements.
- **Strictly increasing** means each element is **greater** than the previous one (i.e.,  arr[i<sub>k + 1</sub>] > arr[i<sub>k</sub>]]).

--

### Small Example

- Suppose the array is `arr = [10, 9, 2, 5, 3, 7, 101, 18]`.
- One of the **longest** increasing subsequences is `[2, 3, 7, 18]` or `[2, 5, 7, 18]` or `[2, 5, 7, 101]` (length 4).
- The length of the LIS is **4**.

---

## Pseudocode

```css
for i in [0..n-1]:
    dp[i] = 1

max_length = 1   // at least one element

for i in [1..n-1]:
    for j in [0..i-1]:
        if arr[j] < arr[i]:
            dp[i] = max(dp[i], dp[j] + 1)
    max_length = max(max_length, dp[i])

return max_length
```

### Complexity:

- We have a nested loop (`i` and `j`), each up to `n`.

- Time complexity: O(n<sup>2</sup>)

- Space complexity: O(n).

---

## C Implementation

Below is a **self-contained** C program that:

1. Reads an array from the user (or uses a fixed array).
2. Computes the **LIS** length using the O(n<sup>2</sup>) DP solution.
3. Prints the result.

#### Program Code

`hello.h`

```c
```

`hello.c`

```c
```

`main.c`

```c
```



#### Program Output

```shell
```

