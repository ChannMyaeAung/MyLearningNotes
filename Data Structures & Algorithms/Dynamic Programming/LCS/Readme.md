# Longest Common Subsequence (LCS)

- The **Longest Common Subsequence (LCS)** is a fundamental concept in computer science, particularly in areas like bioinformatics, text comparison, and version control systems.
- A **Longest Common Subsequence (LCS)** of two sequences (strings) is the longest sequence that appears in both sequences in the same order but not necessarily contiguously.

**Key Points:**

- **Subsequence vs. Substring:**
  - *Subsequence*: A sequence derived by deleting zero or more elements without changing the order of the remaining elements.
  - *Substring*: A contiguous sequence of characters within a string.
- **LCS Properties:**
  - The LCS is not necessarily unique; there can be multiple LCSs with the same maximum length.
  - The length of the LCS provides a measure of similarity between two sequences.

### Example

- **String A:** `AGGTAB`
- **String B:** `GXTXAYB`

**Finding the LCS:**

**Possible Common Subsequences:**

- `GTAB`
- `GTTAB`
- `GXTAB`
- `GXTAB`
- ... and others.

**Longest Common Subsequence:**

- `GTAB` with length **4**.

### Visualization

```css
String A: A G G T A B
String B: G X T X A Y B

LCS:      G   T   A B

```

---

## Dynamic Programming Approach to LCS

Dynamic Programming (DP) offers an efficient way to solve the LCS problem by breaking it down into simpler subproblems and storing their solutions to avoid redundant computations.

### Approach Overview

1. **Define the DP Table:**
   - Let `dp[i][j]` represent the length of the LCS of the first `i` characters of String A and the first `j` characters of String B.
2. **Initialization:**
   - If either string is empty (`i = 0` or `j = 0`), then `dp[i][j] = 0` because the LCS of an empty string with any string is `0`.
3. **Recurrence Relation:**
   - If the current characters match (`A[i-1] == B[j-1]`), then: `dp[i][j] = dp[i−1][j−1]+1`
   - If the current characters do not match, then: `dp[i][j] = max⁡(dp[i−1][j],dp[i][j−1])`
   - This ensures that we carry forward the maximum LCS length found so far.
4. **Filling the DP Table:**
   - Iterate through each character of both strings and fill the table based on the above rules.
5. **Result:**
   - The value `dp[m][n]` will contain the length of the LCS, where `m` and `n` are the lengths of Strings A and B, respectively.
6. **Reconstructing the LCS (Optional):**
   - To find the actual LCS string, trace back from `dp[m][n]` to `dp[0][0]`, choosing the path that led to the current cell.

### Example Walkthrough

**Strings:**

- **String A:** `AGGTAB` (Length: 6)
- **String B:** `GXTXAYB` (Length: 7)

**DP Table Construction:**

|      |      | G    | X    | T    | X    | A    | Y    | B    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|      | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| A    | 0    | 0    | 0    | 0    | 0    | 1    | 1    | 1    |
| G    | 0    | 1    | 1    | 1    | 1    | 1    | 1    | 1    |
| G    | 0    | 1    | 1    | 1    | 1    | 1    | 1    | 1    |
| T    | 0    | 1    | 1    | 2    | 2    | 2    | 2    | 2    |
| A    | 0    | 1    | 1    | 2    | 2    | 3    | 3    | 3    |
| B    | 0    | 1    | 1    | 2    | 2    | 3    | 3    | 4    |

**Explanation:**

- **Row 1 (A):**
  - **Column 1 (G):** `A` vs `G` → No match → `max(dp[0][1], dp[1][0]) = 0`
  - **Column 2 (X):** `A` vs `X` → No match → `0`
  - **Column 3 (T):** `A` vs `T` → No match → `0`
  - **Column 4 (X):** `A` vs `X` → No match → `0`
  - **Column 5 (A):** `A` vs `A` → Match → `dp[0][4] + 1 = 1`
  - **Column 6 (Y):** `A` vs `Y` → No match → `max(dp[1][5], dp[0][6]) = 1`
  - **Column 7 (B):** `A` vs `B` → No match → `max(dp[1][6], dp[0][7]) = 1`
