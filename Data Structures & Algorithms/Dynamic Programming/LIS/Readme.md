# Longest Increasing Subsequence (LIS)

A **subsequence** means the elements need not be contiguous, only in the same order.

Given an array `arr` of length `n`, the **Longest Increasing Subsequence (LIS)** is the **longest** subsequence of `arr` in which the elements are **strictly increasing**.

- A **subsequence** is a sequence we can derive from the array by **removing** zero or more elements **without changing the order** of the remaining elements.
- **Strictly increasing** means each element is **greater** than the previous one (i.e.,  arr[i<sub>k + 1</sub>] > arr[i<sub>k</sub>]]).
- What makes this problem suitable for dynamic programming is that any prefix of a longest increasing subsequence is a longest increasing subsequence of the part of the array that ends where the prefix ends; if it weren't, we could make a big sequence longer by choosing a longer prefix.

---

### Small Example # 1

- Suppose the array is `arr = [10, 9, 2, 5, 3, 7, 101, 18]`.
- One of the **longest** increasing subsequences is `[2, 3, 7, 18]` or `[2, 5, 7, 18]` or `[2, 5, 7, 101]` (length 4).
- The length of the LIS is **4**.

### Small Example #2

Consider the sequence: `10, 22, 9, 33, 21, 50, 41, 60, 80`

Possible increasing subsequences include:

- `10, 22, 33, 50, 60, 80` (Length: 6)
- `10, 22, 33, 41, 60, 80` (Length: 6)
- `10, 22, 50, 60, 80` (Length: 5)
- `9, 21, 41, 60, 80` (Length: 5)

Here, the **Longest Increasing Subsequence** has a length of **6**, such as `10, 22, 33, 50, 60, 80`.

We don't need to choose consecutive elements, just elements that maintain an increasing order.

**Note:** There can be multiple LIS with the same maximum length.

---

### Algorithmic Approach

There are two primary ways to solve this problem:

1. Dynamic Programming (O(n²) time complexity)
2. Dynamic Programming with Binary Search (O(n log n) time complexity)

---

## Pseudocode (O(n<sup>2</sup>))

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

#### Program Code

`hello.h`

```c
int LIS(int arr[], int n, int prev[]);

void printLIS(int arr[], int prev[], int i);
```

`hello.c`

```c
int LIS(int arr[], int n, int prev[])
{
    if (n == 0)
    {
        return 0;
    }

    // dp[i] will hold the length of the LIS ending at index i
    int *dp = malloc(n * sizeof(int));
    assert(dp);

    // initialize each position to 1 since the min LIS length is 1
    for (int i = 0; i < n; i++)
    {
        dp[i] = 1;
        prev[i] = -1;
    }

    int maxLength = 1; // At least one element
    int maxIdx = 0;    // to track the index where the LIS ends

    // Build the dp array
    for (int i = 1; i < n; i++)
    {
        for (int j = 0; j < i; j++)
        {
            // if current element is greater than previous element and including it increases the LIS length
            if (arr[j] < arr[i] && dp[j] + 1 > dp[i])
            {
                dp[i] = dp[j] + 1; // update LIS length at i
                prev[i] = j;       // update predecessor of i
            }
        }

        // update maximum LIS length and its ending index
        if (dp[i] > maxLength)
        {
            maxLength = dp[i];
            maxIdx = i;
        }
    }

    // print the LIS
    printf("Longest Increasing Subsequence: ");
    printLIS(arr, prev, maxIdx);
    printf("\n");
    free(dp);
    return maxLength;
}

// Function to print the Longest Increasing Subsequence using the prev array
void printLIS(int arr[], int prev[], int i)
{
    if (i == -1)
    {
        return; // Base case: no more elements to print
    }
    printLIS(arr, prev, prev[i]); // Recursively print earlier elements
    printf("%d ", arr[i]);        // Print current element
}
```

`main.c`

