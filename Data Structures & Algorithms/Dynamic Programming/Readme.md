# Dynamic Programming

- **Dynamic Programming** is a general-purpose algorithm design technique that is most often used to solve combinatorial optimization problems, where we are looking for the best possible input to some function chosen from an exponentially large search space.
- **Dynamic Programming (DP)** is a method for solving complex problems by breaking them down into smaller **subproblems**. 
- Rather than solving the same subproblems repeatedly (as in naive recursion), DP **stores** or **reuses** the results of these subproblems, which speeds up the overall computation.

---

## Key Ideas Behind Dynamic Programming

1. **Optimal Substructure**
   A problem has **optimal substructure** if its solution can be built from the solutions to its subproblems.
   - Example: The **shortest path** from A to B in a graph can be built by concatenating shortest paths of sub-routes.
2. **Overlapping Subproblems**
   A problem has **overlapping subproblems** if the same subproblems occur multiple times in a naive recursive solution.
   - Example: Calculating the **n-th Fibonacci number** involves recalculating the same sub-Fibonacci numbers many times.

When both properties (optimal substructure and overlapping subproblems) are present, Dynamic Programming can be highly effective.

---

## Why Use Dynamic Programming?

- **Efficiency**: Reduces exponential-time naive solutions to polynomial time for many problems (by avoiding repeated computation).
- **Clarity**: Forces us to define a clear subproblem structure and recurrence relation.

---

## Common DP Applications

1. **Sequence / Subsequence Problems**
   - Longest Increasing Subsequence, Longest Common Subsequence.
2. **Knapsack / Optimization Problems**
   - 0/1 Knapsack, Unbounded Knapsack, Coin Change.
3. **Pathfinding / Grid Problems**
   - Minimum path sum or number of ways to traverse a grid.
4. **String Operations**
   - Edit Distance, palindrome-based problems.
5. **Graph Algorithms**
   - Shortest paths in certain contexts (Bellman-Ford, Floyd-Warshall), DAG-based dynamic programming.

---

## Two Common Approaches

1. **Top-Down (Memoization)**
   - We write a recursive solution as we normally would.
   - Whenever we compute a subproblem’s result, we **cache** (store) it in an array or dictionary.
   - Next time that subproblem is needed, we **reuse** the cached result instead of recomputing.
2. **Bottom-Up (Tabulation)**
   - We iteratively solve all the smaller subproblems first, storing results in a table (array).
   - We build up to the final answer by combining solutions from smaller to larger subproblems in a systematic way.
   - The bottom-up aspect of dynamic programming is most useful when a straight-forward recursion would produce many duplicate subproblems. 
   - It is most efficient when we can enumerate a class of subproblems that doesn't include too many extraneous cases that we don't need for our original problem.

---

## Simple Example: Fibonacci Numbers

Naive recursion:

F(n) = F(n - 1) + F(n - 2), with F(0) = 0, F(1) = 1

- **Overlapping Subproblems**: To compute F(5), we calculate F(4) and F(3). But to compute F(4), we again compute F(3) and F(2) and so on.

**Memoization** approach:

- Keep an array `memo[]`, where `memo[i]` stores F(i) once it’s computed.
- Before computing F(i), we check if `memo[i]` is already filled. If so, just return it.

**Tabulation** approach:

- Create an array `dp[]` of size `n + 1`.
- Initialize `dp[0] = 0` and `dp[1] = 1`.
- Iteratively fill `dp[i] = dp[i - 1] + dp[i - 2]` for `i` from 2 to `n`.

---

## C Implementations (Fibonacci)

### Top-Down (Memoization) Approach

**Key Idea**: Use a global or static array to **cache** the results of F(n) once calculated.

#### Program Code

`practice.h`

```c
extern int memo[1000];

int fib_memo(int n);
```

`functions.c`

```c
int fib_memo(int n){
    // base cases
    if(n == 0){
        return 0;
    }
    if(n == 1){
        return 1;
    }
    
    // if we've already computed fib(n), just return it
    if(memo[n] != -1){
        return memo[n];
    }
    
    // otherwise, compute and store in memo
    memo[n] = fib_memo(n - 1) + fib_memo(n - 2);
    return memo[n];
}
```

`practice.c` (Main)

```c
int memo[1000];
int main()
{
    // initialize memo array to -1
    for (int i = 0; i < 1000; i++)
    {
        memo[i] = -1;
    }

    int n = 10;
    int result = fib_memo(n);
    printf("fib_memo(%d) = %d\n", n, result);
    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
fib_memo(10) = 55
```

