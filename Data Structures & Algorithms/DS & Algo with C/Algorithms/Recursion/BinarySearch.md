### Binary Search: recursive and iterative versions

#### Recursive

`practice.h`

```C
int binarySearch(int target, const int *a, size_t length);
```

`functions.c`

```C
int binarySearch(int target, const int *a, size_t length){
    size_t index;
    index = length / 2;
    
    if(length == 0){
        // nothing left
        return 0;
    }else if(target == a[index]){
        // got it
        return 1;
    }else if(target < a[index]){
        //recurse on bottom half
        return binarySearch(target, a, index);
    }else{
        // recurse on top half
        // we throw away index + 1 elements (including a[index])
        return binarySearch(target, a + index + 1, length - (index + 1));
    }
}
```

**Parameters:**

- `int target`: The value we are searching for in the array.
- `const int *a`: A pointer to the first element of the sorted array where the search is performed.
- `size_t length`: The number of elements in the current subarray being searched.

**Explanation**:

**If the target is less than the middle element (`target < a[index]`):**

- The target must be in the **left half** of the array.
- Recursively call `binarySearch` on the left subarray: `a` remains the same, and `length` is updated to `index`.

**If the target is greater than the middle element (`target > a[index]`):**

- The target must be in the **right half** of the array.
- Recursively call `binarySearch` on the right subarray:
  - `a + index + 1`: Moves the pointer to the element right after the middle.
  - `length - (index + 1)`: Updates the length to exclude the left half and the middle element.

`practice.c`

```C
int main(int argc, char *argv[])
{
    // sorted array for binary search
    int array[] = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    size_t length = sizeof(array) / sizeof(array[0]);

    // targets to search for
    int targets[] = {5, 6, 1, 19, 20};
    size_t numTargets = sizeof(targets) / sizeof(targets[0]);

    // iterate thru each target and perform binary search
    for (size_t i = 0; i < numTargets; i++)
    {
        int target = targets[i];
        int found = binarySearch(target, array, length);

        if (found)
        {
            printf("Target %d found in the array.\n", target);
        }
        else
        {
            printf("Target %d not found in the array.\n", target);
        }
    }
    return 0;
}
```

**Execution Steps**

**Array:** `{1, 3, 5, 7, 9, 11, 13, 15, 17, 19}`

**Targets:** `{5, 6, 1, 19, 20}`

1. **Search for 5:**

   - Middle index: `10 / 2 = 5` → `a[5] = 11`
   - `5 < 11` → Search left subarray `{1, 3, 5, 7, 9}`
     - New middle index: `5 / 2 = 2` → `a[2] = 5`
     - `5 == 5` → Found (`1`)

2. **Search for 6:**

   - Middle index: `10 / 2 = 5` → `a[5] = 11`
   - `6 < 11`→ Search left subarray  `{1, 3, 5, 7, 9}`
     - New middle index: `5 / 2 = 2` → `a[2] = 5`
     - `6 > 5`→ Search right subarray `{7, 9}`
       - New middle index: `2 / 2 = 1` → `a[1] = 7`
       - `6 < 7` → Search left subarray `{7}`with length `0`(since `index = 1`)
         - Length `0` → Not found (`0`)

3. **Search for 1:**

   - Middle index: `10 / 2 = 5` → `a[5] = 11`
   - `1 < 11`→ Search left subarray  `{1, 3, 5, 7, 9}`
     - New middle index: `5 / 2 = 2` → `a[2] = 5`
     - `1 < 5`→ Search left subarray `{1, 3}`with length `2`
       - New middle index: `2 / 2 = 1` → `a[1] = 3`
       - `1 < 3` → Search left subarray `{1}`with length `1`
         - New middle index: `1 / 2 = 0` → `a[0] = 1`
         - `1 == 1` → Found (`1`)

4. **Search for 19:**

   - Middle index: `10 / 2 = 5` → `a[5] = 11`

   - `19 > 11`→ Search right subarray `{13, 15, 17, 19}`

     with length `5`

     - New middle index: `5 / 2 = 2` → `a[7] = 15`
     - `19 > 15` → Search right subarray `{17, 19}`with length `2`
       - New middle index: `2 / 2 = 1` → `a[9] = 19`
       - `19 == 19` → Found (`1`)