```c
int main()
{
    int arr[] = {10, 22, 9, 33, 21, 50, 41, 60, 80};
    int n = sizeof(arr) / sizeof(arr[0]);
    int *prev = malloc(n * sizeof(int));
    assert(prev);
    int lisLength = LIS(arr, n, prev);

    printf("Length of LIS is %d\n", lisLength);

    int arr2[] = {10, 9, 2, 5, 3, 7, 101, 18};
    int n2 = sizeof(arr2) / sizeof(arr2[0]);

    int *prev2 = malloc(n2 * sizeof(int));
    assert(prev2);
    int lisLength2 = LIS(arr2, n2, prev2);

    printf("Length of LIS is %d\n", lisLength2);

    free(prev);
    free(prev2);
    return 0;
}

```

#### Step-by-Step Execution

1. **Initialization:**
   - `arr[] = {10, 22, 9, 33, 21, 50, 41, 60, 80}`
   - `n = 9`
   - `dp[] = {1, 1, 1, 1, 1, 1, 1, 1, 1}` (initialized to 1)
   - `prev[] = {-1, -1, -1, -1, -1, -1, -1, -1, -1}` (initialized to -1)
   - `maxLength = 1`
   - `maxIndex = 0`

**Step 2: Processing Each Element**

**Element at Index 0 (`10`)**

- **Action**: Starting element; no previous elements to compare.
- **`dp` remains**: `1`
- **`prev` remains**: `-1`
- **`maxLength` and `maxIndex` remain**: `1` and `0`

**Element at Index 1 (`22`)**

- Comparisons:
  - Compare with `10`(Index 0): i = 1, j = 0
    - `10 < 22` and `dp[0] + 1 > dp[1]` → `1 + 1 > 1`
    - **Update:**
      - `dp[1] = 2`
      - `prev[1] = 0`
- **`dp` now**: `{1, 2, 1, 1, 1, 1, 1, 1, 1}`
- **`prev` now**: `{-1, 0, -1, -1, -1, -1, -1, -1, -1}`
- **Update `maxLength` to `2` and `maxIndex` to `1`**

**Element at Index 2 (`9`)**

- Comparisons:
  - Compare with `10`(Index 0): i = 2, j = 0
    - `10 < 9` → **No update**
  - Compare with `22` (Index 1): i = 2, j = 1
    - `22 < 9` → **No update**
- **`dp` remains**: `{1, 2, 1, 1, 1, 1, 1, 1, 1}`
- **`prev` remains**: `{-1, 0, -1, -1, -1, -1, -1, -1, -1}`
- **`maxLength` and `maxIndex` remain**: `2` and `1`

**Element at Index 3 (`33`)**

- Comparisons:
  - Compare with `10` (Index 0): i = 3, j = 0
    - `10 < 33` and `dp[0] + 1 > dp[3]` → `1 + 1 > 1`
    - Update:
      - `dp[3] = 2`
      - `prev[3] = 0`
  - Compare with `22` (Index 1): i = 3, j = 1
    - `22 < 33` and `dp[1] + 1 > dp[3]` → `2 + 1 > 2`
    - Update:
      - `dp[3] = 3`
      - `prev[3] = 1`
  - Compare with `9` (Index 2): i = 3, j = 2
    - `9 < 33` but `dp[2] + 1 > dp[3]` → `1 + 1 > 3` → **No update**
- **`dp` now**: `{1, 2, 1, 3, 1, 1, 1, 1, 1}`
- **`prev` now**: `{-1, 0, -1, 1, -1, -1, -1, -1, -1}`
- **Update `maxLength` to `3` and `maxIndex` to `3`**

**Element at Index 4 (`21`)**