#### Big O Analysis

**A. Time Complexity**

- **Without Memoization**:
  - **Standard Recursive Fibonacci** has a time complexity of **O(2ⁿ)** because it recalculates the same Fibonacci numbers multiple times.
- **With Memoization**:
  - **Time Complexity:** **O(n)**
    - **Reasoning:**
      - Each Fibonacci number from `fib_memo(0)` to `fib_memo(n)` is computed **exactly once**.
      - Subsequent calls for the same `n` retrieve the result from the `memo` array in **constant time** (**O(1)**).
      - The function makes two recursive calls for `fib_memo(n - 1)` and `fib_memo(n - 2)` **only if** those values haven't been computed yet.
      - Thus, the total number of computations grows linearly with `n`.

**B. Space Complexity**

- **Space Complexity:** **O(n)**
  - **Reasoning:**
    - **Memo Array**: Uses an array `memo[1000]`, which is considered **O(n)** space relative to the input size `n`.
    - **Call Stack**: The depth of the recursive call stack is **O(n)** in the worst case (when computing `fib_memo(n)` without prior memoization).
    - Overall, both the memoization structure and the potential recursion depth contribute to **O(n)** space usage.

##### **4. Breakdown of Operations**

1. **Initialization (`main` Function)**:
   - **Loop**: Initializes `memo` array with `-1`.
   - **Complexity**: **O(n)**, where `n = 1000` in this context.
2. **`fib_memo(n)` Function Calls**:
   - **First Computation**: For each unique `n`, computes the Fibonacci number recursively.
   - **Subsequent Calls**: Retrieves the Fibonacci number from `memo` in **O(1)** time.
3. **Memoization Effect**:
   - **Eliminates Redundant Calculations**: Ensures that each Fibonacci number is computed only once.
   - **Reduces Time from Exponential to Linear**: Transforms the naive recursive solution's **O(2ⁿ)** time complexity to **O(n)** through memoization.

### Bottom-Up (Tabulation) Approach

**Key Idea**: Build up the solution from smaller subproblems to larger ones **iteratively**.

#### Program Code

`hello.h`

```c
int fib_tab(int n)
{
    if (n == 0)
    {
        return 0;
    }
    if (n == 1)
    {
        return 1;
    }

    int *dp = malloc((n + 1) * sizeof(int));
    dp[0] = 0;
    dp[1] = 1;

    for (int i = 2; i <= n; i++)
    {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    int result = dp[n];
    free(dp);
    return result;
}
```

`hello.c`

```c
int fib_tab(int n);
```

`main.c`

```c
int main()
{
    int n = 10;
    int result = fib_tab(n);
    printf("fib_tab(%d) = %d\n", n, result);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==7759== Memcheck, a memory error detector
==7759== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==7759== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==7759== Command: ./final
==7759== 
fib_tab(10) = 55
==7759== 
==7759== HEAP SUMMARY:
==7759==     in use at exit: 0 bytes in 0 blocks
==7759==   total heap usage: 2 allocs, 2 frees, 1,068 bytes allocated
==7759== 
==7759== All heap blocks were freed -- no leaks are possible
==7759== 
==7759== For lists of detected and suppressed errors, rerun with: -s
==7759== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

#### Big O Analysis

**Time Complexity** : O(n)

**Space Complexity**: O(n)

**`fib_tab(int n)` in `hello.c`**:

- **Time Complexity**: The function uses a single loop that runs from `2` to `n`, performing constant-time operations within each iteration. Hence, the time complexity is **O(n)**.
- **Space Complexity**: It allocates an array of size `n + 1` to store Fibonacci numbers, resulting in **O(n)** space usage.

---

## C Implementations (Knapsack)

**Problem Statement**: We have:

- `n` items, each with a **weight** `w[i]` and a **value** `v[i]`.
- A knapsack with a **capacity** `W`.

Maximize the total **value** of items we can fit into the knapsack **without exceeding** the capacity.

In simpler words:

- If the current item `i` doesn’t fit in the knapsack (because `w[i] > j`), skip it.
- Otherwise, take the **maximum** of **not taking** the item or **taking** the item + the best solution for the leftover capacity.

### Knapsack in C (Botton-Up DP)

#### Program Code

`hello.h`

```c
int knapsack(int W, int weights[], int values[], int n);
```

`hello.c`

```c
// solves the knapsack problem for a knapsack of capacity W, given n items with corresponding weights and values.
int knapsack(int W, int weights[], int values[], int n)
{
    // dp[i][w] will store the max value for capacity w using items up to i
    int **dp = malloc((n + 1) * sizeof(int *));
    for (int i = 0; i <= n; i++)
    {
        dp[i] = malloc((W + 1) * sizeof(int));
    }

    // Initialize dp array
    for (int i = 0; i <= n; i++)
    {
        for (int w = 0; w <= W; w++)
        {
            if (i == 0 || w == 0)
            {
                dp[i][w] = 0;
            }
            // if the current item's weight is less than or equal to w
            else if (weights[i - 1] <= w)
            {
                // either skip the item or take it
                int skip = dp[i - 1][w];
                int take = values[i - 1] + dp[i - 1][w - weights[i - 1]];
                // choose the option with the higher value
                dp[i][w] = (skip > take) ? skip : take;
            }
            else
            {
                // cannot take the item if weight is too large
                dp[i][w] = dp[i - 1][w];
            }
        }
    }

    // the answer is in dp[n][W]
    int result = dp[n][W];

    // free memory
    for (int i = 0; i <= n; i++)
    {
        free(dp[i]);
    }
    free(dp);

    return result;
}
```

`main.c`

```c
int main()
{
    int values[] = {60, 100, 120};
    int weights[] = {10, 20, 30};
    int W = 50;
    int n = sizeof(values) / sizeof(values[0]);

    int maxValue = knapsack(W, weights, values, n);
    printf("Maximum value in Knapsack: %d\n", maxValue);
    return 0;
}