- **Row 4 (T):**
  - **Column 1 (G):** `T` vs `G` → No match → `max(dp[4][1], dp[3][1]) = 1`
  - **Column 2 (X):** `T` vs `X` → No match → `max(dp[4][2], dp[3][2]) = 1`
  - **Column 3 (T):** `T` vs `T` → Match → `dp[3][2] + 1 = 2`
  - **Column 4 (X):** `T` vs `X` → No match → `max(dp[4][3], dp[3][4]) = 2`
  - **Column 5 (A):** `T` vs `A` → No match → `max(dp[4][4], dp[3][5]) = 2`
  - **Column 6 (Y):** `T` vs `Y` → No match → `max(dp[4][5], dp[3][6]) = 2`
  - **Column 7 (B):** `T` vs `B` → No match → `max(dp[4][6], dp[3][7]) = 2`
- **Cell `dp[6][7] = 4`:** Indicates the length of the LCS is 4.
- **Reconstruction:** By tracing back, we find that the LCS is `GTAB`.

---

## C Implementation of LCS Using Dynamic Programming

Below is a comprehensive C program that computes the Longest Common Subsequence (LCS) between two input strings using the dynamic programming approach. The program also reconstructs and prints the actual LCS.

### Features of the Implementation

- **Input:** Two strings from the user.
- **Output:** Length of the LCS and the LCS itself.
- **Reconstruction:** Uses the DP table to backtrack and find the LCS.
- **Modularity:** Functions are used to handle different parts of the algorithm for clarity.

#### Program Code

`hello.h`

```c
// Function to return the maximum of two integers.
// Used to determine whether to take the value from the top or left cell in the DP table.
int max(int a, int b);

// Function to print the Longest Common Subsequence by backtracking the DP table.
// Parameters:
// - dp[][100]: The DP table storing LCS lengths.
// - A[]: First input string.
// - B[]: Second input string.
// - i: Current index in string A.
// - j: Current index in string B.
void printLCS(int dp[][100], char A[], char B[], int i, int j);
```

`hello.c`

```c
// Function to return the maximum of two integers.
// Used to determine whether to take the value from the top or left cell in the DP table.
int max(int a, int b)
{
    return a > b ? a : b;
}

// Function to print the Longest Common Subsequence by backtracking the DP table.
// Parameters:
// - dp[][100]: The DP table storing LCS lengths.
// - A[]: First input string.
// - B[]: Second input string.
// - i: Current index in string A.
// - j: Current index in string B.
void printLCS(int dp[][100], char A[], char B[], int i, int j)
{
    // Base case: If either string is exhausted, stop recursion.
    if (i == 0 || j == 0)
    {
        return;
    }
    // if current characters at current indices match, it includes that character in the LCS and moves diagonally
    if (A[i - 1] == B[j - 1])
    {
        // Recursively move to the previous characters in both strings.
        printLCS(dp, A, B, i - 1, j - 1);
        // Print the matching character.
        printf("%c ", A[i - 1]);
    }
    // If characters do not match, determine the direction to move based on the DP table.
    else if (dp[i - 1][j] > dp[i][j - 1])
    {
        // Move in the direction of the larger value (either up or left)
        printLCS(dp, A, B, i - 1, j);
    }
    else
    {
        // Move left in the DP table.
        printLCS(dp, A, B, i, j - 1);
    }
}
```

`main.c`

```c
int main()
{
    char A[100], B[100];
    int dp[100][100];
    int m, n;

    // input two strings
    printf("Enter the first string(A): ");
    scanf("%s", A);
    printf("Enter the second string(B): ");
    scanf("%s", B);

    // calculate the length of the two strings
    m = strlen(A);
    n = strlen(B);

    // Build the DP table
    // Iterate over each character of A
    for (int i = 0; i <= m; i++)
    {
        // Iterate over each character of B
        for (int j = 0; j <= n; j++)
        {
            if (i == 0 || j == 0)
            {
                // Base case: LCS of empty string with any string is 0
                dp[i][j] = 0;
            }
            else if (A[i - 1] == B[j - 1])
            {
                // If characters match, increment the count from previous indices
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else
            {
                // If characters do not match, take the maximum of left and top cell
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    // print the DP table
    printf("\nDP Table:\n");
    printf("    ");
    for (int j = 0; j <= n; j++)
    {
        if (j > 0)
        {
            printf(" %c ", B[j - 1]); // Print column headers (characters of B)
        }
        else
        {
            printf("   "); // Initial padding for row headers
        }
    }
    printf("\n");

    for (int i = 0; i <= m; i++)
    {
        if (i > 0)
        {
            printf(" %c ", A[i - 1]); // Print row headers (characters of A)
        }
        else
        {
            printf("   "); // Initial padding for row headers
        }
        for (int j = 0; j <= n; j++)
        {
            printf("%2d ", dp[i][j]); // Print DP table values with padding
        }
        printf("\n");
    }

    // Length of LCS is dp[m][n]
    printf("\nLength of Longest Common Subsequence: %d\n", dp[m][n]);

    // print the LSC
    printf("Longest Common Subsequence: ");
    printLCS(dp, A, B, m, n);
    printf("\n");
    return 0;
}
```