- Comparisons:
  - Compare with `10` (Index 0): i = 4, j = 0
    - `10 < 21` and `dp[0] + 1 > dp[4]` → `1 + 1 > 1`
    - Update:
      - `dp[4] = 2`
      - `prev[4] = 0`
  - Compare with `22` (Index 1): i = 4, j = 1
    - `22 < 21` → **No update**
  - Compare with `9` (Index 2): i = 4, j = 2
    - `9 < 21` and `dp[2] + 1 > dp[4]` → `1 + 1 > 2` → **No update**
  - Compare with `33` (Index 3): i = 4, j = 3
    - `33 < 21` → **No update**
- **`dp` now**: `{1, 2, 1, 3, 2, 1, 1, 1, 1}`
- **`prev` now**: `{-1, 0, -1, 1, 0, -1, -1, -1, -1}`
- **`maxLength` and `maxIndex` remain**: `3` and `3`

**Element at Index 5 (`50`)**

- Comparisons:
  - Compare with `10` (Index 0): i = 5, j = 0
    - `10 < 50` and `dp[0] + 1 > dp[5]` → `1 + 1 > 1`
    - Update:
      - `dp[5] = 2`
      - `prev[5] = 0`
  - Compare with `22` (Index 1): i = 5, j = 1
    - `22 < 50` and `dp[1] + 1 > dp[5]` → `2 + 1 > 2`
    - Update:
      - `dp[5] = 3`
      - `prev[5] = 1`
  - Compare with `9` (Index 2): i = 5, j = 2
    - `9 < 50` but `dp[2] + 1 > dp[5]` → `1 + 1 > 3` → **No update**
  - Compare with `33` (Index 3): i = 5, j = 3
    - `33 < 50` and `dp[3] + 1 > dp[5]` → `3 + 1 > 3`
    - Update:
      - `dp[5] = 4`
      - `prev[5] = 3`
  - Compare with `21` (Index 4): i = 5, j = 4
    - `21 < 50` but `dp[4] + 1 > dp[5]` → `2 + 1 > 4` → **No update**
- **`dp` now**: `{1, 2, 1, 3, 2, 4, 1, 1, 1}`
- **`prev` now**: `{-1, 0, -1, 1, 0, 3, -1, -1, -1}`
- **Update `maxLength` to `4` and `maxIndex` to `5`**

**Element at Index 6 (`41`)**

- Comparisons:
  - Compare with `10` (Index 0): i = 6, j = 0
    - `10 < 41` and `dp[0] + 1 > dp[6]` → `1 + 1 > 1`
    - Update:
      - `dp[6] = 2`
      - `prev[6] = 0`
  - Compare with `22` (Index 1): i = 6, j = 1
    - `22 < 41` and `dp[1] + 1 > dp[6]` → `2 + 1 > 2`
    - Update:
      - `dp[6] = 3`
      - `prev[6] = 1`
  - Compare with `9` (Index 2): i = 6, j = 2
    - `9 < 41` but `dp[2] + 1 > dp[6]` → `1 + 1 > 3` → **No update**
  - Compare with `33` (Index 3): i = 6, j = 3
    - `33 < 41` and `dp[3] + 1 > dp[6]` → `3 + 1 > 3`
    - Update:
      - `dp[6] = 4`
      - `prev[6] = 3`
  - Compare with `21` (Index 4): i = 6, j = 4
    - `21 < 41` but `dp[4] + 1 > dp[6]` → `2 + 1 > 4` → **No update**
  - Compare with `50` (Index 5): i = 6, j = 5
    - `50 < 41` → **No update**
- **`dp` now**: `{1, 2, 1, 3, 2, 4, 4, 1, 1}`
- **`prev` now**: `{-1, 0, -1, 1, 0, 3, 3, -1, -1}`
- **`maxLength` remains**: `4`
- **`maxIndex` remains**: `5`

**Element at Index 7 (`60`)**

