# Data Structures and programming techniques

## Asymptotic notation

- Asymptotic notation is a tool for measuring the growth rate of  functions, which for program design usually means the way in which the time or space costs of a program scale with the size of the input.



## Two sorting algorithms

- Sorting algorithms arrange elements in a list or array in a specific order—commonly in ascending or descending numerical order, or lexicographical (alphabetical) order for strings. 

- Efficient sorting is crucial for optimizing the performance of other algorithms (like search algorithms) and for tasks such as organizing data for readability.
- There are numerous sorting algorithms, each with its own strengths and weaknesses. **Selection Sort** and **Merge Sort** are two such algorithms that illustrate fundamental sorting techniques—**comparison-based sorting**.

### Selection sort

**Selection Sort** is a simple, comparison-based sorting algorithm. It divides the input list into two parts:

1. **Sorted Sublist:** Initially empty and built up from left to right.
2. **Unsorted Sublist:** Contains the remaining elements to be sorted.

The algorithm repeatedly selects the **minimum** (or maximum, depending on sorting order) element from the unsorted sublist and swaps it with the first unsorted element, effectively growing the sorted sublist.

#### Selection Sort Algorithm Steps

1. **Start with the first element** in the array as the initial position.
2. **Find the minimum element** in the unsorted sublist (from current position to the end).
3. **Swap** the found minimum element with the first element of the unsorted sublist.
4. **Move** the boundary between the sorted and unsorted sublists one element to the right.
5. **Repeat** steps 2-4 until the entire array is sorted.

#### Visualization

```
57123486
17523486  // swap 1 with 5 since 1 is the minimum value
12573486
12375486
12345786
12345687
12345678
```

#### C Implementation of Selection Sort

Below is a C program that implements Selection Sort to sort an array of integers in ascending order.

#### Program Code

```C
// Function to perform Selection Sort
void selectionSort(int arr[], int n)
{
    int i, j, min_idx, temp;

    // One by one move boundary of unsorted subarray
    for (i = 0; i < n - 1; i++)
    {
        // Assume the minimum is the first element
        min_idx = i;

        // Find the minimum element in the unsorted subarray
        for (j = i + 1; j < n; j++)
        {
            if (arr[j] < arr[min_idx])
            {
                min_idx = j;
            }
        }

        // Swap the found minimum element with the first element
        if (min_idx != i)
        { // Only swap if a smaller element was found
            temp = arr[i];
            arr[i] = arr[min_idx];
            arr[min_idx] = temp;
        }

        // Print the array after each iteration
        printf("Iteration %d: ", i + 1);
        for (int k = 0; k < n; k++)
        {
            printf("%d ", arr[k]);
        }
        printf("\n");
    }
}

// Function to print the array
void printArray(int arr[], int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

int main()
{
    int arr[] = {64, 25, 12, 22, 11};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original array: \n");
    printArray(arr, n);

    selectionSort(arr, n);
    printf("Sorted array: \n");
    printArray(arr, n);

    return 0;
}
```

#### Example Walkthrough

Let's walk through the Selection Sort process using the example array: `{64, 25, 12, 22, 11}`.

**Initial Array**:

```
64, 25, 12, 22, 11
```

**Iteration 1 (i = 0):**

- **Find Minimum:** The minimum element in `{64, 25, 12, 22, 11}` is `11`.

- **Swap:** Swap `64` with `11`.

- Array After Swap:

  ```
  11, 25, 12, 22, 64
  ```

**Iteration 2 (i = 1):**

- **Find Minimum:** The minimum element in `{25, 12, 22, 64}` is `12`.

- **Swap:** Swap `25` with `12`.

- **Array After Swap**:

  ```
  11, 12, 25, 22, 64
  ```

**Iteration 3 (i = 2):**

- **Find Minimum:** The minimum element in `{25, 22, 64}` is `22`.

- **Swap:** Swap `25` with `22`.

- **Array After Swap:**

  ```
  11, 12, 22, 25, 64
  ```

**Iteration 4 (i = 3):**

- **Find Minimum:** The minimum element in `{25, 64}` is `25`.

- **Swap:** No swap needed as `25` is already in place.

- **Array Remains:**

  ```
  11, 12, 22, 25, 64
  ```

**Final Sorted Array:**

```
11, 12, 22, 25, 64
```