```



#### Step-by-Step Program Execution

##### **Input Details**

- **Items:**
  - **Values**: `{60, 100, 120}`
  - **Weights**: `{10, 20, 30}`
- **Number of Items (n)**: `3`
- **Knapsack Capacity (W)**: `50`
- **Items:**
  1. **Item 1**: Weight = `10`, Value = `60`
  2. **Item 2**: Weight = `20`, Value = `100`
  3. **Item 3**: Weight = `30`, Value = `120`

##### **Dynamic Programming Table (`dp`)**

The `dp` table has dimensions `(n+1) x (W+1)`:

- **Rows (`i`)**: `0` to `3` (representing items)
- **Columns (`w`)**: `0` to `50` (representing weight capacities)

Each cell `dp[i][w]` will store the **maximum value achievable** with the first `i` items and a knapsack capacity of `w`.

#### **Initialization**

- **Base Cases:**
  - `dp[0][w] = 0` for all `w` (no items)
  - `dp[i][0] = 0` for all `i` (knapsack capacity `0`)

**Initial `dp` Table**:

| i/w  | 0    | 1    | 2    | ...  | 10   | ...  | 20   | ...  | 30   | ...  | 50   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | ...  | 0    | ...  | 0    | ...  | 0    | ...  | 0    |
| 1    | 0    |      |      |      |      |      |      |      |      |      |      |
| 2    | 0    |      |      |      |      |      |      |      |      |      |      |
| 3    | 0    |      |      |      |      |      |      |      |      |      |      |

**Item 1 (i = 1): Weight = 10, Value = 60**

**For each `w` from 1 to 50**:

- If `w < 10`:
  - `dp[1][w] = dp[0][w] = 0` (Cannot include Item 1)
- Else:
  - `dp[1][w] = max(dp[0][w], 60 + dp[0][w - 10])`
  - Since `dp[0][w - 10] = 0`, `dp[1][w] = max(0, 60) = 60`

**After Item 1**, the `dp` table looks like:

| i/w  | 0    | 10   | 20   | 30   | 40   | 50   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| 1    | 0    | 60   | 60   | 60   | 60   | 60   |
| 2    | 0    |      |      |      |      |      |
| 3    | 0    |      |      |      |      |      |

**Item 2 (i = 2): Weight = 20, Value = 100**

**For each `w` from 1 to 50**:

- If `w < 20`:
  - `dp[2][w] = dp[1][w]`
- Else:
  - `dp[2][w] = max(dp[1][w], 100 + dp[1][w - 20])`

**Calculations**:

| w    | `dp[1][w]` | `dp[1][w-20]` | `100 + dp[1][w-20]` | `dp[2][w] = max(dp(dp[1][w], 100 + dp[1][w-20]))` |
| ---- | ---------- | ------------- | ------------------- | ------------------------------------------------- |
| 1-19 | 0-60       | N/A           | N/A                 | Same as `dp[1][w]`                                |
| 20   | 60         | 0             | 100                 | 100                                               |
| 30   | 60         | 60            | 160                 | 160                                               |
| 40   | 60         | 60            | 160                 | 160                                               |
| 50   | 60         | 60            | 160                 | 160                                               |

**After Item 2**, the `dp` table updates as:

| i\w   | 0    | 10   | 20   | 30   | 40   | 50   |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- |
| **0** | 0    | 0    | 0    | 0    | 0    | 0    |
| **1** | 0    | 60   | 60   | 60   | 60   | 60   |
| **2** | 0    | 60   | 100  | 160  | 160  | 160  |
| **3** | 0    |      |      |      |      |      |

**Item 3 (i = 3): Weight = 30, Value = 120**

**For each `w` from 1 to 50**:

- If `w < 30`:
  - `dp[3][w] = dp[2][w]`
- Else:
  - `dp[3][w] = max(dp[2][w], 120 + dp[2][w - 30])`

**Calculations**:

| w    | `dp[2][w]`         | `dp[2][w - 30]` | `120 + dp[2][w - 30]` | `dp[3][w] = max(dp[2][w], 120 + dp[2][w - 30])` |
| ---- | ------------------ | --------------- | --------------------- | ----------------------------------------------- |
| 1-29 | Same as `dp[2][w]` | N/A             | N/A                   | Same as `dp[2][w]`                              |
| 30   | 160                | 0               | 120                   | 160                                             |
| 40   | 160                | 60            | 180                  | 180                                             |
| 50   | 160                | 100            | 220                   | 220                                             |

**After Item 3**, the `dp` table updates as:

| i\w   | 0    | 10   | 20   | 30   | 40   | 50   |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- |
| **0** | 0    | 0    | 0    | 0    | 0    | 0    |
| **1** | 0    | 60   | 60   | 60   | 60   | 60   |
| **2** | 0    | 60   | 100  | 160  | 160  | 160  |
| **3** | 0    | 60   | 100  | 160  | 180  | 220  |

------

**Final Result**

- **Maximum Value**: `dp[3][50] = 220`

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==8754== Memcheck, a memory error detector
==8754== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==8754== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==8754== Command: ./final
==8754== 
Maximum value in Knapsack: 220
==8754== 
==8754== HEAP SUMMARY:
==8754==     in use at exit: 0 bytes in 0 blocks
==8754==   total heap usage: 6 allocs, 6 frees, 1,872 bytes allocated
==8754== 
==8754== All heap blocks were freed -- no leaks are possible
==8754== 
==8754== For lists of detected and suppressed errors, rerun with: -s
==8754== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



#### Big O Analysis

**Time Complexity**

- O(n × W)
  - **n**: Number of items
  - **W**: Maximum weight capacity
  - **Reasoning:** The algorithm uses two nested loops:
    - The outer loop runs **n + 1** times (from `0` to `n`).
    - The inner loop runs **W + 1** times (from `0` to `W`).
  - **Total Operations**: Approximately `(n + 1) × (W + 1)` ⇒ **O(n × W)**.

**Space Complexity**

- O(n × W)
  - **Reasoning:**
    - A 2D array `dp[n + 1][W + 1]` is allocated to store the maximum values for each subproblem.
    - Each entry `dp[i][w]` represents the maximum value achievable with the first `i` items and a knapsack capacity of `w`.
  - **Total Space Used**: Proportional to the size of the `dp` array ⇒ **O(n × W)**.

---

# Conclusion

**In short**: **Dynamic Programming** is a technique to **optimize** recursive or step-by-step solutions by **systematically storing** the results of subproblems, ensuring each subproblem is solved **once** and then reused.

**Dynamic Programming** is a technique for:

- Breaking problems into **overlapping subproblems**
- Leveraging **optimal substructure**
- **Caching** or **building up** solutions to avoid repeated work

**Fibonacci** is the simplest demonstration of DP:

- **Top-Down** uses recursion + memo array.
- **Bottom-Up** uses an iterative approach to fill a table.

**Knapsack** shows DP in a more **real-world optimization** context:

- We build a 2D table where rows represent items and columns represent possible capacities.
- Each cell contains the **best** (maximum) value achievable.