- Comparisons:

  - Compare with `10` (Index 0): i = 7, j = 0

    - `10 < 60` and `dp[0] + 1 > dp[7]` → `1 + 1 > 1`
    - Update:
      - `dp[7] = 2`
      - `prev[7] = 0`

  - Compare with `22` (Index 1): i = 7, j = 1

    - `22 < 60` and `dp[1] + 1 > dp[7]` → `2 + 1 > 2`
    - Update:
      - `dp[7] = 3`
      - `prev[7] = 1`

  - Compare with `9` (Index 2): i = 7, j = 2

    - `9 < 60` but `dp[2] + 1 > dp[7]` → `1 + 1 > 3` → **No update**

  - Compare with `33` (Index 3): i = 7, j = 3

    - `33 < 60` and `dp[3] + 1 > dp[7]` → `3 + 1 > 3`
    - Update:
      - `dp[7] = 4`
      - `prev[7] = 3`

  - Compare with `21` (Index 4): i = 7, j = 4

    - `21 < 60` but `dp[4] + 1 > dp[7]` → `2 + 1 > 4` → **No update**

  - Compare with `50` (Index 5): i = 7, j = 5

    - `50 < 60` and `dp[5] + 1 > dp[7]` → `4 + 1 > 4`
    - Update:
      - `dp[7] = 5`
      - `prev[7] = 5`
    
  - Compare with `41` (Index 6): i = 7, j = 6

    - `41 < 60` and `dp[6] + 1 > dp[7]` → `4 + 1 > 5`
    - Update:
      - `dp[7] = 5` (No change, since `dp[6] + 1 = 5` equals `dp[7]`)
  
- **`dp` now**: `{1, 2, 1, 3, 2, 4, 4, 5, 1}`

- **`prev` now**: `{-1, 0, -1, 1, 0, 3, 3, 5, -1}`

- **Update `maxLength` to `5` and `maxIndex` to `7`**

**Element at Index 8 (`80`)**

- Comparisons:

  - Compare with `10` (Index 0): i = 8, j = 0

    - `10 < 80` and `dp[0] + 1 > dp[8]` → `1 + 1 > 1`
    - Update:
      - `dp[8] = 2`
      - `prev[8] = 0`

  - Compare with `22` (Index 1): i = 8, j = 1

    - `22 < 80` and `dp[1] + 1 > dp[8]` → `2 + 1 > 2`
    - Update:
      - `dp[8] = 3`
      - `prev[8] = 1`

  - Compare with `9` (Index 2): i = 8, j = 2

    - `9 < 80` but `dp[2] + 1 > dp[8]` → `1 + 1 > 3` → **No update**

  - Compare with `33` (Index 3): i = 8, j = 3

    - `33 < 80` and `dp[3] + 1 > dp[8]` → `3 + 1 > 3`
    - Update:
      - `dp[8] = 4`
      - `prev[8] = 3`

  - Compare with `21` (Index 4): i = 8, j = 4

    - `21 < 80` but `dp[4] + 1 > dp[8]` → `2 + 1 > 4` → **No update**

  - Compare with `50` (Index 5): i = 8, j = 5

    - `50 < 80` and `dp[5] + 1 > dp[8]` → `4 + 1 > 4`
    - Update:
      - `dp[8] = 5`
      - `prev[8] = 5`
    
  - Compare with `41` (Index 6): i = 8, j = 6
  
    - `41 < 80` and `dp[6] + 1 > dp[8]` → `4 + 1 > 5`
  - Update:
      - `dp[8] = 5` (No change, since `dp[6] + 1 = 5` equals `dp[8]`)

  - Compare with `60` (Index 7): i = 8, j = 7

    - `60 < 80` and `dp[7] + 1 > dp[8]` → `5 + 1 > 5`
    - Update:
      - `dp[8] = 6`
      - `prev[8] = 7`
  
- **`dp` now**: `{1, 2, 1, 3, 2, 4, 4, 5, 6}`

- **`prev` now**: `{-1, 0, -1, 1, 0, 3, 3, 5, 7}`

- **Update `maxLength` to `6` and `maxIndex` to `8`**

**Step 3: Reconstructing the LIS**

Using the `prev` array, we trace back from the `maxIndex` to identify the LIS elements.