#### Program Output

```sh
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==26159== Memcheck, a memory error detector
==26159== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==26159== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==26159== Command: ./final
==26159== 
Enter the first string(A): AGGTAB
Enter the second string(B): GXTXAYB

DP Table:
        G  X  T  X  A  Y  B 
    0  0  0  0  0  0  0  0 
 A  0  0  0  0  0  1  1  1 
 G  0  1  1  1  1  1  1  1 
 G  0  1  1  1  1  1  1  1 
 T  0  1  1  2  2  2  2  2 
 A  0  1  1  2  2  3  3  3 
 B  0  1  1  2  2  3  3  4 

Length of Longest Common Subsequence: 4
Longest Common Subsequence: G T A B 
==26159== 
==26159== HEAP SUMMARY:
==26159==     in use at exit: 0 bytes in 0 blocks
==26159==   total heap usage: 2 allocs, 2 frees, 2,048 bytes allocated
==26159== 
==26159== All heap blocks were freed -- no leaks are possible
==26159== 
==26159== For lists of detected and suppressed errors, rerun with: -s
==26159== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

#### Program Output #2

```sh
chan@CMA:~/C_Programming/test$ make valgrind
Compiling the main file
clang -std=c23 -Wall -Wextra -g -D_XOPEN_SOURCE=700 -c main.c -o ./obj/main.o
Compiling the hello file
clang -std=c23 -Wall -Wextra -g -D_XOPEN_SOURCE=700 -c hello.c -o ./obj/hello.o
Creating static library libhello.a
ar -rcs ./libs/libhello.a ./obj/hello.o
Linking and producing the final application
clang -std=c23 -Wall -Wextra -g -D_XOPEN_SOURCE=700 ./obj/main.o -L./libs -lhello -o final -lssl -lcrypto -lpthread -lm
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==28087== Memcheck, a memory error detector
==28087== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==28087== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==28087== Command: ./final
==28087== 
Enter the first string(A): ABCDGH
Enter the second string(B): AEDFHR

DP Table:
        A  E  D  F  H  R 
    0  0  0  0  0  0  0 
 A  0  1  1  1  1  1  1 
 B  0  1  1  1  1  1  1 
 C  0  1  1  1  1  1  1 
 D  0  1  1  2  2  2  2 
 G  0  1  1  2  2  2  2 
 H  0  1  1  2  2  3  3 

Length of Longest Common Subsequence: 3
Longest Common Subsequence: A D H 
==28087== 
==28087== HEAP SUMMARY:
==28087==     in use at exit: 0 bytes in 0 blocks
==28087==   total heap usage: 2 allocs, 2 frees, 2,048 bytes allocated
==28087== 
==28087== All heap blocks were freed -- no leaks are possible
==28087== 
==28087== For lists of detected and suppressed errors, rerun with: -s
==28087== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



## Time and Space Complexity

### Time Complexity

The dynamic programming approach to solving the LCS problem involves filling an `m * n` table, where `m` and `n` are the lengths of the two input strings. For each cell `dp[i][j]`, a constant amount of work is done.

- **Overall Time Complexity:** O(m * n)

### Space Complexity

The space required is mainly for storing the DP table, which has `m * n` cells.

- **Overall Space Complexity:** O(m * n)

---

## Conclusion & Applications

The **Longest Common Subsequence (LCS)** problem is a classic example where dynamic programming excels, providing an efficient solution to find the longest subsequence common to two strings. This has wide-ranging applications in areas like:

- **Bioinformatics:** Comparing genetic sequences.
- **Text Processing:** Diff tools to find differences between files.
- **Data Compression:** Identifying redundancy in data streams.

---