5. **Search for 20:**

   - Middle index: `10 / 2 = 5` → `a[5] = 11`
   - `20 > 11`→ Search right subarray `{13, 15, 17, 19}`with length `5`
     - New middle index: `5 / 2 = 2` → `a[7] = 15`
     - `20 > 15`→ Search right subarray `{17, 19}`with length `2`
       - New middle index: `2 / 2 = 1` → `a[9] = 19`
       - `20 > 19`→ Search right subarray `{}`with length`0`
         - Length `0` → Not found (`0`)

`Output`

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==14274== Memcheck, a memory error detector
==14274== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==14274== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==14274== Command: ./practice
==14274== 
Target 5 found in the array.
Target 6 not found in the array.
Target 1 found in the array.
Target 19 found in the array.
Target 20 not found in the array.
==14274== 
==14274== HEAP SUMMARY:
==14274==     in use at exit: 0 bytes in 0 blocks
==14274==   total heap usage: 1 allocs, 1 frees, 1,024 bytes allocated
==14274== 
==14274== All heap blocks were freed -- no leaks are possible
==14274== 
==14274== For lists of detected and suppressed errors, rerun with: -s
==14274== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

- This will work just fine, and indeed it finds the target element (or not) in O(log n) time, because we can only recurse O(log n) times before running out of elements and we only pay O(1) cost per recursive call to `binarySearch`. 
- But we do have to pay function call overhead for each recursive call, and there is a potential to run into stack overflow if our stack is very constrained.
- Fortunately, we don't do anything with the return value from `binarySearch` but pass it on up the stack; the function is tail-recursive. 
- This means that we can get rid of the recursion by reusing the stack from the initial call.
- The mechanical way to do this is wrap the body of the routine in a `for(;;)` loop (so that we jump back to the top whenever we hit the bottom), and replace each recursive cal with one or more assignments to update any parameters that change in the recursive call.

#### Iterative

**Example 1:**

```C
int binarySearchIterative(int target, const int *a, size_t length){
    size_t index;
    
    // the for(;;) loop creates an infinite loop that continues until the func returns a result (1 for found, 0 for not found).
    for(;;){
        index = length / 2;
        if(length == 0){
            return 0;
        }else if(target == a[index]){
            return 1;
        }else if(target < a[index]){
            // recurse on bottom half
            length = index;
        }else{
            //recurse on top half
            a = a + index + 1;
            length = length - (index + 1);
        }
    }
}
```

**Example 2:**

```C
int binarySearchIterative(int target, const int *a, size_t length)
{
    size_t left = 0;
    size_t right = length;

    while (left < right)
    {
        size_t mid = left + (right - left) / 2;
        if (a[mid] == target)
        {
            return 1;
        }
        else if (a[mid] < target)
        {
            left = mid + 1;
        }
        else
        {
            right = mid;
        }
    }
    return 0;
}
```

```C
int main(int argc, char *argv[])
{
    // sorted array for binary search
    int array[] = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    size_t length = sizeof(array) / sizeof(array[0]);

    // targets to search for
    int targets[] = {5, 6, 1, 19, 20};
    size_t numTargets = sizeof(targets) / sizeof(targets[0]);

    // iterate thru each target and perform binary search
    for (size_t i = 0; i < numTargets; i++)
    {
        int target = targets[i];
        int found = binarySearchIterative(target, array, length);

        if (found)
        {
            printf("Target %d found in the array (Iterative Search).\n", target);
        }
        else
        {
            printf("Target %d not found in the array (Iterative Search).\n", target);
        }
    }
    return 0;
}

```

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Target 5 found in the array (Iterative Search).
Target 6 not found in the array (Iterative Search).
Target 1 found in the array (Iterative Search).
Target 19 found in the array (Iterative Search).
Target 20 not found in the array (Iterative Search).
```

**Time Complexity:** O(log n), where n is the number of elements in the array.

**Space Complexity:**

- **Recursive Approach:** O(log n) due to the call stack.
- **Iterative Approach:** O(1), as it uses a constant amount of additional space.



#### Mergesort: a recursive sorting algorithm

- Here is an example of a recursive procedure that cannot be turned into an iterative version so easily.
- This is the mergesort algorithm, a classic **divide and conquer** sorting algorithm that splits an array into two pieces, sorts each piece (recursively!), then merges the results back together.

`practice.h`

- `n1`, `a1[]`: Size and elements of first subarray.
- `n2`, `a2[]`: Size and elements of second subarray.
- `out[]`: Array to store merged result.
- **Merging Logic**:
  - Initializes indices for both subarrays and output.
  - Continues until all elements from both subarrays are processed.
  - Compares current elements of `a1` and `a2`, appending the smaller to `out`.
  - Handles remaining elements in either subarray.

```C
// merges two sorted subarrays into a single sorted array
void merge(int n1, const int a1[], int n2, const int a2[], int out[]);