1. Start at Index 8 (`80`)
   - `prev[8] = 7`
   - **Element**: `80`
2. Move to Index 7 (`60`)
   - `prev[7] = 5`
   - **Element**: `60`
3. Move to Index 5 (`50`)
   - `prev[5] = 3`
   - **Element**: `50`
4. Move to Index 3 (`33`)
   - `prev[3] = 1`
   - **Element**: `33`
5. Move to Index 1 (`22`)
   - `prev[1] = 0`
   - **Element**: `22`
6. Move to Index 0 (`10`)
   - `prev[0] = -1` (End of subsequence)
   - **Element**: `10`

**Reversed Order of Elements**: `10`, `22`, `33`, `50`, `60`, `80`

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==16244== Memcheck, a memory error detector
==16244== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==16244== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==16244== Command: ./final
==16244== 
Longest Increasing Subsequence: 10 22 33 50 60 80 
Length of LIS is 6
Longest Increasing Subsequence: 2 5 7 101 
Length of LIS is 4
==16244== 
==16244== HEAP SUMMARY:
==16244==     in use at exit: 0 bytes in 0 blocks
==16244==   total heap usage: 5 allocs, 5 frees, 1,160 bytes allocated
==16244== 
==16244== All heap blocks were freed -- no leaks are possible
==16244== 
==16244== For lists of detected and suppressed errors, rerun with: -s
==16244== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

---

## C Implementation (O(n log n))

### Optimized LIS Approach Using Binary Search

- The classic Dynamic Programming (DP) approach for LIS runs in O(n²) time. 
- However, an optimized method leveraging Binary Search can achieve O(n log n) time complexity.

### **Algorithm Overview:**

1. **Tail Array:**
   - Maintain an array `tail[]` where `tail[i]` stores the smallest possible tail value of all increasing subsequences with length `i+1`.
   - Initially, `tail[]` is empty.
2. **Process Each Element:**
   - For each element `num` in the input array:
     - **Case 1:** If `num` is larger than the last element in `tail[]`, append it to `tail[]`.
     - **Case 2:** Otherwise, find the smallest element in `tail[]` that is greater than or equal to `num` using Binary Search and replace it with `num`. This ensures that `tail[]` always contains the smallest possible tails.
3. **Reconstruction (Optional):**
   - To reconstruct the actual LIS, maintain additional information such as predecessor indices.
4. **Result:**
   - The length of `tail[]` will be the length of the LIS.
   - If reconstruction is implemented, the actual LIS can be retrieved.

### **Why It Works:**

- By maintaining the smallest possible tail for each subsequence length, the algorithm ensures that subsequences remain extendable.
- Binary Search allows efficient insertion and replacement within `tail[]`, leading to the optimized time complexity.

#### Program Code

`hello.h`

```c
int binarySearch(int tail[], int l, int r, int key);
int LIS(int arr[], int n, int lis[]);
```

`hello.c`