#### Code Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Original array: 
64 25 12 22 11 
Iteration 1: 11 25 12 22 64 
Iteration 2: 11 12 25 22 64 
Iteration 3: 11 12 22 25 64 
Iteration 4: 11 12 22 25 64 
Sorted array: 
11 12 22 25 64 
```

#### Time and Space Complexity

- **Time Complexity**:
  - **Best Case:** O(n²)
  - **Average Case:** O(n²)
  - **Worst Case:** O(n²)
  - Selection Sort consistently performs O(n²) comparisons, making it inefficient for large datasets.
- **Space Complexity**:
  - Auxiliary Space: O(1)
  - It is an **in-place** sorting algorithm, requiring only a constant amount of additional memory.

#### Pros and Cons

**Pros:**

- **Simplicity:** Easy to understand and implement.
- **In-Place Sorting:** Doesn't require additional memory.
- **Performance Predictability:** Always performs O(n²) comparisons, regardless of the input.

**Cons:**

- **Inefficiency:** Poor performance on large lists due to quadratic time complexity.
- **Not Stable:** May not preserve the relative order of equal elements (can be modified to be stable).
- **Number of Swaps:** Minimizes the number of swaps (exactly n-1 swaps), but the overall inefficiency still outweighs this benefit.

### Merge Sort

**Merge Sort** is a **divide-and-conquer** algorithm that efficiently sorts large datasets. 

- It divides the unsorted list into smaller sublists until each sublist contains a single element. 
- Then, it repeatedly merges these sublists to produce new sorted sublists until there is only one sorted list remaining—the sorted array.

#### Merge Sort Algorithm Steps

1. **Divide:** Split the array into two halves.
2. **Conquer:** Recursively apply Merge Sort to both halves.
3. **Combine:** Merge the two sorted halves into a single sorted array.

#### Visualization

```
5 7 1 2 3 4 8 6
57 12 34 86
1257 3468
12345678
```

#### C implementation of Merge Sort

Below is a C program that implements Merge Sort to sort an array of integers in ascending order.

#### Program Code

```C
// Function to merge two halves of the array
void merge(int arr[], int left, int mid, int right)
{
    int n1 = mid - left + 1; // Number of elements in left subarray
    int n2 = right - mid;    // Number of elements in right subarray

    // Create temp arrays
    int *L = (int *)malloc(n1 * sizeof(int));
    int *R = (int *)malloc(n2 * sizeof(int));

    // Check for memory allocation error
    if (L == NULL || R == NULL)
    {
        fprintf(stderr, "Memory allocation error\n");
        exit(EXIT_FAILURE);
    }

    // Copy data to temp arrays L[] and R[]
    for (int i = 0; i < n1; i++)
    {
        L[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++)
    {
        R[j] = arr[mid + 1 + j];
    }

    // Merge the temp arrays back into arr[left..right]
    int i = 0;    // Initial index of first subarray
    int j = 0;    // Initial index of second subarray
    int k = left; // Initial index of merged subarray

    while (i < n1 && j < n2)
    {
        if (L[i] <= R[j])
        {
            arr[k] = L[i];
            i++;
        }
        else
        {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy the remaining elements of L[], if any
    while (i < n1)
    {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the remaining elements of R[], if any
    while (j < n2)
    {
        arr[k] = R[j];
        j++;
        k++;
    }

    free(L);
    free(R);
}

// Function to implement merge sort
void mergeSort(int arr[], int left, int right)
{
    if (left < right)
    {
        // Same as (left + right) / 2, but avoids overflow for large left and right
        int mid = left + (right - left) / 2;

        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}
void printArray(int arr[], int size)
{
    for (int i = 0; i < size; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
}
```

```C
int main()
{
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int arr_size = sizeof(arr) / sizeof(arr[0]);

    printf("Original array: \n");
    printArray(arr, arr_size);

    mergeSort(arr, 0, arr_size - 1);
    printf("Sorted array: \n");
    printArray(arr, arr_size);

    return 0;
}
```

#### Code Walkthrough

**3.1. Initial Setup**

- **Array:** `{38, 27, 43, 3, 9, 82, 10}`
- Indices:
  - `left = 0`
  - `right = 6` (since array size is 7)

**3.2. First Call to `mergeSort`**

```c
mergeSort(arr, 0, 6);
```

Since `left < right` (`0 < 6`), calculate `mid`:

```c
mid = 0 + (6 - 0) / 2 = 3
```

Now, recursively sort the first half (`left = 0` to `mid = 3`) and the second half (`mid + 1 = 4` to `right = 6`).

**3.3. Sorting the First Half: `mergeSort(arr, 0, 3)`**

Again, `left < right` (`0 < 3`), so calculate `mid`:

```c
mid = 0 + (3 - 0) / 2 = 1
```

Recursively sort the first half (`left = 0` to `mid = 1`) and the second half (`mid + 1 = 2` to `right = 3`).

**3.3.1. Sorting Subarray `mergeSort(arr, 0, 1)`**

- **Subarray:** `{38, 27}`
- `left = 0`, `right = 1`
- Calculate `mid`:

```c
mid = 0 + (1 - 0) / 2 = 0
```

Recursively sort `mergeSort(arr, 0, 0)` and `mergeSort(arr, 1, 1)`.

**3.3.1.1. Base Cases**

- `mergeSort(arr, 0, 0)`: `left == right` (`0 == 0`) → Return immediately.
- `mergeSort(arr, 1, 1)`: `left == right` (`1 == 1`) → Return immediately.

**3.3.1.2. Merging `{38}` and `{27}`**

Call `merge(arr, 0, 0, 1)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{38}`
- **Right Subarray (`R`):** `{27}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 0` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 38` and `R[0] = 27`.
2. Since `27 < 38`, set `arr[0] = 27`.
3. Increment `j` and `k`.
4. No elements left in `R`, so copy remaining `L[0] = 38` to `arr[1]`.

**Array After Merge:** `{27, 38, 43, 3, 9, 82, 10}`

**Temporary Arrays Freed:** `L` and `R`.

**3.3.2. Sorting Subarray `mergeSort(arr, 2, 3)`**

- **Subarray:** `{43, 3}`
- `left = 2`, `right = 3`
- Calculate `mid`:

```C
mid = 2 + (3 - 2) / 2 = 2
```

Recursively sort `mergeSort(arr, 2, 2)` and `mergeSort(arr, 3, 3)`.

**3.3.2.1. Base Cases**

- `mergeSort(arr, 2, 2)`: `left == right` (`2 == 2`) → Return immediately.
- `mergeSort(arr, 3, 3)`: `left == right` (`3 == 3`) → Return immediately.

**3.3.2.2. Merging `{43}` and `{3}`**

Call `merge(arr, 2, 2, 3)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{43}`
- **Right Subarray (`R`):** `{3}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 2` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 43` and `R[0] = 3`.
2. Since `3 < 43`, set `arr[2] = 3`.
3. Increment `j` and `k`.
4. No elements left in `R`, so copy remaining `L[0] = 43` to `arr[3]`.

**Array After Merge:** `{27, 38, 3, 43, 9, 82, 10}`

**Temporary Arrays Freed:** `L` and `R`.

**3.3.3. Merging `{27, 38}` and `{3, 43}`**

Call `merge(arr, 0, 1, 3)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{27, 38}`
- **Right Subarray (`R`):** `{3, 43}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 0` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 27` and `R[0] = 3`.
2. Since `3 < 27`, set `arr[0] = 3`.
3. Increment `j` and `k`.
4. Compare `L[0] = 27` and `R[1] = 43`.
5. Since `27 < 43`, set `arr[1] = 27`.
6. Increment `i` and `k`.
7. Compare `L[1] = 38` and `R[1] = 43`.
8. Since `38 < 43`, set `arr[2] = 38`.
9. Increment `i` and `k`.
10. No elements left in `L`, so copy remaining `R[1] = 43` to `arr[3]`.

**Array After Merge:** `{3, 27, 38, 43, 9, 82, 10}`

**Temporary Arrays Freed:** `L` and `R`.

**3.4. Sorting the Second Half: `mergeSort(arr, 4, 6)`**

- **Subarray:** `{9, 82, 10}`
- `left = 4`, `right = 6`
- Calculate `mid`:

```c
mid = 4 + (6 - 4) / 2 = 5
```

Recursively sort `mergeSort(arr, 4, 5)` and `mergeSort(arr, 6, 6)`.

**3.4.1. Sorting Subarray `mergeSort(arr, 4, 5)`**

- **Subarray:** `{9, 82}`
- `left = 4`, `right = 5`
- Calculate `mid`:

```c
mid = 4 + (5 - 4) / 2 = 4
```

Recursively sort `mergeSort(arr, 4, 4)` and `mergeSort(arr, 5, 5)`.

**3.4.1.1. Base Cases**

- `mergeSort(arr, 4, 4)`: `left == right` (`4 == 4`) → Return immediately.
- `mergeSort(arr, 5, 5)`: `left == right` (`5 == 5`) → Return immediately.

**3.4.1.2. Merging `{9}` and `{82}`**

Call `merge(arr, 4, 4, 5)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{9}`
- **Right Subarray (`R`):** `{82}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 4` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 9` and `R[0] = 82`.
2. Since `9 < 82`, set `arr[4] = 9`.
3. Increment `i` and `k`.
4. No elements left in `L`, so copy remaining `R[0] = 82` to `arr[5]`.

**Array After Merge:** `{3, 27, 38, 43, 9, 82, 10}`

**Temporary Arrays Freed:** `L` and `R`.

**3.4.2. Sorting Subarray `mergeSort(arr, 6, 6)`**

- **Subarray:** `{10}`
- `left = 6`, `right = 6` → `left == right` → Return immediately.

**3.4.3. Merging `{9, 82}` and `{10}`**

Call `merge(arr, 4, 5, 6)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{9, 82}`
- **Right Subarray (`R`):** `{10}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 4` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 9` and `R[0] = 10`.
2. Since `9 < 10`, set `arr[4] = 9`.
3. Increment `i` and `k`.
4. Compare `L[1] = 82` and `R[0] = 10`.
5. Since `10 < 82`, set `arr[5] = 10`.
6. Increment `j` and `k`.
7. No elements left in `R`, so copy remaining `L[1] = 82` to `arr[6]`.

**Array After Merge:** `{3, 27, 38, 43, 9, 10, 82}`

**Temporary Arrays Freed:** `L` and `R`.

**3.5. Final Merge: `merge(arr, 0, 3, 6)`**

Call `merge(arr, 0, 3, 6)`.

**Inside `merge`:**

- **Left Subarray (`L`):** `{3, 27, 38, 43}`
- **Right Subarray (`R`):** `{9, 10, 82}`
- Pointers:
  - `i = 0` (for `L`)
  - `j = 0` (for `R`)
  - `k = 0` (for merging into `arr`)

**Merging Steps:**

1. Compare `L[0] = 3` and `R[0] = 9`. Set `arr[0] = 3`. Increment `i` and `k`.
2. Compare `L[1] = 27` and `R[0] = 9`. Set `arr[1] = 9`. Increment `j` and `k`.
3. Compare `L[1] = 27` and `R[1] = 10`. Set `arr[2] = 10`. Increment `j` and `k`.
4. Compare `L[1] = 27` and `R[2] = 82`. Set `arr[3] = 27`. Increment `i` and `k`.
5. Compare `L[2] = 38` and `R[2] = 82`. Set `arr[4] = 38`. Increment `i` and `k`.
6. Compare `L[3] = 43` and `R[2] = 82`. Set `arr[5] = 43`. Increment `i` and `k`.
7. No elements left in `L`, so copy remaining `R[2] = 82` to `arr[6]`.

**Final Sorted Array:** `{3, 9, 10, 27, 38, 43, 82}`

**Temporary Arrays Freed:** `L` and `R`.

```css
Initial Array:
{38, 27, 43, 3, 9, 82, 10}

First Level of Division:
Left Half: {38, 27, 43, 3}
Right Half: {9, 82, 10}

Second Level of Division:
Left Subarray: {38, 27}
Right Subarray: {43, 3}

Third Level of Division:
Left Subarray: {38}
Right Subarray: {27}
Merging: {27, 38}

Left Subarray: {43}
Right Subarray: {3}
Merging: {3, 43}

Merging: {3, 27, 38, 43}

Second Half Division:
Left Subarray: {9, 82}
Right Subarray: {10}

Merging: {9, 82}

Merging: {9, 10, 82}

Final Merge:
{3, 27, 38, 43} and {9, 10, 82}
Result: {3, 9, 10, 27, 38, 43, 82}

```



#### Example Walkthrough:

Let's walk through the Merge Sort process using the example array: `{38, 27, 43, 3, 9, 82, 10}`.

**Initial Array**:

```
38, 27, 43, 3, 9, 82, 10
```

**Step 1: Divide**

- **First Division:**
  - Split into `{38, 27, 43, 3}` and `{9, 82, 10}`
- **Second Division:**
  - Split `{38, 27, 43, 3}` into `{38, 27}` and `{43, 3}`
  - Split `{9, 82, 10}` into `{9, 82}` and `{10}`
- **Third Division:**
  - Split `{38, 27}` into `{38}` and `{27}`
  - Split `{43, 3}` into `{43}` and `{3}`
  - Split `{9, 82}` into `{9}` and `{82}`
  - `{10}` remains as is.

**Step 2: Conquer (Sort Each Subarray)**

- `{38}` and `{27}` are single elements—already sorted.
- `{43}` and `{3}` are single elements—already sorted.
- `{9}` and `{82}` are single elements—already sorted.

**Step 3: Combine (Merge Subarrays)**

- **Merge `{38}` and `{27}`:**
  - Compare `38` and `27`. Since `27 < 38`, place `27` first.
  - Result: `{27, 38}`
- **Merge `{43}` and `{3}`:**
  - Compare `43` and `3`. Since `3 < 43`, place `3` first.
  - Result: `{3, 43}`
- **Merge `{27, 38}` and `{3, 43}`:**
  - Compare `27` and `3`. Place `3` first.
  - Compare `27` and `43`. Place `27` next.
  - Compare `38` and `43`. Place `38` next.
  - Place remaining `43`.
  - Result: `{3, 27, 38, 43}`
- **Merge `{9}` and `{82}`:**
  - Compare `9` and `82`. Place `9` first.
  - Place remaining `82`.
  - Result: `{9, 82}`
- **Merge `{9, 82}` and `{10}`:**
  - Compare `9` and `10`. Place `9` first.
  - Compare `82` and `10`. Place `10` next.
  - Place remaining `82`.
  - Result: `{9, 10, 82}`
- **Final Merge: `{3, 27, 38, 43}` and `{9, 10, 82}`:**
  - Compare `3` and `9`. Place `3` first.
  - Compare `27` and `9`. Place `9` next.
  - Compare `27` and `10`. Place `10` next.
  - Compare `27` and `82`. Place `27` next.
  - Compare `38` and `82`. Place `38` next.
  - Compare `43` and `82`. Place `43` next.
  - Place remaining `82`.
  - Result: `{3, 9, 10, 27, 38, 43, 82}`

**Final Sorted Array**:

```
3, 9, 10, 27, 38, 43, 82
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Original array: 
38 27 43 3 9 82 10 
Sorted array: 
3 9 10 27 38 43 82 
```

#### Time and Space Complexity

- **Time Complexity**:
  - **Best Case:** O(n log n)
  - **Average Case:** O(n log n)
  - **Worst Case:** O(n log n)
  - Merge Sort consistently performs O(n log n) operations, making it much more efficient than Selection Sort for large datasets.
- **Space Complexity**:
  - **Auxiliary Space: O(n)**
  - It requires additional memory proportional to the size of the input array due to the temporary arrays used during the merge process.

#### Pros and Cons

**Pros:**

- **Efficiency:** Significantly faster on large datasets compared to simple algorithms like Selection Sort.
- **Stable Sort:** Preserves the relative order of equal elements, which is beneficial when the order carries meaning.
- **Predictable Performance:** Consistent O(n log n) time complexity regardless of input.

**Cons:**

- **Space Consumption:** Requires additional memory for temporary arrays, which can be a drawback for very large datasets.
- **Implementation Complexity:** More complex to implement compared to simpler algorithms like Selection Sort.



### Comparison: Merge Sort vs. Selection Sort

| Feature          | Selection Sort           | Merge Sort                   |
| ---------------- | ------------------------ | ---------------------------- |
| Time Complexity  | O(n²)                    | O(n log n)                   |
| Space Complexity | O(1) (in-place)          | O(n) (not in place)          |
| Stability        | Not stable (can be made) | Stable                       |
| Divide & Conquer | No                       | Yes                          |
| Best Use Case    | Small datasets           | Large datasets, linked lists |
| Implementation   | Simple                   | More complex                 |
| Number of Swaps  | O(n)                     | O(n log n)                   |



### **Key Takeaways:**

- **Selection Sort** is straightforward and requires minimal additional memory, making it suitable for small or nearly sorted datasets where its inefficiency is less impactful.
- **Merge Sort** is highly efficient for large datasets and ensures stability, but it requires additional memory and a more complex implementation.

---

## Big O Notation

- "How code slows as data grows".

- Describes the performance of an algorithm as the amount of data increases.

- The idea of "asymptotic notation" is to consider the shape of the worst-case cost T(n) to process an input of size `n`.

- Here, worst-case means we consider the input that gives the greatest cost, where cost is usually time, but may be something else like space.

- **Big O Notation** is a mathematical representation used to describe the **upper bound** of an algorithm's running time or space requirements in terms of the size of its input (denoted as *n*). 

  - It abstracts away constants and lower-order terms to focus on the algorithm's growth rate as *n* increases.
  - **Time Complexity:** How the running time increases with input size.
  - **Space Complexity:** How the memory usage increases with input size.

- **Key Point:** Big O describes the **worst-case scenario** for an algorithm's growth rate.

- Common Big O complexities from fastest to slowest are:

  1. **O(1):** Constant time
  2. **O(log n):** Logarithmic time
  3. **O(n):** Linear time
  4. **O(n log n):** Linearithmic time
  5. **O(n²):** Quadratic time
  6. **O(2ⁿ):** Exponential time
  7. **O(n!):** Factorial time

  ![Screenshot from 2024-12-01 21-35-57](/home/chan/Downloads/Screenshot from 2024-12-01 21-35-57.png)

### Why is Big O Important?

1. **Performance Prediction:** Helps estimate how an algorithm scales with larger inputs.
2. **Algorithm Comparison:** Allows comparison between different algorithms' efficiencies.
3. **Optimization Guidance:** Identifies bottlenecks and areas for improvement.
4. **Resource Management:** Assists in choosing algorithms that fit memory and time constraints.

### Common Big O Classes

#### O(1) - Constant Time

- **Definition**: The running time **does not change** with the input size.

- **Characteristics**: Single opeartion regardless of n.

- **Example Operations:**

  - Accessing an array element by index.
  - Assigning a value to a variable.
  - Inserting at the beginning of linkedlist. 

- Example:

  ```C
  int getElement(int arr[], int index)
  {
      return arr[index]; // O(1)
  }
  
  int main()
  {
      int myArray[100];
      for (int i = 0; i < 100; i++)
      {
          myArray[i] = i;
      }
      printf("Element at index 50: %d\n", getElement(myArray, 50));
      return 0;
  }
  ```

- **Explanation**:

  - `getElement` retrieves an element at a specific index.
  - **Time Complexity**: O(1) because it performs a single operation regardless of array size.

#### O(log n) - Logarithmic Time

- **Definition**: The running time increases **logarithmically** as the input size increases.

- **Characteristics**: Reduces the problem size by a constant factor each step.

- An algorithm is logarithmic if it halves the input size at each step.

- **Example Operations:**

  - Binary search in a sorted array.
  - Certain tree operations (e.g., balanced binary search trees).

- **Example**:

  ```C
  int binarySearch(int arr[], int left, int right, int target)
  {
      while (left <= right)
      {                                        // O(log n)
          int mid = left + (right - left) / 2; // O(1)
          if (arr[mid] == target)
          {
              return mid; // O(1)
          }
  
          if (arr[mid] < target)
          {
              left = mid + 1; // O(1)
          }
          else
          {
              right = mid - 1; // O(1)
          }
      }
      return -1; // O(1)
  }
  
  int main()
  {
      int myArray[7] = {2, 3, 4, 10, 40, 55, 100};
      int target = 10;
      int result = binarySearch(myArray, 0, 6, target);
      if (result != -1)
      {
          printf("Element found at index %d\n", result);
      }
      else
      {
          printf("Element not found.\n");
      }
  
      return 0;
  }
  ```

  ```shell
  chan@CMA:~/C_Programming/test$ ./final
  Element found at index 3
  ```

- **Explanation:**

  - `binarySearch` divides the search interval in half each step.
  - **Time Complexity:** O(log n) because the search space reduces exponentially.

#### O(n) - Linear Time

- **Definition:** The running time increases **linearly** with the input size.
- **Characteristics:** Single loop through the data.

- **Example Operations:**

  - Looping through elements in an array.
  - Finding the maximum element in an array.
  - Searching through a LinkedList.

  - Simple linear search.

- **Example**:

  ```C
  int findMax(int arr[], int n)
  {
      int max = arr[0]; // O(1)
      for (int i = 1; i < n; i++)
      { // O(n)
          if (arr[i] > max)
          {
              max = arr[i]; // O(1)
          }
      }
      return max; // O(1)
  }
  
  int main()
  {
      int myArray[5] = {3, 1, 4, 1, 5};
      printf("Maximum element: %d\n", findMax(myArray, 5));
      return 0;
  }
  ```

  ```shell
  chan@CMA:~/C_Programming/test$ ./final
  Maximum element: 9
  ```

- **Explanation:**

  - `findMax` iterates through each element to find the maximum.
  - **Time Complexity:** O(n) because the loop runs *n* times.

#### O(n log n) - Linearithmic Time

- **Definition:** The running time increases as **n multiplied by log n**.

- **Characteristics:** Combines linear and logarithmic growth rates.

- An algorithm is logarithmic if it halves the input size at each step.

- **Example Operations:**

  - Efficient sorting algorithms like Merge Sort, Heap Sort and Quick Sort.
  - Certain divide-and-conquer algorithms.

- **Example**:

  ```C
  // Function to merge two halves of the array
  void merge(int arr[], int left, int mid, int right)
  {
      int n1 = mid - left + 1; // Number of elements in left subarray
      int n2 = right - mid;    // Number of elements in right subarray
  
      // Create temp arrays
      int *L = (int *)malloc(n1 * sizeof(int));
      int *R = (int *)malloc(n2 * sizeof(int));
  
      // Check for memory allocation error
      if (L == NULL || R == NULL)
      {
          fprintf(stderr, "Memory allocation error\n");
          exit(EXIT_FAILURE);
      }
  
      // Copy data to temp arrays L[] and R[]
      for (int i = 0; i < n1; i++)
      {
          L[i] = arr[left + i];
      }
      for (int j = 0; j < n2; j++)
      {
          R[j] = arr[mid + 1 + j];
      }
  
      // Merge the temp arrays back into arr[left..right]
      int i = 0;    // Initial index of first subarray
      int j = 0;    // Initial index of second subarray
      int k = left; // Initial index of merged subarray
  
      while (i < n1 && j < n2)
      {
          if (L[i] <= R[j])
          {
              arr[k] = L[i];
              i++;
          }
          else
          {
              arr[k] = R[j];
              j++;
          }
          k++;
      }
  
      // Copy the remaining elements of L[], if any
      while (i < n1)
      {
          arr[k] = L[i];
          i++;
          k++;
      }
  
      // Copy the remaining elements of R[], if any
      while (j < n2)
      {
          arr[k] = R[j];
          j++;
          k++;
      }
  
      free(L);
      free(R);
  }
  
  // Function to implement merge sort
  void mergeSort(int arr[], int left, int right)
  {
      if (left < right)
      {
          // Same as (left + right) / 2, but avoids overflow for large left and right
          int mid = left + (right - left) / 2;
  
          // Sort first and second halves
          mergeSort(arr, left, mid);
          mergeSort(arr, mid + 1, right);
  
          // Merge the sorted halves
          merge(arr, left, mid, right);
      }
  }
  void printArray(int arr[], int size)
  {
      for (int i = 0; i < size; i++)
      {
          printf("%d ", arr[i]);
      }
      printf("\n");
  }
  
  int main()
  {
      int myArray[6] = {12, 11, 13, 5, 6, 7};
      int arr_size = sizeof(myArray) / sizeof(myArray[0]);
  
      printf("Original array: \n");
      for (int i = 0; i < arr_size; i++)
      {
          printf("%d ", myArray[i]);
      }
      printf("\n");
  
      mergeSort(myArray, 0, arr_size - 1);
  
      printf("Sorted array: \n");
      for (int i = 0; i < arr_size; i++)
      {
          printf("%d ", myArray[i]);
      }
      printf("\n");
      return 0;
  }
  ```

  ```shell
  chan@CMA:~/C_Programming/test$ ./final
  Original array: 
  12 11 13 5 6 7 
  Sorted array: 
  5 6 7 11 12 13 
  ```

- **Explanation:**

  - `mergeSort` recursively splits the array and merges sorted halves.
  - **Time Complexity:** O(n log n) because it divides the array log n times and merges in linear time each division.

#### O(n²) - Quadratic Time

- **Definition:** The running time increases **quadratically** with the input size.
- **Characteristics:** Nested loops, each running *n* times.

- **Example Operations:**

  - Bubble Sort, Selection Sort, and Insertion Sort.

  - Checking all pairs in a dataset.

- **Example**: Selection Sort

  ```C
  void selectionSort(int arr[], int n){
      int i, j, min_idx, temp;
      
      for(i = 0; i < n - 1; i++){
          min_idx = i;
          for(j = i + 1; j < n; j++){
              if(arr[j] < arr[min_idx]){
                  min_idx = j;
              }
          }
          
          // Swap if min_idx changed
          if(min_idx != i){
              temp = arr[i];
              arr[i] = arr[min_idx];
              arr[min_idx] = temp;
          }
      }
  }
  
  int main()
  {
      int myArray[5] = {64, 25, 12, 22, 11};
      int arr_size = sizeof(myArray) / sizeof(myArray[0]);
  
      selectionSort(myArray, arr_size);
      printf("Sorted array: \n");
      for (int i = 0; i < arr_size; i++)
      {
          printf("%d ", myArray[i]);
      }
      printf("\n");
      return 0;
  }
  ```

  ```shell
  chan@CMA:~/C_Programming/test$ ./final
  Sorted array: 
  11 12 22 25 64 
  ```

  

#### O(2<sup>n</sup>) - Exponential Time Complexity

- The execution time doubles with each additional input element.

- **Example**:

  ```C
  int fibonacci(int n)
  {
      if (n <= 1)
      {
          return n;
      }
      else
      {
          return fibonacci(n - 1) + fibonacci(n - 2);
      }
  }
  ```

- **Explanation:** The function makes two recursive calls for each call, leading to exponential growth.

#### O(n!) - Factorial Time Complexity

- Time complexity grows factorially with input size - extremely inefficient for large `n`.
- **Example**: Solving the Traveling Salesman Problem via brute-force
- **Explanation:** Calculating all possible permutations of routes.

### Asymptotic cost of programs

- To compute the asymptotic cost of a program, the rule of thumb is that any simple statement costs O(1) time to evaluate, and larger costs are the result of loops or calls to expensive functions, where a loop multiplies the cost by the number of iterations in the loop.
- When adding costs together, the biggest cost wins:

This function takes O(1) time:

```C
int sumTo(int n){
    return n*(n - 1) / 2;
}

int main(){
    printf("SumTo: %d\n", sumTo(10));
    return 0;
}
```

```shell
chan@CMA:~/C_Programming/test$ ./final
SumTo: 45
```

But this function, which computes exactly the same value, takes O(n) time:

```C
int sumTo(int n)
{
    int i;
    int sum = 0;

    for (i = 0; i < n; i++)
    {
        sum += i;
    }
    return sum;
}
int main()
{
    printf("SumTo: %d\n", sumTo(10));
    return 0;
}
```

```shell
chan@CMA:~/C_Programming/test$ ./final
SumTo: 45
```

- The reason it takes so long is that each iteration of the loop takes only O(1) time, but we execute the loop n times, and n . O(1) = O(n).

Here's an even worse version that takes O(n<sup>2</sup>) time:

```C
int sumTo(int n)
{
    int i;
    int j;
    int sum = 0;

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < i; j++)
        {
            sum++;
        }
    }
    return sum;
}
int main()
{
    printf("SumTo: %d\n", sumTo(10));
    return 0;
}
```

```shell
chan@CMA:~/C_Programming/test$ ./final
SumTo: 45
```

- Here we have two nested loops.
- The outer loop iterates exactly `n` times, and for each iteration the inner loop iterates at most `n` times, and the innermost iteration costs `O(1)` each time, so the total is at most O(n<sup>2</sup>).

###  Tips for Determining Big O

1. **Identify the Largest Factor:**
   - Focus on the term that grows the fastest as *n* increases.
   - Ignore constants and lower-order terms.
2. **Identify Loops:**
   - A simple loop from 0 to `n` → `O(n)`
   - Nested loops (loop inside a loop) → Multiply their complexities (`O(n * n)`)
3. **Nested Loops:**
   - Each nested loop typically multiplies the time complexity.
   - Example: Two nested loops over *n* elements → O(n²).
4. **Recursive Calls:**
   - Determine how many times the function calls itself and the work done at each level.
   - Use the **Master Theorem** for divide-and-conquer recursions.
5. **Single vs. Multiple Operations:**
   - Sequential operations add up (but Big O takes the highest), while nested operations multiply.
6. **Common Patterns:**
   - **Single Loop:** O(n)
   - **Double Loop:** O(n²)
   - **Logarithmic Operations:** O(log n)
   - **Combining Operations:** e.g., O(n) + O(n log n) = O(n log n)

### Summary

- **Big O Notation** describes the **upper limit** of an algorithm's running time or space in terms of input size (*n*).

- An algorithm is logarithmic if it halves the input size at each step.
- **Common Big O Classes:**
  - **O(1):** Constant time. Operations that don’t depend on *n*.
  - **O(log n):** Logarithmic time. Reduces problem size exponentially each step.
  - **O(n):** Linear time. Processes each element once.
  - **O(n log n):** Linearithmic time. Combines linear and logarithmic growth, common in efficient sorting algorithms.
  - **O(n²):** Quadratic time. Involves nested iterations over the input.

![Screenshot from 2024-12-01 21-35-57](/home/chan/Downloads/Screenshot from 2024-12-01 21-35-57.png)

---

## Linked lists

- A **Linked List** is a linear data structure where each element is a separate object called a **node**. Unlike arrays, linked lists are **dynamic** and can easily grow or shrink in size by allocating or deallocating memory during runtime.
- Linked lists are about the simplest data structure beyond arrays.
- They aren't very efficient for many purposes, but have very good performance for certain specialized applications.
- The basic idea is that instead of storing `n` items in one big array, we store each item in its own `struct`, and each of these `structs` includes a pointer to the next `struct` in the list (with a null pointer to indicate that there are no more elements).
- If we follow the pointers we can eventually reach all of the elements.

### Linked Lists use cases

- Implement other data structures: stacks, queues, hash tables
- Dynamic memory allocation

### Types of Linked Lists

1. **Singly Linked List:** Each node points to the next node.
2. **Doubly Linked List:** Each node points to both the next and the previous node.
3. **Circular Linked List:** The last node points back to the first node, forming a circle.

### Singly Linked List in C

Each node in a singly linked list contains **data** and a **pointer** to the next node.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node
struct Node {
    int data;           // Data part
    struct Node* next;  // Pointer to the next node
};

```

#### Basic Operations

**Insertion**

1. **At the Beginning:** Add a new node at the start of the list.
2. **At the End:** Add a new node at the end of the list.
3. **After a Given Node:** Add a new node after a specified node.

**Deletion**

1. **From the Beginning:** Remove the first node.
2. **From the End:** Remove the last node.
3. **By Value:** Remove a node with a specific value.

**Traversal**

- **Display:** Print all the elements in the list.



### C Code Example (Simple)

#### Program Code

```C
#include <stdio.h>
#include <stdlib.h> // For malloc and free functions

// Define the structure of a node
struct Node {
    int data;
    struct Node* next;
};

int main() {
    // Allocate memory for nodes in the linked list
    struct Node* head = NULL;
    struct Node* second = NULL;
    struct Node* third = NULL;

    // Allocate memory for three nodes
    head = (struct Node*)malloc(sizeof(struct Node));
    second = (struct Node*)malloc(sizeof(struct Node));
    third = (struct Node*)malloc(sizeof(struct Node));

    // Assign data and link nodes
    head->data = 1;       // First node data
    head->next = second;  // Link first node with second node

    second->data = 2;     // Second node data
    second->next = third; // Link second node with third node

    third->data = 3;      // Third node data
    third->next = NULL;   // End of the list

    // Print the linked list
    struct Node* temp = head;
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }

    // Free allocated memory
    free(head);
    free(second);
    free(third);

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
1 2 3 
```



### C Code Example (advanced)

#### Program Code

`hello.h`

```C
struct Node
{
    int data;
    struct Node *next;
};

struct Node *createNode(int data);

void insertAtBeginning(struct Node **head, int new_data);

void insertAtEnd(struct Node **head_ref, int new_data);

void deleteNode(struct Node **head_ref, int key);

void printList(struct Node *node);

```

`hello.c`

```C
// Function to create a new node with given data
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*) malloc(sizeof(struct Node));
    if (!newNode) { // Check for malloc failure
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Insert a node at the beginning
void insertAtBeginning(struct Node** head_ref, int new_data) {
    struct Node* new_node = createNode(new_data);
    new_node->next = *head_ref; // Link the old list to the new node
    *head_ref = new_node;       // Move the head to point to the new node
}

// Insert a node at the end
void insertAtEnd(struct Node** head_ref, int new_data) {
    struct Node* new_node = createNode(new_data);
    if (*head_ref == NULL) {    // If the list is empty, make the new node as head
        *head_ref = new_node;
        return;
    }
    struct Node* last = *head_ref;
    while (last->next != NULL) { // Traverse to the last node
        last = last->next;
    }
    last->next = new_node; // Link the last node to the new node
}

// Delete a node by value
void deleteNode(struct Node** head_ref, int key) {
    struct Node* temp = *head_ref;
    struct Node* prev = NULL;

    // If head node itself holds the key
    if (temp != NULL && temp->data == key) {
        *head_ref = temp->next; // Changed head
        free(temp);             // Free old head
        return;
    }

    // Search for the key
    while (temp != NULL && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }

    // If key was not present
    if (temp == NULL) return;

    // Unlink the node and free memory
    prev->next = temp->next;
    free(temp);
}

// Traverse and print the linked list
void printList(struct Node* node) {
    while (node != NULL) {
        printf("%d -> ", node->data);
        node = node->next;
    }
    printf("NULL\n");
}

```

`main.c`

```C
int main() {
    struct Node* head = NULL;

    // Insert nodes into the list
    insertAtEnd(&head, 10);          // List: 10
    insertAtBeginning(&head, 20);    // List: 20 -> 10
    insertAtEnd(&head, 30);          // List: 20 -> 10 -> 30
    insertAtBeginning(&head, 40);    // List: 40 -> 20 -> 10 -> 30

    printf("Linked List after insertions: \n");
    printList(head); // Expected Output: 40 -> 20 -> 10 -> 30 -> NULL

    // Delete a node
    deleteNode(&head, 20);
    printf("Linked List after deleting 20: \n");
    printList(head); // Expected Output: 40 -> 10 -> 30 -> NULL

    return 0;
}
```

#### **Explanation:**

1. **Creating Nodes:**
   - `createNode` function allocates memory for a new node, assigns data, and initializes the `next` pointer to `NULL`.
2. **Insertion:**
   - `insertAtBeginning` adds a new node at the start by adjusting the head pointer.
   - `insertAtEnd` traverses to the end and links the new node.
3. **Deletion:**
   - `deleteNode` searches for a node with the specified key and removes it by updating pointers.
4. **Traversal:**
   - `printList` iterates through the list and prints each node's data.
5. **Main Function:**
   - Demonstrates inserting and deleting nodes, and displays the list after each operation.

#### **Step-by-Step Walkthrough**

##### **1. Program Start**

- The `main` function begins execution.
- The linked list is initialized as empty with `head = NULL`.

##### **2. Insertion Operations**

**a. Insert `10` at End**

- Function Call: `insertAtEnd(&head, 10);`
- Since `head` is `NULL`, the new node becomes the head.
- List: `10 -> Null`

**b. Insert `20` at Beginning**

- Function Call: `insertAtBeginning(&head, 20);`
- A new node with data `20` is created.
- Its `next` points to the current head (`10`).
- `head` is updated to point to the new node (`20`).
- List: `20 -> 10 -> Null`

**c. Insert `30` at End**

- Function Call: `insertAtEnd(&head, 30);`
- A new node with data `30` is created.
- The function traverses to the end of the list:
  - Starts at `head` (`20`), moves to `10`, then reaches the end.
- The last node's (`10`) `next` is updated to point to the new node (`30`).
- List: `20 -> 10 -> 30 -> Null`

**d. Insert `40` at Beginning**

- Function Call: `insertAtBeginning(&head, 40);`
- A new node with data `40` is created.
- Its `next` points to the current head (`20`).
- `head` is updated to point to the new node (`40`).
- List: `40 -> 20 -> 10 -> 30 -> Null`

##### **3. Print List after Insertions**

- Function Call: `printList(head);`
- Traverses the list from `head` (40):
  - Prints `40`, moves to `20`.
  - Prints `20`, moves to `10`.
  - Prints `10`, moves to `30`.
  - Prints `30`, reaches `NULL`.
- Output: `40 -> 20 -> 10 -> 30 -> Null`

##### **4. Deletion Operation**

**Delete Node with Data `20`**

- Function Call: `deleteNode(&head, 20);`
- Checks if `head` contains the key (20):
  - `head` (`40`) does not match `20`.
- Traverses the list to find the node with `data == 20`:
  - `prev` is `NULL`, `temp` is `head` (`40`).
  - Moves `prev` to `40`, `temp` to `20`.
- Node with `data == 20` is found (`temp`).
- Removes the node:
  - `prev->next` (`40->next`) is updated to `temp->next` (`10`).
- Frees the memory allocated for the node with `20`.
- List after deletion: `40 -> 10 -> 30 -> Null`

##### **5. Print List after Deletion**

- Function Call: `printList(head);`
- Traverses the list from `head` (40):
  - Prints `40`, moves to `10`.
  - Prints `10`, moves to `30`.
  - Prints `30`, reaches `NULL`.
- Output: `40 -> 10 -> 30 -> Null`

##### **6. Program End**

- The `main` function returns `0`, indicating successful execution.
- The program terminates.

------

##### **Visualization of Linked List Changes**

**After Each Operation**

1. **Initial List:**
   - Empty (`head = NULL`)
2. **Insert `10` at End:**
   - List: `10 -> Null`
3. **Insert `20` at Beginning:**
   - List: `20 -> 10 -> Null`
4. **Insert `30` at End:**
   - List: `20 -> 10 -> 30 -> Null`
5. **Insert `40` at Beginning:**
   - List: `40 -> 20 -> 10 -> 30 -> Null`
6. **Delete Node with Data `20`:**
   - List: `40 -> 10 -> 30 -> Null`

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Linked List after insertions: 
40 -> 20 -> 10 -> 30 -> Null
Linked List after deleting 20: 
40 -> 10 -> 30 -> Null

```



### C Code Example (advanced) - 2

#### Program Code

```C
struct Node
{
    int value;
    struct Node *next;
};

typedef struct Node node_t;

void printList(node_t *head){
    node_t *temp = head;
    
    while(temp != NULL){
        printf("%d - ", temp->value);
        temp = temp->next;
    }
    printf("\n");
}

node_t *create_new_node(int value){
    node_t *result = malloc(sizeof(node_t));
    result->value = value;
    result->next = NULL;
    return result;
}

// head-> = [0, NULL]
// node_to_insert = 1, 1->next = [0, NULL]
// now the head points to -> [1, next] -> [0, NULL]]
node_t *insert_at_head(node_t **head, node_t *node_to_insert){
    node_to_insert->next = *head;
    *head = node_to_insert;
    return node_to_insert;
}

// 13 = node_to_insert, 75 = newnode
// Let's say 13 is pointing to 12, 13->next = 12
// now 75->next = 13->next which is = 12
// then set 13->next to 75 (newnode)
node_t *insert_after_node(node_t *node_to_insert, node_t *newnode){
    // set the newnode next pointer to where the node_to_insert is pointing
    newnode->next = node_to_insert->next;
    
    // for successful insert, set the current node next pointer to point to the newnode
    node_to_insert->next = newnode; 
}

node_t *find_node(node_t *head, int value){
    node_t *tmp = head;
    while(tmp != NULL){
        //If a match is found, the function returns the pointer to that node.
        if(tmp->value == value){
            return tmp;
        }
        
        // If the current node doesn't match, it moves to the next node:
        tmp = tmp->next;
    }
    
    // Return NULL if Not Found
    return NULL;
}

int main(){
    node_t *head = NULL;
    node_t *tmp;
    
    for(int i = 0; i < 25; i++){
        tmp = create_new_node(i);
        insert_at_head(&head, tmp);
    }
    
    tmp = find_node(head, 13);
    printf("found node with %d\n", tmp->value);
    
    insert_after_node(tmp, create_new_node(75));
    
    printList(head);
    return 0;
}
```

- The double pointer `node_t **head` allows the `insert_at_head` function to modify the original `head` pointer from the calling function (`main`). This is necessary because we need to update the `head` of the linked list to point to the new node we've inserted.
  - **`node_t **head`**: A pointer to the pointer `head`. By passing the address of `head`, the function can directly modify the pointer in `main`.
  - **Modifying `*head`**: By dereferencing the double pointer, `*head`, we access and modify the original `head` pointer.

##### Visualization

**`insert_after_node`** function:

**Before Insertion:**

```
... -> [14] -> [13] -> [12] -> ...
```

- **Nodes Involved:**
  - **`node_to_insert`**: Node with `value = 13`
  - **`newnode`**: Node with `value = 75`
- **Steps:**
  1. **Set `newnode->next` to `node_to_insert->next`**
     - `newnode->next` points to `[12]`.
  2. **Update `node_to_insert->next` to `newnode`**
     - `node_to_insert->next` points to `newnode`.

**After Insertion:**

```
... -> [14] -> [13] -> [75] -> [12] -> ...
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
found node with 13
24 - 23 - 22 - 21 - 20 - 19 - 18 - 17 - 16 - 15 - 14 - 13 - 75 - 12 - 11 - 10 - 9 - 8 - 7 - 6 - 5 - 4 - 3 - 2 - 1 - 0 - 

```



```C
struct elt{
    struct elt *next; // pointer to next element in the list
    int contents; // contents of this element
};
```

- In comparison to arrays, inserting or removing elements in linkedList can be cheap: at the front of the list, inserting a new element just requires allocating another `struct` and hooking up a few pointers, while removing an element just requires moving the pointer to the first element to point to the second element instead, and then freeing the first element.

![Screenshot from 2024-12-01 21-53-26](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-01 21-53-26.png)

- To make this work, we need to change two pointers:
  - The head pointer and the `next` pointer in the element holding 0.
  - These operations aren't affected by the size of the rest of the list and so take O(1) time.
  - Removal is the reverse of installation.
  - We patch out the first element by shifting the head pointer to the second element, then deallocate it with `free`.
  - We do have to be careful to get any data we need out of it before calling `free()`.
  - This is also O(1) opeartion.
- The fact that we can add and remove elements at the start of linked lists for cheap makes them particularly useful for implementing a **stack**.

### Looping over a linked list backwards

```C
```



### Doubly Linked List

A **Doubly Linked List** is a type of linked list where each node contains pointers to both the **next** and **previous** nodes in the sequence. This bidirectional nature allows traversal in both directions, making certain operations more efficient compared to a singly linked list.

**Key Features:**

- **Bidirectional Traversal:** Can move forward and backward through the list.
- **Dynamic Size:** Can easily grow or shrink by adding or removing nodes.
- **Efficient Insertion/Deletion:** Especially at both ends or any given node.

#### Structure Definition

Each node in a doubly linked list contains three parts:

1. **Data:** The value stored in the node.
2. **Next Pointer:** Points to the next node in the list.
3. **Previous Pointer:** Points to the previous node in the list.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a doubly linked list
struct Node {
    int data;
    struct Node* next;
    struct Node* prev;
};
```

#### Basic Operations

##### Insertion

1. **At the Beginning:**
   - Create a new node.
   - Set the new node's `next` to the current head.
   - Set the current head's `prev` to the new node.
   - Update the head to the new node.
2. **At the End:**
   - Create a new node.
   - Traverse to the last node.
   - Set the last node's `next` to the new node.
   - Set the new node's `prev` to the last node.
3. **After a Given Node:**
   - Create a new node.
   - Adjust the `next` and `prev` pointers of neighboring nodes to include the new node.

##### Deletion

1. **From the Beginning:**
   - Update the head to the next node.
   - Set the new head's `prev` to `NULL`.
   - Free the old head node.
2. **From the End:**
   - Traverse to the last node.
   - Update the second last node's `next` to `NULL`.
   - Free the last node.
3. **By Value:**
   - Search for the node containing the value.
   - Adjust the `next` and `prev` pointers of neighboring nodes to exclude the target node.
   - Free the target node.

##### Traversal

- **Forward Traversal:** Start from the head and move using the `next` pointers.
- **Backward Traversal:** Start from the tail and move using the `prev` pointers.

#### C Code Implementation # 1

##### Program Code

`practice.h`

```C
// Definition of a node in a doubly linked list
struct Node
{
    int data;
    struct Node *next;
    struct Node *prev;
};

typedef struct Node Node;

Node *createNode(int data);
void insertAtBeginning(Node **head, int new_data);

void insertAtEnd(Node **head, int new_data);

void deleteNode(Node **head, int key);

void traverseForward(Node *node);

void traverseBackward(Node *tail);

Node *findTail(Node *head);
```

`functions.c`

```C
Node *createNode(int data){
    Node *newNode = malloc(sizeof(Node));
    if(newNode == NULL){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void insertAtBeginning(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    // since we are adding the element at the beginning, we have to set our newNode next to poitn to the head element.
    newNode->next = *head;
    
    // If there's a head element, set the head prev pointer to our newNode effectively placing the head right after our newNode
    if(*head != NULL){
        (*head)->prev = newNode;
    }
    
    // the head will now point to newNode
    // i.e newNode will become the first element (head)
    *head = newNode;
    printf("Inserted %d at the beginning\n", new_data);
}

void insertAtEnd(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    // We will traverse the node starting from the head
    Node *last = *head;
    
    // If the list is empty, make the new node as head
    if(*head == NULL){
        *head = newNode;
        printf("Inserted %d as the first element.\n", new_data);
        return;
    }
    
    // while the next node is not equal to NULL which means we are not at the last node, 
    while(last->next != NULL){
        last = last->next; // continue traversing from the head till the last
    }
    
    // if we've found the last element, set the next pointer of that element to the newNode, effectively inserting our newNode at the end of the list
    // last <-> newNode
    last->next = newNode;
    newNode->prev = last; // point the prev node back to the last element in the list
    
    printf("Inserted %d at the end.\n", new_data);
}

void deleteNode(Node **head, int key){
    Node *temp = *head;
    
    // search for the node to be deleted
    while(temp != NULL && temp->data != key){
        temp = temp->next;
    }
    
    // if the node wasn't found
    if(temp == NULL){
        printf("Element %d not found.\n", key);
        return;
    }
    
    // if the node to be deleted is the head
    if(*head == temp){
        *head = temp->next;
    }
    
    // Change next only if the node to be deleted is not the last node
    if(temp->next != NULL){
        
        // [3] <-> [4] <-> [5]
        // [4] = temp, 
        // temp->next->prev = [5]'s prev
        // temp->prev = [3]
        // set [5]'s prev pointer to [3], removing [4]
        temp->next->prev = temp->prev;
    }
    
    // Change prev only if the node to be deleted is not the first node
    if(temp->prev != NULL){
        // [3] <-> [4] <-> [5]
        // [4] = temp, 
        // temp->prev->next = [3]'s next
        // temp->next = [5]
        // set [3]'s next to [5] removing [4]
        temp->prev->next = temp->next;
    }
    
    free(temp);
    printf("Deleted %d from the list.\n", key);
}

void traverseForward(Node *node){
    printf("Forward Traversal: ");
    while(node != NULL){
        printf("%d <-> ", node->data);
        node = node->next;
    }
    printf("NULL\n");
}

void traverseBackward(Node *tail){
    printf("Backward Traversal: ");
    while(tail != NULL){
        printf("%d <-> ", tail->data);
        tail = tail->prev;
    }
    printf("NULL\n");
}

Node *findTail(Node *head){
    Node *tail = head;
    
    // If the node is empty, return NULL
    if(head == NULL){
        return NULL;
    }
    
    // Keep traversing till the end of the list
    while(tail->next != NULL){
        tail = tail->next;
    }
    return tail;
}
```

`practice.c`

```C
int main(){
    Node *node = NULL;
    
    // Inserting elements
    insertAtBeginning(&node, 10); // List: 10
    insertAtEnd(&node, 20); // List: 10 <-> 20
    insertAtBeginning(&node, 5); // List: 5 <-> 10 <-> 20
    
    insertAtEnd(&node, 25); // List: 5 <-> 10 <-> 20 <-> 25

    // Traversing the list
    traverseForward(node);
    Node *tail = findTail(node);
    traverseBackward(tail);
    
    deleteNode(&node, 10); // List: 5 <-> 20 <-> 25
    
    // Traversing the list again after deleting
    traverseForward(node);
    tail = findTail(node);
    traverseBackward(tail);
    
    // Attempting to delete a non-existent element
    deleteNode(&head, 100);
    return 0;
}
```



##### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 at the beginning
Inserted 20 at the end.
Inserted 5 at the beginning
Inserted 25 at the end.
Forward Traversal: 5 <-> 10 <-> 20 <-> 25 <-> NULL
Backward Traversal: 25 <-> 20 <-> 10 <-> 5 <-> NULL
Deleted 10 from the list.
Forward Traversal: 5 <-> 20 <-> 25 <-> NULL
Backward Traversal: 25 <-> 20 <-> 5 <-> NULL
Element 100 not found.

```

#### Summary

##### Doubly Linked Lists

- **Bidirectional Traversal:** Can move both forward and backward through the list.
- **Nodes:** Each node contains data and pointers to both the next and previous nodes.
- **Dynamic Size:** Easily grows or shrinks by adding or removing nodes.
- **Operations:** Efficient insertion and deletion at both ends or any given node.
- **Use Cases:** Implementing other data structures (e.g., stacks, queues, deques), navigation systems, etc.



### Circular Linked List

A **Circular Linked List** is a variation of the standard linked list where the last node points back to the first node, forming a **circle**. This structure allows traversal of the list starting from any node, looping infinitely through the list. Circular linked lists can be **singly** or **doubly** linked.

**Key Features:**

- **No NULL Pointers:** The last node links back to the first node.
- **Continuous Traversal:** Can cycle through the list endlessly.
- **Efficient for Round-Robin Scheduling:** Ideal for applications requiring cyclic iterations.

#### Structure Definition

A **Singly Circular Linked List** has nodes where each node points to the next node, and the last node points back to the first node. Similarly, a **Doubly Circular Linked List** includes both `next` and `prev` pointers, with the last node's `next` pointing to the first node and the first node's `prev` pointing to the last node.

For simplicity, we'll focus on **Singly Circular Linked Lists**.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a singly circular linked list
struct Node {
    int data;
    struct Node* next;
};

```

#### Basic Operations

##### Insertion

1. **At the Beginning:**
   - Create a new node.
   - If the list is empty, point the new node to itself.
   - Otherwise, traverse to the last node, set its `next` to the new node.
   - Set the new node's `next` to the head.
   - Update the head to the new node.
2. **At the End:**
   - Create a new node.
   - If the list is empty, point the new node to itself and set it as head.
   - Otherwise, traverse to the last node.
   - Set the last node's `next` to the new node.
   - Set the new node's `next` to the head.
3. **After a Given Node:**
   - Create a new node.
   - Find the specified node.
   - Set the new node's `next` to the specified node's `next`.
   - Set the specified node's `next` to the new node.

##### Deletion

1. **From the Beginning:**
   - If the list is empty, indicate underflow.
   - If the list has only one node, set head to `NULL`.
   - Otherwise, traverse to the last node.
   - Set the last node's `next` to the second node.
   - Free the original head.
   - Update the head to the second node.
2. **From the End:**
   - If the list is empty, indicate underflow.
   - If the list has only one node, set head to `NULL`.
   - Otherwise, traverse to the second last node.
   - Set the second last node's `next` to the head.
   - Free the last node.
3. **By Value:**
   - If the list is empty, indicate underflow.
   - Traverse the list to find the node with the specified value.
   - If found, adjust the `next` pointers to exclude the node.
   - Free the node.

##### Traversal

- **Display:** Start from the head and move through the list until you reach back to the head.

#### C Code Example # 1

##### Program Code

`practice.h`

```C
struct Node
{
    int data;
    struct Node *next;
};
typedef struct Node Node;

Node *createNode(int data);
void insertAtBeginning(Node **head, int new_data);

void insertAtEnd(Node **head, int new_data);

void deleteNode(Node **head, int key);

void traverse(Node *head);
```

`functions.c`

```C
Node *createNode(int data){
    Node *newNode = malloc(sizeof(Node));
    if(!newNode){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

void insertAtBeginning(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    if(*head == NULL){
        newNode->next = newNode; // Point to itself
        *head = newNode;
        printf("Inserted %d as the first node.\n", new_data);
        return;
    }
    
    Node *temp = *head;
    
    // traverse to the last node
    while(temp->next != *head){
        temp = temp->next;
    }
    
    temp->next = newNode; // Link last node to new node
    newNode->next = *head; // Link new node to head
    *head = newNode; // Update head to new node
    
    printf("Inserted %d at the beginning.\n", new_data);
}

void insertAtEnd(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    if(*head == NULL){ 
        newNode->next = newNode; // Point to itself
        *head = newNode;
        printf("Inserted %d as the first node.\n", new_data);
        return;
    }
    
    Node *temp = *head;
    
    while(temp->next != *head){
        temp = temp->next;
    }
    
    temp->next = newNode; // Link last node to new node
    newNode->next = *head; // Link new node to head
    
    printf("Inserted %d at the end.\n", new_data);
}

void deleteNode(Node **head, int key){
    if(*head == NULL){
        printf("List is empty.\n");
        return;
    }
    
    Node *current = *head;
    Node *prev = NULL;
    
    // If head node is to be deleted
    if(current->data == key){
        // If there's only one node
        if(current->next == *head){
            *head = NULL;
        }else{
            // Find the last node
            Node *last = *head;
            while(last->next != *head){
                last = last->next;
            }
            
            *head = current->next;
            last->next = *head;
        }
        free(current);
        printf("Deleted %d from the list.\n", key);
        return;
    }
    
    // Search for the node to be deleted
    do{
        prev = current;
        current = current->next;
        if(current->data == key){
            prev->next = current->next;
            free(current);
            printf("Deleted %d from the list\n", key);
            return;
        }
    }while(current != *head);
    printf("%d not found in the list.\n", key);
}

void traverse(Node *head){
    if(head == NULL){
        printf("List is empty.\n");
        return;
    }
    
    Node *temp = head;
    printf("Circular Linked List: ");
    do{
        printf("%d -> ", temp->data);
        temp = temp->next;
    }while(temp != head);
    printf("HEAD\n");
}
```

`practice.c`

```C
int main()
{
    Node *head = NULL;

    // insert elements
    insertAtBeginning(&head, 10); // List: 10
    traverse(head);

    insertAtEnd(&head, 20); // List: 10 -> 20
    traverse(head);

    insertAtBeginning(&head, 5); // List: 5 -> 10 -> 20
    traverse(head);

    insertAtEnd(&head, 25); // List: 5 -> 10 -> 20 -> 25
    traverse(head);

    // Delete elements
    deleteNode(&head, 10); // List: 5 -> 20 -> 25
    traverse(head);

    deleteNode(&head, 5); // List: 20 -> 25
    traverse(head);

    deleteNode(&head, 30); // Attempt to delete non-existent element
    return 0;
}
```



##### Visualization

Let's visualize how the **Singly Circular Linked List** operates step-by-step.

Initial Empty List:

```less
[Head] = NULL
```

After Inserting 10 at the Beginning:

```less
[Head] --> [10] --> [10] (points back to itself)
```

- **Head:** Points to the new node (10).
- **Node10->next:** Points back to itself.

After Inserting 20 at the End:

```less
[Head] --> [10] --> [20] --> [10] (points back to head)
```

- **Head:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points back to 10.

After Inserting 5 at the Beginning:

```css
[Head] --> [5] --> [10] --> [20] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points back to 5.

After Inserting 25 at the End:

```css
[Head] --> [5] --> [10] --> [20] --> [25] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 5.

After Deleting 10:

```css
[Head] --> [5] --> [20] --> [25] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 5.

After Deleting 5:

```css
[Head] --> [20] --> [25] --> [20] (points back to head)
```

- **Head:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 20.

Attempting to Delete 30 (Non-existent):

```css
List remains unchanged:
[Head] --> [20] --> [25] --> [20] (points back to head)
```

**Diagram Representation:**

```css
Step-by-Step Operations:

1. Initial State:
[Head] = NULL

2. Insert 10 at Beginning:
[Head] --> [10] --> [10] (self-loop)

3. Insert 20 at End:
[Head] --> [10] --> [20] --> [10]

4. Insert 5 at Beginning:
[Head] --> [5] --> [10] --> [20] --> [5]

5. Insert 25 at End:
[Head] --> [5] --> [10] --> [20] --> [25] --> [5]

6. Delete 10:
[Head] --> [5] --> [20] --> [25] --> [5]

7. Delete 5:
[Head] --> [20] --> [25] --> [20]

8. Attempt to Delete 30:
No change in the list.
```

##### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 as the first node.
Circular Linked List: 10 -> HEAD
Inserted 20 at the end.
Circular Linked List: 10 -> 20 -> HEAD
Inserted 5 at the beginning.
Circular Linked List: 5 -> 10 -> 20 -> HEAD
Inserted 25 at the end.
Circular Linked List: 5 -> 10 -> 20 -> 25 -> HEAD
Deleted 10 from the list.
Circular Linked List: 5 -> 20 -> 25 -> HEAD
Deleted 5 from the list.
Circular Linked List: 20 -> 25 -> HEAD
30 not found in the list.
```

#### Use Cases

**Circular Linked Lists** are useful in scenarios where the data needs to be processed in a cyclic manner or where continuous traversal is required without restarting from the head. Some common real-world applications include:

1. **Round-Robin Scheduling:**
   - **Operating Systems:** Managing process scheduling in a fair and cyclic order.
   - **Real-Time Systems:** Ensuring tasks are executed in a timely, repetitive manner.
2. **Music or Video Players:**
   - **Playlists:** Continuously playing songs or videos in a loop.
3. **Buffer Management:**
   - **Circular Buffers:** Implementing buffers that wrap around, similar to Ring Buffers, for managing streaming data.
4. **Game Development:**
   - **Turn-Based Games:** Managing player turns in a cyclic order.
5. **Data Structures:**
   - **Josephus Problem:** Solving problems that involve circular elimination processes.
6. **Networking:**
   - **Token Ring Networks:** Managing access to the network using a token passed in a circular manner.

**Example: Round-Robin Scheduler**

In a Round-Robin Scheduler, each process is assigned a fixed time slot in a cyclic order. A circular linked list efficiently manages this by allowing the scheduler to cycle through processes endlessly without restarting the traversal.

```rust
Processes: P1, P2, P3, P4

Circular Linked List:
[P1] -> [P2] -> [P3] -> [P4] -> [P1] ...
```

The scheduler moves to the next process by following the `next` pointer, and once it reaches the end, it loops back to the beginning seamlessly.

---

## Stacks

- A **Stack** is a linear data structure that follows the **Last In, First Out (LIFO)** principle. Think of it like a stack of plates: we add (push) plates to the top and remove (pop) plates from the top.
- When we pop, we get the last item we pushed.
- A Stack is an abstract data type that supports operations `push` (insert a new element on top of the stack) and `pop` (remove and return the element at the top of the stack).
- Below is an example of a simple linked-list implementation of a stack.

### Stack Operations

1. **Push:** Add an element to the top of the stack.
2. **Pop:** Remove the top element from the stack.
3. **Peek (Top):** View the top element without removing it.
4. **IsEmpty:** Check if the stack is empty.



### C Code Example #1

#### Program Code

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a stack node
struct StackNode {
    int data;
    struct StackNode* next;
};

typedef struct StackNode SN;

// Function to create a new stack node
SN* createNode(int data) {
    struct StackNode* newNode = (SN*) malloc(sizeof(SN));
    if (!newNode) { // Check for malloc failure
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Push operation: Add an element to the stack
void push(SN** top_ref, int new_data) {
    SN* new_node = createNode(new_data);
    new_node->next = *top_ref; // Link the old stack to the new node
    *top_ref = new_node;       // Update the top pointer
    printf("Pushed %d to stack\n", new_data);
}

// Pop operation: Remove an element from the stack
int pop(SN** top_ref) {
    if (*top_ref == NULL) {
        printf("Stack Underflow\n");
        exit(1);
    }
    SN* temp = *top_ref;
    int popped = temp->data;
    *top_ref = temp->next; // Move the top pointer to the next node
    free(temp);            // Free memory
    return popped;
}

// Peek operation: Get the top element without removing it
int peek(SN* top) {
    if (top == NULL) {
        printf("Stack is empty\n");
        exit(1);
    }
    return top->data;
}

// Check if the stack is empty
int isEmpty(SN* top) {
    return (top == NULL);
}

// Function to print the stack
void printStack(SN* top) {
    printf("Stack: ");
    while (top != NULL) {
        printf("%d -> ", top->data);
        top = top->next;
    }
    printf("NULL\n");
}

// Driver program to test the stack operations
int main() {
    SN* stack = NULL; // Initialize stack as empty

    push(&stack, 10);
    push(&stack, 20);
    push(&stack, 30);

    printStack(stack); // Expected Output: 30 -> 20 -> 10 -> NULL

    printf("Top element is %d\n", peek(stack)); // Expected Output: 30

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 30
    printStack(stack); // Expected Output: 20 -> 10 -> NULL

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 20
    printStack(stack); // Expected Output: 10 -> NULL

    printf("Is stack empty? %s\n", isEmpty(stack) ? "Yes" : "No"); // Expected Output: No

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 10
    printStack(stack); // Expected Output: NULL

    printf("Is stack empty? %s\n", isEmpty(stack) ? "Yes" : "No"); // Expected Output: Yes

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Pushed 10 to stack
Pushed 20 to stack
Pushed 30 to stack
Stack: 30 -> 20 -> 10 -> NULL
Top element is 30
Popped element: 30
Stack: 20 -> 10 -> NULL
Popped element: 20
Stack: 10 -> NULL
Is stack empty? No
Popped element: 10
Stack: NULL
Is stack empty? Yes

```



### C Code Example # 2

#### Program Code

`hello.h`

```C
// Definition of a stack element
struct elt {
    struct elt *next; // Pointer to the next element in the stack
    int value;        // Integer value stored in the stack element
};

// Typedef for the stack, which is a pointer to the top element
typedef struct elt *Stack;

// Macro representing an empty stack (NULL pointer)
#define STACK_EMPTY (0)

// Function prototypes
void stackPush(Stack *s, int value);
int stackEmpty(const Stack *s);
int stackPop(Stack *s);
void stackPrint(const Stack *s);
```

`hello.c`

```C
void stackPush(Stack *s, int value)
{
    struct elt *e; // Allocate memory for a new stack element

    e = malloc(sizeof(struct elt));
    assert(e); // Ensure memory allocation was successful

    // Initialize the new element with the given value
    e->value = value;
    e->next = *s;// Point the new element to the current top of the stack
    *s = e; // Update the stack pointer to point to the new element
}


int stackEmpty(const Stack *s)
{
    // Check if the stack pointer is NULL (empty stack)
    return (*s == 0);
}


int stackPop(Stack *s)
{
    int ret;
    struct elt *e;

    // Ensure the stack is not empty before popping
    assert(!stackEmpty(s));
    
    // Retrieve the value from the top element
    ret = (*s)->value;

    // Remove the top element from the stack
    e = *s;
    *s = e->next;

    free(e);
    
    // Return the popped value
    return ret;
}


void stackPrint(const Stack *s)
{
    struct elt *e;
    
    // Iterate through the stack elements
    for (e = *s; e != 0; e = e->next)
    {
        printf("%d ", e->value);
    }
    
    // Move to the next line after printing the stack
    putchar('\n');
}
```

`main.c`

```C
int main()
{
    int i;
    Stack s;

    // Initialize the stack to be empty
    s = STACK_EMPTY;

    // Push integers 0 to 4 onto the stack
    for (i = 0; i < 5; i++)
    {
        printf("push %d\n", i);
        stackPush(&s, i);
        stackPrint(&s);
    }

    // Pop elements from the stack until it is empty
    while (!stackEmpty(&s))
    {
        printf("pop gets %d\n", stackPop(&s));
        stackPrint(&s);
    }

    return 0;
}
```

#### Step-by-Step Walkthrough

- The loop runs with `i` from `0` to `4`.

- In each iteration:

  - **Print Push Message**: `"push i"`
  - Push Value: `stackPush(&s, i);`
    - `stackPush` adds the value `i` to the top of the stack `s`.
  - Print Stack State: `stackPrint(&s);`
    - `stackPrint` displays the current contents of the stack.

- **Detailed Operations in Each Iteration**

  - **Iteration 1 (`i = 0`):**
    - Push `0` onto the stack.
    - Stack becomes: `0`
  - **Iteration 2 (`i = 1`):**
    - Push `1` onto the stack.
    - Stack becomes: `1 0`
  - **Iteration 3 (`i = 2`):**
    - Push `2` onto the stack.
    - Stack becomes: `2 1 0`
  - **Iteration 4 (`i = 3`):**
    - Push `3` onto the stack.
    - Stack becomes: `3 2 1 0`
  - **Iteration 5 (`i = 4`):**
    - Push `4` onto the stack.
    - Stack becomes: `4 3 2 1 0`

  **Note**: The stack is implemented as a linked list where each new element becomes the new head of the list, resulting in a LIFO (Last-In-First-Out) order.

**Popping Elements from the Stack**

1. **While Loop to Pop Values**

   ```C
   while (!stackEmpty(&s))
   
   {
   
     printf("pop gets %d\n", stackPop(&s));
   
     stackPrint(&s);
   
   }
   ```

   

   - The loop continues as long as the stack is not empty.
   - In each iteration:
     - Pop Value:`stackPop(&s);`
       - `stackPop` removes and returns the value from the top of the stack `s`.
     - **Print Pop Message**: `"pop gets value"`
     - Print Stack State: `stackPrint(&s);`
       - `stackPrint` displays the current contents of the stack after popping.

2. **Detailed Operations in Each Iteration**

   - **Iteration 1:**
     - Pop the top value `4` from the stack.
     - Stack becomes: `3 2 1 0`
     - Output: `"pop gets 4"`
   - **Iteration 2:**
     - Pop the top value `3` from the stack.
     - Stack becomes: `2 1 0`
     - Output: `"pop gets 3"`
   - **Iteration 3:**
     - Pop the top value `2` from the stack.
     - Stack becomes: `1 0`
     - Output: `"pop gets 2"`
   - **Iteration 4:**
     - Pop the top value `1` from the stack.
     - Stack becomes: `0`
     - Output: `"pop gets 1"`
   - **Iteration 5:**
     - Pop the top value `0` from the stack.
     - Stack becomes: empty
     - Output: `"pop gets 0"`

3. **End of Program**

   - Once the stack is empty, the condition `!stackEmpty(&s)` becomes false, and the loop exits.
   - The program reaches the `return 0;` statement and terminates.

**Stack Using Linked List**

- The stack is implemented using a singly linked list where each node (`struct elt`) contains an integer `value` and a pointer `next` to the next element.

#### **Functions Explanation**

**1. `stackPush(Stack *s, int value)`**

- **Purpose**: Adds a new element with the given `value` to the top of the stack.
- **Steps**:
  - Allocate memory for a new element.
  - Set the new element's `value` to the given `value`.
  - Point the new element's `next` to the current top of the stack (`*s`).
  - Update the stack pointer `*s` to point to the new element.

**2. `int stackPop(Stack *s)`**

- **Purpose**: Removes and returns the value from the top element of the stack.

- **Ensure Stack is Not Empty:**
  
  - `assert(!stackEmpty(s));`
  - Ensures that the stack is not empty before attempting to pop an element.
  
  - **Retrieve the Value:**
    - `ret = (*s)->value;`
    - Retrieves the value from the top element of the stack.
    - `(*s)` dereferences the stack pointer to get the top element. `(*s)->value` accesses the `value` field of the top element and stores it in the variable `ret`.
  - **Remove the Top Element:**
    - `e = *s;`
    - Stores the pointer to the top element in `e`.
    - `*s = e->next;`
    - Updates the stack pointer to point to the next element, effectively removing the top element.
    - `e = *s;` stores the pointer to the top element in the variable `e`.
    - `*s = e->next;` updates the stack pointer to point to the next element in the stack, effectively removing the top element.
  - **Free Memory:**
    - `free(e);`
    - Frees the memory allocated for the removed element.
  - **Return the Value:**
    - `return ret;`
    - Returns the value retrieved from the top element.
  
- **Visualization**:

  - Let's visualize the process with an example. Suppose we have the following stack:

  - ```
    Top -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - We want to pop the top element (`4`) from the stack.

  - **Initial State**

    - **Stack Pointer (`s`):** Points to the top element (`4`).
    - **Top Element (`*s`):** `[4]`
    - **Next Element (`(*s)->next`):** `[3]`

    ```
    s -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Retrieve the Value from the Top Element**

    ```C
    ret = (*s)->value;
    ```

    - `(*s)` points to `[4]`.
    - `(*s)->value` is `4`.
    - `ret` is set to `4`.

  - **Remove the Top Element from the Stack**:

    ```C
    e = *s;
    *s = e->next;
    ```

    - `e = *s;` stores the pointer to `[4]` in `e`.
    - `*s = e->next;` updates `*s` to point to `[3]`.

  - **State After Removal:**

    ```
    e -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    s -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Free the Memory Allocated for the Removed Element**

    ```C
    free(e);
    ```

    - Frees the memory allocated for `[4]`.

  - **State After Freeing Memory:**

    ```
    s -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Return the Popped Value**:

    ```C
    return ret;
    ```

    - Returns `4`.

  - **Final State**:

    After popping the top element (`4`), the stack looks like this:

    ```
    Top -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

    - The stack pointer (`s`) now points to the new top element (`3`).
    - The value `4` has been returned by the `stackPop` function.


**3. `int stackEmpty(const Stack *s)`**

- **Purpose**: Checks if the stack is empty.
- **Returns**: `1` (true) if the stack is empty (`*s == 0`), `0` (false) otherwise.

**4. `void stackPrint(const Stack *s)`**

- **Purpose**: Prints the current contents of the stack from top to bottom.
- **Steps**:
  - Iterate through the linked list starting from the top of the stack (`*s`).
  - In each iteration, print the `value` of the current element.
  - After printing all elements, move to a new line.

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
push 0
0 
push 1
1 0 
push 2
2 1 0 
push 3
3 2 1 0 
push 4
4 3 2 1 0 
pop gets 4
3 2 1 0 
pop gets 3
2 1 0 
pop gets 2
1 0 
pop gets 1
0 
pop gets 0

```



### C Code Example #3

#### Program Code

`practice.h`

```C
typedef struct
{
    int *collection;
    int capacity;
    int size;
} Stack;

Stack *create_stack(int capacity);
void destroy_stack(Stack *stack);
bool is_full(Stack *stack);
bool is_empty(Stack *stack);
bool pop(Stack *stack, int *item);
bool push(Stack *stack, int item);
bool peek(Stack *stack, int *item);
```

`functions.c`

```C
Stack *create_stack(int capacity)
{
    if (capacity <= 0)
    {
        return NULL;
    }

    Stack *stack = malloc(sizeof(Stack));
    if (stack == NULL)
    {
        return NULL;
    }
    stack->collection = malloc(sizeof(int) * capacity);
    if (stack->collection == NULL)
    {
        free(stack);
        return NULL;
    }

    stack->capacity = capacity;
    stack->size = 0;

    return stack;
}

void destroy_stack(Stack *stack)
{
    free(stack->collection);
    free(stack);
}

bool is_full(Stack *stack)
{
    return stack->capacity == stack->size;
}

bool is_empty(Stack *stack)
{
    return stack->size == 0;
}

bool pop(Stack *stack, int *item)
{
    if (is_empty(stack))
    {
        return false;
    }
    stack->size--;
    *item = stack->collection[stack->size];
    return true;
}

bool push(Stack *stack, int item)
{
    if (is_full(stack))
    {
        return false;
    }
    stack->collection[stack->size++] = item;
    return true;
}

bool peek(Stack *stack, int *item)
{
    if (is_empty(stack))
    {
        return false;
    }
    *item = stack->collection[stack->size - 1];
    return true;
}
```



`practice.c`

```C
int main()
{
    Stack *stack = create_stack(5);

    if (stack == NULL)
    {
        fprintf(stderr, "Failed to create stack\n");
        return 1;
    }

    if (is_empty(stack))
    {
        printf("Stack is empty\n");
    }
    push(stack, 2);
    printf("Stack size: %d\n", stack->size);
    push(stack, 9);
    push(stack, 4);
    push(stack, 7);
    push(stack, 8);

    printf("Stack size: %d\n", stack->size);
    if (is_full(stack))
    {
        printf("Stack is full\n");
    }
    bool try_push = push(stack, 5);
    if (!try_push)
    {
        printf("Push failed\n");
    }

    int peek_val = 0;
    peek(stack, &peek_val);
    printf("Peek value: %d\n", peek_val);

    int pop_val = 0;
    for (int i = 0; i < 5; i++)
    {
        pop(stack, &pop_val);
        printf("Popped value: %d\n", pop_val);
    }

    bool try_pop = pop(stack, &pop_val);
    if (try_pop == false)
    {
        printf("Pop failed\n");
    }

    bool try_peek = peek(stack, &peek_val);
    if (try_peek == false)
    {
        printf("Peek failed\n");
    }

    destroy_stack(stack);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Stack is empty
Stack size: 1
Stack size: 5
Stack is full
Push failed
Peek value: 8
Popped value: 8
Popped value: 7
Popped value: 4
Popped value: 9
Popped value: 2
Pop failed
Peek failed
```



### Real World Applications

#### 1. Function Call Management (Call Stack)

**Description:** Every time a function is called in a program, information about that function (like its local variables and return address) is stored on a **call stack**. When the function finishes execution, its information is popped off the stack.

**Real-World Analogy:** Imagine a stack of plates:

- **Push:** Adding a new plate on top when we call a function.
- **Pop:** Removing the top plate when the function returns.

**Use Case Example:**

- **Programming Languages:** Languages like C, Java, and Python use a call stack to manage function calls and returns efficiently.

**Visualization:**

```less
Top of Stack
-------------
| Function A |
-------------
| Function B |
-------------
| Function C |
-------------
Bottom of Stack
```

#### 2. Expression Evaluation

**Description:** Stacks are used to evaluate expressions and convert them between different notations (infix, postfix, prefix).

**Real-World Analogy:** Think of processing operations in a calculator:

- Operations are stored temporarily on a stack to manage the order of execution.

**Use Case Example:**

- **Compilers:** During the parsing phase, stacks help evaluate and convert arithmetic expressions.

**Visualization:**

Evaluating `3 + 4 * 2 / (1 - 5)^2^3` using a stack to handle operator precedence and parentheses.

#### 3. Undo Mechanisms in Applications

**Description:** Applications like text editors and graphic design tools use stacks to keep track of user actions, allowing users to undo operations in the reverse order they were performed.

**Real-World Analogy:** A stack of actions:

- **Push:** Each action (like typing a character) is pushed onto the stack.
- **Pop:** Undoing an action pops the latest action from the stack.

**Use Case Example:**

- **Microsoft Word:** Pressing `Ctrl + Z` undoes the last action by popping it from the undo stack.

**Visualization:**

```less
Undo Stack
-------------
| Action 3   |  <-- Last action to undo
-------------
| Action 2   |
-------------
| Action 1   |
-------------
```

#### 4. Browser Navigation

**Description:** Browsers use stacks to manage the history of visited pages, enabling the "Back" and "Forward" buttons functionality.

**Real-World Analogy:** Navigating back through a stack of web pages you've visited.

**Use Case Example:**

- **Web Browsers:** Each visited URL is pushed onto the back stack. When we click "Back," the top URL is popped off and displayed.

**Visualization:**

```less
Back Stack
-------------
| Page 3     |  <-- Current page
-------------
| Page 2     |
-------------
| Page 1     |
-------------
```

#### 5. Syntax Parsing

**Description:** Stacks are used in parsing the syntax of programming languages, ensuring that constructs like parentheses, brackets, and braces are properly opened and closed.

**Real-World Analogy:** Checking balanced parentheses in mathematical expressions using a stack.

**Use Case Example:**

- **Compilers and Interpreters:** Validating the correct nesting of symbols in code.

**Visualization:**

Validating the expression `(a + b) * (c + d)` using a stack to match parentheses.

---

## Queues

- A **Queue** is a linear data structure that follows the **First In, First Out (FIFO)** principle. Imagine a line at a ticket counter: the first person to join the line is the first to be served.

### **Why Use Queues?**

- **Order Processing:** Useful in scenarios like printer queues, CPU task scheduling.
- **Breadth-First Search (BFS):** Essential in graph traversal algorithms.
- **Buffer Management:** Commonly used in handling streaming data.

### Queue Operations

1. **Enqueue:** Add an element to the end of the queue.
   1. Enqueuing a new element typicaly requires (a) allocating a new `struct` to hold it; (b) making the old tail `struct` point at the new `struct`; and (c) updating the tail pointer to also point to the new `struct`.

2. **Dequeue :** Remove an element from the front of the queue.
   1. Dequeuing an element involves updating the head pointer and freeing the removed `struct`, exactly like a stack pop.

3. **Front:** View the front element without removing it.
4. **IsEmpty:** Check if the queue is empty.

### Queue Implementation Using Linked List

Using a linked list to implement a queue allows for **dynamic memory allocation**, enabling the queue to grow or shrink as needed without a fixed size limit.

### C Code Example # 1

#### Program Code

`practice.h`

```C
#define QUEUE_EMPTY INT_MIN

typedef struct node{
    int value;
    struct node *next;
} node;

typedef struct{
    node *head;
    node *tail;
} queue;

void init_queue(queue *q);

bool enqueue(queue *q, int value);

int dequeue(queue *q);
```

`functions.c`

```C
void init_queue(queue *q){
    q->head = NULL;
    q->tail = NULL;
}

bool enqueue(queue *q, int value){
    // create a new node
    node *newnode = malloc(sizeof(node));
    if(newnode == NULL){
        return false;
    }
    newnode->value = value;
    newnode->next = NULL;
    
    // if there's a tail, link it to the new node
    if(q->tail != NULL){
        q->tail->next = newnode;
    }
    q->tail = newnode;
    
    // if there's no head, set the head to the newnode
    if(q->head == NULL){
        q->head = newnode;
    }
    return true;
}

int dequeue(queue *q){
    // check to see if the queue is empty
    if(q->head == NULL){
        return QUEUE_EMPTY
    }
    
    // save the head of the queue
    node *tmp = q->head;
    
    // save the result we are going to return
    int result = tmp->value;
    
    // take it off
    q->head = q->head->next;
    if(q->head == NULL){
        q->tail = NULL;
    }
    free(tmp);
    return result;
}
```



`practice.c`

```C
int main()
{
    queue q1, q2, q3;

    init_queue(&q1);
    init_queue(&q2);
    init_queue(&q3);

    enqueue(&q1, 56);
    enqueue(&q2, 78);
    enqueue(&q2, 23);
    enqueue(&q2, 988);
    enqueue(&q3, 13);

    int t;
    while ((t = dequeue(&q2)) != QUEUE_EMPTY)
    {
        printf("t = %d\n", t);
    }
    return 0;
}
```

##### **Visualization**

**Queue Operations**

Let's visualize the queue operations step by step.

**1. Initial State**

- **Queues:** `q1`, `q2`, `q3` are empty.

  ```
  q1: head -> NULL, tail -> NULL
  
  q2: head -> NULL, tail -> NULL
  
  q3: head -> NULL, tail -> NULL
  ```

**2. Enqueue Operations**

- **Enqueue `56` to `q1`:**

  ```
  q1: head -> [56] -> NULL, tail -> [56]
  ```

- **Enqueue `78` to `q2`:**

  ```
  q2: head -> [78] -> NULL, tail -> [78]
  ```

- **Enqueue `23` to `q2`:**

  ```
  q2: head -> [78] -> [23] -> NULL, tail -> [23]
  ```

- **Enqueue `988` to `q2`:**

  ```
  q2: head -> [78] -> [23] -> [988] -> NULL, tail -> [988]
  ```

- **Enqueue `13` to `q3`:**

  ```
  q3: head -> [13] -> NULL, tail -> [13]
  ```

**3. Dequeue Operations from `q2`**

- **Dequeue from `q2`:**

  - First Dequeue:
    - Removes `78`.
    - Updates `head` to `23`.

  ```
  q2: head -> [23] -> [988] -> NULL, tail -> [988]
  ```

  - Second Dequeue:
    - Removes `23`.
    - Updates `head` to `988`.

  ```
  q2: head -> [988] -> NULL, tail -> [988]
  ```

  - Third Dequeue:
    - Removes `988`.
    - Updates `head` and `tail` to `NULL`.

  ```
  q2: head -> NULL, tail -> NULL
  ```

  **Final State**

- **Queues:**

  ```
  q1: head -> [56] -> NULL, tail -> [56]
  
  q2: head -> NULL, tail -> NULL
  
  q3: head -> [13] -> NULL, tail -> [13]
  ```

- **Output:**

```
t = 78

t = 23

t = 988
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
t = 78
t = 23
t = 988

```



### C Code Example #2

#### Program Code

`practice.h`

```C
typedef struct QueueNode{
    int data;
    struct QueueNode *next;
}QueueNode;

typedef struct{
    QueueNode *head;
    QueueNode *tail;
}Queue;

QueueNode *createNode(int data);
void enqueue(Queue *q, int new_data);
int dequeue(Queue *q);
int front(Queue *q);
int isEmpty(Queue *q);
void printQueue(Queue *q);
```

`functions.c`

```C
// create a new node with the given data and returns a pointer to it
QueueNode *createNode(int data){
    QueueNode *newNode = (QueueNode *)malloc(sizeof(QueueNode));
    if(!newNode){
        printf("Memory allocation error\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Adds a new element to the end of the queue
void enqueue(Queue *q, int new_data){
    QueueNode *newNode = createNode(new_data);
    
    // If the queue is empty, sets both head and tail to the new node
    if(q->tail == NULL){
        q->head = q->tail = newNode;
        printf("Enqueued %d to queue\n", new_data);
        return;
    }
    
    // Otherwise, linkes the new node to the current tail and updates the tail pointer
    //Connects the new node to the end of the existing list, effectively adding it after the current tail.
    q->tail->next = newNode;
    
    // Marks newNode as the new rear (tail) of the queue, ensuring that subsequent additions will follow after it.
    q->tail = newNode; // updates the tail pointer
    printf("Enqueued %d to queue\n", new_data);
}

// Removes and returns the element from the front of the queue
int dequeue(Queue *q){
    
    // Checks if the queue is empty
    if(q->head == NULL){
        printf("Queue underflow\n");
        exit(1);
    }
    
    // save the head node and its data
    QueueNode *temp = q->head;
    int dequeued = temp->data;
    
    // update the head pointer the next node
    q->head = temp->next;
    
    // If the queue becomes empty, set the tail pointer to NULL
    if(q->head == NULL){
        q->tail = NULL;
    }
    
    free(temp);
    
    // Return the data of the removed node
    return dequeued;
}

// Returns the front element of the queue
int front(Queue *q){
    if(q->head == NULL){
        printf("Queue is empty\n");
        exit(1);
    }
    return q->head->data;
}

int isEmpty(Queue *q){
    return q->head == NULL;
}

void printQueue(Queue *q){
    QueueNode *current = q->head;
    printf("Queue: ");
    while(current != NULL){
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}
```

##### Visualization of some code blocks

- ```C
  q->tail->next = newNode;
  q->tail = newNode;
  ```

```
[Head] --> [Node1] --> [Node2] --> NULL
                           ^
                        [Tail]

```

- **`q->head`** points to **Node1**.
- **`q->tail`** points to **Node2**.
- **Node2->next** is **NULL**.

New Node to Add:

```yaml
newNode: [Node3] --> NULL
```

After Executing `q->tail->next = newNode;`:

```css
[Front] --> [Node1] --> [Node2] --> [Node3] --> NULL

```

- **Node2->next** now points to **Node3**.
- **Node3->next** remains **NULL**.

After Executing `q->tail = newNode;`:

```css
[Front] --> [Node1] --> [Node2] --> [Node3] --> NULL
                                     ^
                                  [Tail]

```

- **`q->tail`** now points to **Node3**.

- **Node3** is the new rear of the queue.

##### Visualization with Enqueue Operations:

1. **Initial Empty Queue:**

```yaml
Queue Structure:
[Front] = NULL
[Tail] = NULL
```

2. **Enqueue 10:**

- Since the queue is empty, both `front` and `tail` point to the new node.

```less
Queue Structure:
[Front] --> [10] --> NULL
[Tail] --> [10] --> NULL
```

3. **Enqueue 20:**

- Link `tail->next` to the new node (20).
- Update `tail` to point to 20.

```less
Queue Structure:
[Front] --> [10] --> [20] --> NULL
[Tail] --> [20] --> NULL
```

4. **Enqueue 30:**

- Link `tail->next` to the new node (30).
- Update `tail` to point to 30.

```less
Queue Structure:
[Front] --> [10] --> [20] --> [30] --> NULL
[Tail] --> [30] --> NULL
```

`practice.c`

```C
int main(){
    Queue q;

    q.head = NULL;
    q.tail = NULL;

    enqueue(&q, 10);
    enqueue(&q, 20);
    enqueue(&q, 30);

    printQueue(&q);

    printf("Front element is %d\n", front(&q));

    printf("Dequeued element: %d\n", dequeue(&q));

    printQueue(&q);

    printf("Dequeued element: %d\n", dequeue(&q));

    printQueue(&q);

    printf("Is queue empty? %s\n", isEmpty(&q) ? "Yes" : "No");

    printf("Dequeued element: %d\n", dequeue(&q));

    printf("Is queue empty? %s\n", isEmpty(&q) ? "Yes" : "No");
    return 0;
}
```

##### Visualization

**Initial State**

- **Queue:** Empty (`head = NULL`, `tail = NULL`).

```
head -> NULL
tail -> NULL
```

**Enqueue Operations**

- **Enqueue `10`:**

  ```
  head -> [10] -> NULL
  tail -> [10]
  ```

- Enqueue `20`:

  ```
  head -> [10] -> [20] -> NULL
  tail -> [20]
  ```

- Enqueue `30`:

  ```
  head -> [10] -> [20] -> [30] -> NULL
  tail -> [30]
  ```

**Dequeue Operations**

- **Dequeue `10`:**

  ```
  head -> [20] -> [30] -> NULL
  
  tail -> [30]
  ```

- **Dequeue `20`:**

  ```
  head -> [30] -> NULL
  
  tail -> [30]
  ```

- **Dequeue `30`:**

  ```
  head -> NULL
  
  tail -> NULL
  ```

### **Final State**

- **Queue:** Empty (`head = NULL`, `tail = NULL`).

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Enqueued 10 to queue
Enqueued 20 to queue
Enqueued 30 to queue
Queue: 10 -> 20 -> 30 -> NULL
Front element is 10
Dequeued element: 10
Queue: 20 -> 30 -> NULL
Dequeued element: 20
Queue: 30 -> NULL
Is queue empty? No
Dequeued element: 30
Is queue empty? Yes
```



### C Code Example #3

#### Program Code

`practice.h`

```C
// Standard Link list element
typedef struct elt{
    struct elt *next;
    int value;
}Elt:

typedef struct queue{
    struct elt *head; // dequeue this next
    struct elt *tail; // enqueue after this
}Queue:

Queue *queueCreate(void);
void enq(Queue *q, int value);
int queueEmpty(const Queue *q);
int deq(Queue *q);
void queuePrint(const Queue *q);
void queueDestroy(Queue *q);
```

`functions.c`

```c
// create a new empty queue
Queue *queueCreate(void){
    Queue *q = malloc(sizeof(Queue));
    assert(q);
    q->head = q->tail = 0;
    
    return q;
}

// add a new value to back of queue
void enq(Queue *q, int value){
    Elt *e = malloc(sizeof(Queue));
    assert(e);
    
    e->value = value;
    
    // Because I will be the tail, nobody is behind me
    e->next = 0;
    
    if(q->tail == 0){
        // If the queue was empty, I become the head
        q->head = e;
    }else{
        // otherwise, I get in line after the old tail
        q->tail->next = e;
    }
    
    // I become the new tail
    q->tail = e;
}

int queueEmpty(const Queue *q){
    return q->head == 0;
}

// Remove and return value from front of queue
int deq(Queue *q){
    int ret;
    Elt *e;
    assert(!queueEmpty(q));
    
    ret = q->head->value;
    
    // patch out first element
    e = q->head;
    q->head = e->next;
    
    free(e);
    
    return ret;
}

// print contents of queue on a single line, head first
void queuePrint(const Queue *q){
    Elt *e;
    for(e = q->head; e != 0; e = e->next){
        printf("%d ", e->value);
    }
    putchar('\n');
}

// free a queue and all of its elements
void queueDestroy(Queue *q){
    while(!queueEmpty(q)){
        deq(q);
    }
    free(q);
}
```

`practice.c`

```C
int main()
{
    Queue *q = queueCreate();

    for (int i = 0; i < 5; i++)
    {
        printf("enq %d\n", i);
        enq(q, i);
        queuePrint(q);
    }

    while (!queueEmpty(q))
    {
        printf("deq gets %d\n", deq(q));
        queuePrint(q);
    }

    queueDestroy(q);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
enq 0
0 
enq 1
0 1 
enq 2
0 1 2 
enq 3
0 1 2 3 
enq 4
0 1 2 3 4 
deq gets 0
1 2 3 4 
deq gets 1
2 3 4 
deq gets 2
3 4 
deq gets 3
4 
deq gets 4

```

### Real World Applications:

#### 1. Order Processing Systems

**Description:** Queues manage the order in which tasks are processed, ensuring that the first task added is the first to be handled.

**Real-World Analogy:** A line of customers waiting at a checkout counter.

**Use Case Example:**

- **Customer Service:** Handling customer inquiries in the order they arrive.

**Visualization:**

```css
Front of Queue        Rear of Queue
     |                     |
     v                     v
[Customer1] -> [Customer2] -> [Customer3] -> NULL
```

#### 2. Print Queue

**Description:** Queues manage print jobs sent to a printer, processing them in the order they were received.

**Real-World Analogy:** A queue of print documents waiting to be printed.

**Use Case Example:**

- **Office Printers:** Documents are printed in the order they were submitted to the printer.

**Visualization:**

```less
Front of Print Queue        Rear of Print Queue
          |                             |
          v                             v
    [Doc1] -> [Doc2] -> [Doc3] -> NULL
```

#### 3. Task Scheduling

**Description:** Operating systems use queues to manage and schedule tasks or processes, ensuring efficient CPU utilization.

**Real-World Analogy:** A to-do list where tasks are executed in the order they are added.

**Use Case Example:**

- **CPU Scheduling:** Managing processes to execute based on priority and arrival time.

**Visualization:**

```less
Task Queue
-------------
| Task1      |
-------------
| Task2      |
-------------
| Task3      |
-------------
| Task4      |
-------------
```

#### 4. Breadth-First Search (BFS) in Graphs

**Description:** Queues are integral to BFS algorithms, which explore nodes level by level in graph or tree structures.

**Real-World Analogy:** Exploring friends of friends in a social network one level at a time.

**Use Case Example:**

- **Networking:** Finding the shortest path in network routing protocols.

**Visualization:**

```less
Queue during BFS:
[Node1] -> [Node2] -> [Node3] -> [Node4] -> NULL
```

#### 5. Buffering Data Streams

**Description:** Queues are used to manage data streams, buffering incoming data before processing to handle varying data rates.

**Real-World Analogy:** A conveyor belt where items are queued before being processed.

**Use Case Example:**

- **Streaming Services:** Buffering video data to ensure smooth playback.

**Visualization:**

```less
Data Buffer Queue
---------------------
| Data Packet1      |
---------------------
| Data Packet2      |
---------------------
| Data Packet3      |
---------------------
| ...               |
---------------------
```

---

## Deques

- Suppose we want a data structure that represents a line of elements where we can push or pop elements at either end.
- Such a data structure is known as a `deque` (pronounced like "deck") and can be implemented with all operations taking O(1) time by a **doubly-linked list**, where each element has a pointer to both its successor and its predecessor.
- A **Deque** (pronounced "deck") stands for **Double-Ended Queue**. 
- It's a data structure that allows insertion and deletion of elements from both the **front** and **rear** ends. This flexibility makes deques more versatile than standard queues or stacks.

**Key Features:**

- **Double-Ended:** Operations can be performed at both ends.
- **Dynamic Size:** Can grow or shrink as needed.
- **Efficient Operations:** Insertion and deletion at both ends occur in constant time.

### Structure Definition

A deque can be efficiently implemented using a **Doubly Linked List** because of its bidirectional pointers.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a deque (using doubly linked list)
struct DequeNode {
    int data;
    struct DequeNode* next;
    struct DequeNode* prev;
};

```

### Basic Operations

#### Insert Front

- Create a new node.
- Set the new node's `next` to the current front.
- Set the current front's `prev` to the new node.
- Update the front pointer to the new node.
- If the deque was empty, also set the rear pointer to the new node.

#### Insert Rear

- Create a new node.
- Set the current rear's `next` to the new node.
- Set the new node's `prev` to the current rear.
- Update the rear pointer to the new node.
- If the deque was empty, also set the front pointer to the new node.

#### Delete Front

- If the deque is empty, indicate underflow.
- Remove the front node.
- Update the front pointer to the next node.
- If the deque becomes empty, set the rear pointer to `NULL`.
- Free the removed node.

#### Delete Rear

- If the deque is empty, indicate underflow.
- Remove the rear node.
- Update the rear pointer to the previous node.
- If the deque becomes empty, set the front pointer to `NULL`.
- Free the removed node.

### C Code Example

#### Program Code

`practice.h`

```C
struct DequeNode
{
    int data;
    struct DequeNode *next;
    struct DequeNode *prev;
};

typedef struct DequeNode DequeNode;

DequeNode *createNode(int data);
void insertFront(DequeNode **head, DequeNode **tail, int data);
void insertRear(DequeNode **head, DequeNode **tail, int data);

int deleteFront(DequeNode **head, DequeNode **tail);

int deleteRear(DequeNode **head, DequeNode **tail);

void traverseDeque(DequeNode *head);
```

`functions.c`

```C
DequeNode *createNode(int data){
    DequeNode *newNode = malloc(sizeof(DequeNode));
    if(!newNode){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void insertFront(DequeNode **head, DequeNode **tail, int data){
    DequeNode *newNode = createNode(data);
    newNode->next = *head;
    
    // if there's a current head node, set that node prev pointer to newNode placing the newNode before that.
    if(*head != NULL){
        *head->prev = newNode;
    }
    
    // the newNode now becomes the head element.
    *head = newNode;
    
    // If deque was empty
    if(*tail == NULL){
        *tail = newNode;
    }
    
    printf("Inserted %d at the front\n", data);
}

void insertRear(DequeNode **head, DequeNode **tail, int data){
    DequeNode *newNode = createNode(data);
    newNode->prev = *tail;
    
    // if there's a tail node, set that node's next pointer to point to the newNode, placing the newNode after that.
    if(*tail != NULL){
        (*tail)->next = newNode;
    }
    
    // The newNode becomes the tail element
    *tail = newNode;
    
    // if deque was empty
    if(*head == NULL){
        *head = newNode;
    }
    
    printf("Inserted %d at the rear\n", data);
}

int deleteFront(DequeNode **head, DequeNode **tail){
    if(*head == NULL){
        printf("Deque is empty\n");
        exit(1);
    }
    
    // store the current head node in a temp node
    dequeNode *temp = *head;
    
    // store its data in data variable
    int data = temp->data;
    
    // set the head pointer to point to the next node
    *head = temp->next;
    
    if(*head != NULL){
        (*head)->prev = NULL;
    }else{ // if deque becomes empty
        *tail = NULL;
    }
    
    free(temp);
    printf("Deleted %d from the front\n", data);
    return data.
}

int deleteRear(DequeNode **head, DequeNode **tail){
    if(*tail == NULL){
        printf("Deque is empty\n");
        exit(1);
    }
    
    DequeNode *temp = *tail;
    int data = temp->data;
    *tail = temp->prev;
    
    if(*tail != NULL){
        (*tail)->next = NULL;
    }else{
        *head = NULL;
    }
    
    free(temp);
    printf("Deleted %d from the rear.\n", data);
    return data;
}

void traverseDeque(DequeNode *head){
    printf("Deque (Front to Rear): ");
    while(head != NULL){
        printf("%d ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}
```

`practice.c`

```C
int main()
{
    DequeNode *head = NULL;
    DequeNode *tail = NULL;

    // Insert elements
    insertFront(&head, &tail, 10); // Deque: 10
    traverseDeque(head);

    insertRear(&head, &tail, 20); // Deque: 10 <-> 20
    traverseDeque(head);

    insertFront(&head, &tail, 5); // Deque: 5 <-> 10 <-> 20
    traverseDeque(head);

    insertRear(&head, &tail, 25); // Deque: 5 <-> 10 <-> 20 <-> 25
    traverseDeque(head);

    // Delete elements
    deleteFront(&head, &tail); // Deque: 10 <-> 20 <-> 25
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque: 10 <-> 20
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque: 10
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque is empty
    traverseDeque(head);

    free(head);
    head = NULL;
    tail = NULL;

    printf("Deque is empty\n");

    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 at the front
Deque (Front to Rear): 10 NULL
Inserted 20 at the rear
Deque (Front to Rear): 10 20 NULL
Inserted 5 at the front
Deque (Front to Rear): 5 10 20 NULL
Inserted 25 at the rear
Deque (Front to Rear): 5 10 20 25 NULL
Deleted 5 from the front
Deque (Front to Rear): 10 20 25 NULL
Deleted 25 from the rear.
Deque (Front to Rear): 10 20 NULL
Deleted 20 from the rear.
Deque (Front to Rear): 10 NULL
Deleted 10 from the rear.
Deque (Front to Rear): NULL
Deque is empty

```



### Summary

#### Deques (Double-Ended Queues)

- **Double-Ended:** Supports insertion and deletion from both the front and rear ends.
- **LIFO and FIFO Combined:** Can function as both a stack and a queue.
- **Dynamic Size:** Grows and shrinks as needed without fixed size limitations.
- **Implementation:** Often implemented using a doubly linked list for efficient operations.
- **Use Cases:** Sliding window algorithms, task scheduling, browser history management, etc.

---

## Ring Buffer

A **Ring Buffer** (also known as a **Circular Buffer**) is a fixed-size data structure that uses a single, fixed-size buffer as if it were connected end-to-end. It efficiently manages data in a **First-In-First-Out (FIFO)** manner, making it ideal for scenarios where data streams continuously, such as audio processing, networking, and real-time systems.

**Key Features:**

- **Fixed Size:** Predefined capacity.
- **Circular Nature:** Wraps around to the beginning when the end is reached.
- **Efficient Memory Use:** Avoids the need to shift data after dequeue operations.
- **FIFO Behavior:** Maintains the order of data.

### Structure Definition

A ring buffer typically maintains three components:

1. **Buffer Array:** Holds the data.
2. **Head Pointer:** Points to the next position to dequeue (read).
3. **Tail Pointer:** Points to the next position to enqueue (write).

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define BUFFER_SIZE 5 // Define the size of the ring buffer

typedef struct {
    int buffer[BUFFER_SIZE];
    int head; // Points to the next element to dequeue
    int tail; // Points to the next position to enqueue
    int count; // Number of elements in the buffer
} RingBuffer;
```



### C implementation # 1

#### Program Code

`practice.h`

```C
#define BUFFER_SIZE 5
typedef struct{
    int buffer[BUFFER_SIZE];
    int head;
    int tail;
    int count;
} RingBuffer;

void initRingBuffer(RingBuffer *rb);

bool enqueue(RingBuffer *rb, int data);

bool dequeue(RingBuffer *rb, int *data);

void displayBuffer(RingBuffer *rb);
```

`functions.c`

```C
void initRingBuffer(RingBuffer *rb){
    rb->head = 0;
    rb->tail = 0;
    rb->count = 0;
}

bool enqueue(RingBuffer *rb, int data){
    if(rb->count == BUFFER_SIZE){
        printf("Buffer is full. Cannot enqueue %d.\n", data);
        return false;
    }
    
    rb->buffer[rb->tail] = data;
    rb->tail = (rb->tail + 1) % BUFFER_SIZE;
    rb->count++;
    printf("Enqueued %d to buffer.\n", data);
    return true;
}

bool dequeue(RingBuffer *rb, int *data){
    if(rb->count == 0){
        printf("Buffer is empty. Cannot dequeue.\n");
        return false;
    }
    
    *data = rb->buffer[rb->head];
    rb->head = (rb->head + 1) % BUFFER_SIZE;
    rb->count--;
    printf("Dequeued %d from buffer.\n", *data);
    return true;
}

void displayBuffer(RingBuffer *rb){
    printf("Buffer state: ");
    for(int i = 0; i < rb->count; i++){
        int index = (rb->head + i) % BUFFER_SIZE;
        printf("%d ", rb->buffer[index]);
    }
    printf("\n");
}
```

`practice.c`

```C
int main()
{
    RingBuffer rb;
    initRingBuffer(&rb);

    // Enqueue elements
    enqueue(&rb, 10);
    enqueue(&rb, 20);
    enqueue(&rb, 30);
    enqueue(&rb, 40);
    enqueue(&rb, 50);
    enqueue(&rb, 60); // should indicate buffer is full

    displayBuffer(&rb); // expected output: 10 20 30 40 50

    // Dequeue elements
    int data;
    dequeue(&rb, &data);
    dequeue(&rb, &data);

    displayBuffer(&rb); // expected output: 30 40 50

    // Enqueue more elements
    enqueue(&rb, 60);
    enqueue(&rb, 70);

    displayBuffer(&rb); // expected output: 30 40 50 60 70

    // Attempt to enqueue another element
    enqueue(&rb, 80); // should indicate buffer is full
    return 0;
}
```

##### Visualization

Initial State:

```makefile
Buffer: [ _ | _ | _ | _ | _ ]
Head: 0
Tail: 0
Count: 0
```

After Enqueue 10:

```makefile
Buffer: [10 | _ | _ | _ | _ ]
Head: 0
Tail: 1
Count: 1
```

After Enqueue 20:

```makefile
Buffer: [10 | 20 | _ | _ | _ ]
Head: 0
Tail: 2
Count: 2
```

After Enqueue 30:

```makefile
Buffer: [10 | 20 | 30 | _ | _ ]
Head: 0
Tail: 3
Count: 3
```

After Enqueue 40:

```makefile
Buffer: [10 | 20 | 30 | 40 | _ ]
Head: 0
Tail: 4
Count: 4
```

After Enqueue 50:

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0
Tail: 0 (wraps around)
Count: 5
```

Attempting to Enqueue 60 (Buffer Full):

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0
Tail: 0
Count: 5
```

- **Result:** Cannot enqueue 60. Buffer is full.

After Dequeue 10 and 20:

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 2
Tail: 0
Count: 3
```

- **Elements in Buffer:** 30, 40, 50

After Enqueue 60 and 70:

```makefile
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2
Tail: 2
Count: 5
```

- **Buffer Wraps Around:** Tail moves to position 2 after enqueuing 60 and 70.

Attempting to Enqueue 80 (Buffer Full):

```makefile
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2
Tail: 2
Count: 5
```

- **Result:** Cannot enqueue 80. Buffer is full.

**Diagram Representation:**

```yaml
Step-by-Step Enqueue and Dequeue Operations:

Initial State:
Buffer: [ _ | _ | _ | _ | _ ]
Head: 0, Tail: 0, Count: 0

Enqueue 10:
Buffer: [10 | _ | _ | _ | _ ]
Head: 0, Tail: 1, Count: 1

Enqueue 20:
Buffer: [10 | 20 | _ | _ | _ ]
Head: 0, Tail: 2, Count: 2

Enqueue 30:
Buffer: [10 | 20 | 30 | _ | _ ]
Head: 0, Tail: 3, Count: 3

Enqueue 40:
Buffer: [10 | 20 | 30 | 40 | _ ]
Head: 0, Tail: 4, Count: 4

Enqueue 50:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0, Tail: 0, Count: 5

Attempt Enqueue 60 (Full):
Buffer remains unchanged.
Cannot enqueue 60.

Dequeue 10:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 1, Tail: 0, Count: 4

Dequeue 20:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 2, Tail: 0, Count: 3

Enqueue 60:
Buffer: [60 | 20 | 30 | 40 | 50 ]
Head: 2, Tail: 1, Count: 4

Enqueue 70:
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2, Tail: 2, Count: 5

Attempt Enqueue 80 (Full):
Buffer remains unchanged.
Cannot enqueue 80.
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Enqueued 10 to buffer.
Enqueued 20 to buffer.
Enqueued 30 to buffer.
Enqueued 40 to buffer.
Enqueued 50 to buffer.
Buffer is full. Cannot enqueue 60.
Buffer state: 10 20 30 40 50 
Dequeued 10 from buffer.
Dequeued 20 from buffer.
Buffer state: 30 40 50 
Enqueued 60 to buffer.
Enqueued 70 to buffer.
Buffer state: 30 40 50 60 70 
Buffer is full. Cannot enqueue 80.
```

### Use Cases

**Ring Buffers** are particularly useful in scenarios where data streams continuously, and there’s a need for efficient, fixed-size buffering without dynamic memory allocation. Some common real-world applications include:

1. **Data Streaming:**
   - **Audio and Video Processing:** Managing real-time data streams where data is continuously produced and consumed.
   - **Networking:** Handling incoming and outgoing packets in network devices.
2. **Embedded Systems:**
   - **Microcontrollers:** Efficiently managing limited memory resources for real-time tasks.
3. **Operating Systems:**
   - **I/O Buffers:** Managing data between hardware devices and applications.
4. **Circular Queues:**
   - Implementing fixed-size queues in situations where the queue size is known and constant.