// recursively divides the array and sorts it using the merge function above
void mergeSort(int n, const int a[], int out[]);
```

`functions.c`

```C
// merge sorted arrays a1 and a2, putting result in out
void merge(int n1, const int a1[], int n2, const int a2[], int out[])
{
    int i1;
    int i2;
    int iout;
    i1 = i2 = iout = 0;
    while (i1 < n1 || i2 < n2)
    {
        if (i2 >= n2 || ((i1 < n1) && (a1[i1] < a2[i2])))
        {
            // a1[i1] exists and is smaller
            out[iout++] = a1[i1++];
        }
        else
        {
            // a1[i1] doesn't exists, or is bigger than a2[i2]
            out[iout++] = a2[i2++];
        }
    }
}

// sort a, putting resulting in out
// we call this mergeSort to avoid conflict with mergesort in libc
// n: Size of the array to sort.
// a[]: Input array.
// out[]: Array to store sorted result.
void mergeSort(int n, const int a[], int out[])
{
    int *a1;
    int *a2;

    // If the array size n is less than 2, copies a to out as it's already sorted.
    if (n < 2)
    {
        // 0 or 1 elements is already sorted
        memcpy(out, a, sizeof(int) * n);
    }
    else
    {
        // sort into temp arrays
        // splits the array into two halves
        a1 = malloc(sizeof(int) * (n / 2));
        a2 = malloc(sizeof(int) * (n - n / 2));

        // recursively sorts each half
        mergeSort(n / 2, a, a1);
        mergeSort(n - n / 2, a + n / 2, a2);

        // merge results
        // merge the sorted halves into the arg `out`
        merge(n / 2, a1, n - n / 2, a2, out);

        free(a1);
        free(a2);
    }
}
```

- `mergeSort()`:
- **Divide**:
  - Calculates midpoint `n/2`.
  - Allocates memory for two subarrays `a1` and `a2`.
  - Copies first half of `a` to `a1` and second half to `a2`.
- - **Conquer**:
    - Recursively calls `mergeSort` on both `a1` and `a2`.
  - **Combine**:
    - Merges the sorted subarrays `a1` and `a2` into `out` using the `merge` function.
  - **Cleanup**:
    - Frees allocated memory for `a1` and `a2` to prevent memory leaks.

`practice.c`

```C
#define N (20)

int main(int argc, char *argv[])
{
    int a[N];
    int b[N];
    int i;

    // initializes array a with values from 19 to 0.
    for (i = 0; i < N; i++)
    {
        a[i] = N - i - 1;
    }

    // print the original array
    for (i = 0; i < N; i++)
    {
        printf("%d ", a[i]);
    }

    putchar('\n');
	
    // call mergeSort to sort array a into array b
    mergeSort(N, a, b);
	
    // prints the sorted array
    for (i = 0; i < N; i++)
    {
        printf("%d ", b[i]);
    }
    putchar('\n');
    return 0;
}

```

1. Merge Sort Flowchart

  ```css
flowchart TD
    A[Start] --> B[Check if array size < 2]
    B -- Yes --> C[Return array]
    B -- No --> D[Divide array into two halves]
    D --> E[Recursively sort first half]
    E --> F[Recursively sort second half]
    F --> G[Merge the two sorted halves]
    G --> H[Return merged array]
    H --> I[End]
  ```

2. Recursive Division and Merging

  ```css