```c
// Func to perform binary search
// It returns the index of the smallest number in tail[] >= key
// tail[] should be sorted in increasing order.
int binarySearch(int tail[], int l, int r, int key)
{
    // continue searching while the range has more than one element
    while (r - l > 1)
    {
        // calculate the middle index to avoid potential overflow
        int m = l + (r - l) / 2;
        
        // if the middle element is greater than or equal to the key,
        // narrow the search to the left half by updating the right boundary.
        if (tail[m] >= key)
        {
            r = m;
        }
        else
        {
            // if the middle element is less than the key,
            // narrow the search to the right half by updating the left boundary.
            l = m;
        }
    }
    
    // after the loop, check if the left boundary element is >= key.
    if (tail[l] >= key)
    {
        return l; // the left boundary is the desired position
    }
    return r;
}

// func to find LIS using Binary Search (O(n log n)) time
// arr[]: input array
// n: num of elements in arr[]
// lis[]: array to store the actual LIS elements
int LIS(int arr[], int n, int lis[])
{
    // allocate memory for temp arrays
    int *tail = malloc(n * sizeof(int));        // Stores the smallest tail of all increasing subsequences of length i+1.
    int *tailIndices = malloc(n * sizeof(int)); // Stores the predecessor of each element in the LIS.
    assert(tail != NULL);
    assert(tailIndices != NULL);

    // initialize
    tail[0] = arr[0];
    tailIndices[0] = 0;
    int size = 1;

    // Array to keep track of predecessors
    int *pred = malloc(n * sizeof(int));
    assert(pred != NULL);
    for (int i = 0; i < n; i++)
    {
        pred[i] = -1;
    }

    // iterate through the array to build tail, tailIndices and pred arrays.
    for (int i = 1; i < n; i++)
    {
        // if the current element is greater than the last element in tail[],
        // case 1: arr[i] extends the largest subsequence
        if (arr[i] > tail[size - 1])
        {
            pred[i] = tailIndices[size - 1]; // set predecessor
            tail[size] = arr[i];
            tailIndices[size] = i;
            size++;
        }
        // otherwise, it replaces the smallest element in tail[] that is >= the current element arr[i] to potentially form a better subseq.
        else
        {
            // case 2: Find the smallest element >= arr[i]
            // arr[i] will replace the first element in tail >= arr[i].
            int pos = binarySearch(tail, 0, size - 1, arr[i]);
            tail[pos] = arr[i];
            tailIndices[pos] = i;
            if (pos != 0)
            {
                pred[i] = tailIndices[pos - 1]; // set predecessor for arr[i]
            }
        }
    }

    // after processing all elements, traces back using the pred[] array to reconstruct the LIS
    int idx = tailIndices[size - 1]; // start from the last index of LIS.
    for (int i = size - 1; i >= 0; i--)
    {
        lis[i] = arr[idx]; // assign the current element to LIS
        idx = pred[idx];   // move to the predecessor
    }

    // free temp arrays
    free(tail);
    free(tailIndices);
    free(pred);

    return size; // return the length of the LIS.
}
```

`main.c`

```c
int main()
{
    int n;
    printf("Enter the number of elements in the array: ");
    scanf("%d", &n);

    if (n <= 0)
    {
        printf("Array must have at least one element.\n");
        return 0;
    }

    int arr[n];
    printf("Enter the elements of the array: \n");
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    // array to store the LIS
    int lis[n];
    int length = LIS(arr, n, lis);

    // print the length of LIS
    printf("Length of LIS: %d\n", length);

    // print the LIS
    printf("LIS: ");
    for (int i = 0; i < length; i++)
    {
        printf("%d ", lis[i]);
    }
    printf("\n");
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==18494== Memcheck, a memory error detector
==18494== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==18494== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==18494== Command: ./final
==18494== 
Enter the number of elements in the array: 9
Enter the elements of the array: 
10 22 9 33 21 50 41 60 80
Length of LIS: 6
LIS: 10 22 33 41 60 80 
==18494== 
==18494== HEAP SUMMARY:
==18494==     in use at exit: 0 bytes in 0 blocks
==18494==   total heap usage: 6 allocs, 6 frees, 2,192 bytes allocated
==18494== 
==18494== All heap blocks were freed -- no leaks are possible
==18494== 
==18494== For lists of detected and suppressed errors, rerun with: -s
==18494== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

#### Time and Space Complexity Analysis

##### **Time Complexity:**

- **Binary Search:** Each binary search operation takes **O(log n)** time.
- **Iterating Through Elements:** The algorithm processes each element once.
- **Overall Time Complexity:** **O(n log n)**, where `n` is the number of elements in the array.

##### **Space Complexity:**

- Arrays Used:
  - `tail[]`: **O(n)**
  - `tailIndices[]`: **O(n)**
  - `predecessors[]`: **O(n)**
  - `lis[]`: **O(n)**
- **Overall Space Complexity:** **O(n)**

---