graph TD
    A[19,18,17,16,15,14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
    A --> B["19,18,17,16,15,14,13,12,11,10"]
    A --> C["9,8,7,6,5,4,3,2,1,0"]
    B --> D["19,18,17,16,15"]
    B --> E["14,13,12,11,10"]
    D --> F["19,18"]
    D --> G["17,16,15"]
    F --> H["19"]
    F --> I["18"]
    G --> J["17"]
    G --> K["16,15"]
    K --> L["16"]
    K --> M["15"]
    E --> N["14,13"]
    E --> O["12,11,10"]
    N --> P["14"]
    N --> Q["13"]
    O --> R["12,11"]
    O --> S["10"]
    R --> T["12"]
    R --> U["11"]
    C --> V["9,8,7,6,5"]
    C --> W["4,3,2,1,0"]
    V --> X["9,8"]
    V --> Y["7,6,5"]
    X --> Z["9"]
    X --> AA["8"]
    Y --> AB["7"]
    Y --> AC["6,5"]
    AC --> AD["6"]
    AC --> AE["5"]
    W --> AF["4,3"]
    W --> AG["2,1,0"]
    AF --> AH["4"]
    AF --> AI["3"]
    AG --> AJ["2,1"]
    AG --> AK["0"]
    AJ --> AL["2"]
    AJ --> AM["1"]
  ```

3. Merging Process

  ```css
flowchart TB
    A[Merge [19] and [18]] --> B[[18,19]]
    C[Merge [17] and [16,15]] --> D[[15,16,17]]
    B --> E[Merge [18,19] and [15,16,17]] --> F[[15,16,17,18,19]]
    G[Merge [14] and [13]] --> H[[13,14]]
    I[Merge [12] and [11,10]] --> J[[10,11,12]]
    H --> K[Merge [13,14] and [10,11,12]] --> L[[10,11,12,13,14]]
    F --> M[Merge [15,16,17,18,19] and [10,11,12,13,14]] --> N[[10,11,12,13,14,15,16,17,18,19]]
    O[Merge [9] and [8]] --> P[[8,9]]
    Q[Merge [7] and [6,5]] --> R[[5,6,7]]
    P --> S[Merge [8,9] and [5,6,7]] --> T[[5,6,7,8,9]]
    U[Merge [4] and [3]] --> V[[3,4]]
    W[Merge [2] and [1]] --> X[[1,2]]
    V --> Y[Merge [3,4] and [1,2]] --> Z[[1,2,3,4]]
    T --> AA[Merge [5,6,7,8,9] and [1,2,3,4]] --> AB[[1,2,3,4,5,6,7,8,9]]
    N --> AC[Merge [10,11,12,13,14,15,16,17,18,19] and [1,2,3,4,5,6,7,8,9]] --> AD[[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19]]
  ```

4. **Sorting** (`mergeSort`):

  - **First Call:** `mergeSort(20, a, b)`

    - Splits `a` into `a1[10] = [19, 18, ..., 10]` and `a2[10] = [9, 8, ..., 0]`.

    - `Recursively sorts a1 and a2`.

  - Subsequent Recursive Calls:

    - Each `mergeSort` call continues to split arrays until subarrays of size 1 are reached.
    - Example:
      - `mergeSort(10, a1, temp1)`
        - Splits into `temp1a[5]` and `temp1b[5]`.
        - Continues until single-element arrays.

  - **Merging**:

    - After reaching base cases, merges subarrays step-by-step.
    - Uses merge to combine sorted subarrays.
    - Merging progresses upward, combining larger sorted arrays.

  

  

  `Output`

```C
chan@CMA:~/C_Programming/practice$ ./practice
19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0 
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 
```

- The cost of this is pretty cheap: O(n log n), since each element of `a` is processed through `merge` once for each array it gets put in, and the recursion only goes O(log n) layers deep before we hit 1-element arrays.
- The reason that we can't easily transform this into an iterative version is that the `mergeSort` function is not tail-recursive.
- Not only does it call itself twice, but it also needs to free the temporary arrays at the end.
- Because the algorithm has to do these tasks on the way back up the stack, we need to keep the stack around to track them.