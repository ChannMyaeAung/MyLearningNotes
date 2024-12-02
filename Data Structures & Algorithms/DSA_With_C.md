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

## Stacks

- A **Stack** is a linear data structure that follows the **Last In, First Out (LIFO)** principle. Think of it like a stack of plates: we add (push) plates to the top and remove (pop) plates from the top.
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

// Function to create a new stack node
struct StackNode* createNode(int data) {
    struct StackNode* newNode = (struct StackNode*) malloc(sizeof(struct StackNode));
    if (!newNode) { // Check for malloc failure
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Push operation: Add an element to the stack
void push(struct StackNode** top_ref, int new_data) {
    struct StackNode* new_node = createNode(new_data);
    new_node->next = *top_ref; // Link the old stack to the new node
    *top_ref = new_node;       // Update the top pointer
    printf("Pushed %d to stack\n", new_data);
}

// Pop operation: Remove an element from the stack
int pop(struct StackNode** top_ref) {
    if (*top_ref == NULL) {
        printf("Stack Underflow\n");
        exit(1);
    }
    struct StackNode* temp = *top_ref;
    int popped = temp->data;
    *top_ref = temp->next; // Move the top pointer to the next node
    free(temp);            // Free memory
    return popped;
}

// Peek operation: Get the top element without removing it
int peek(struct StackNode* top) {
    if (top == NULL) {
        printf("Stack is empty\n");
        exit(1);
    }
    return top->data;
}

// Check if the stack is empty
int isEmpty(struct StackNode* top) {
    return (top == NULL);
}

// Function to print the stack
void printStack(struct StackNode* top) {
    printf("Stack: ");
    while (top != NULL) {
        printf("%d -> ", top->data);
        top = top->next;
    }
    printf("NULL\n");
}

// Driver program to test the stack operations
int main() {
    struct StackNode* stack = NULL; // Initialize stack as empty

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

    // patch out first element
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

**1. `stackPush(Stack \*s, int value)`**

- **Purpose**: Adds a new element with the given `value` to the top of the stack.
- **Steps**:
  - Allocate memory for a new element.
  - Set the new element's `value` to the given `value`.
  - Point the new element's `next` to the current top of the stack (`*s`).
  - Update the stack pointer `*s` to point to the new element.

**2. `int stackPop(Stack \*s)`**

- **Purpose**: Removes and returns the value from the top element of the stack.
- **Steps**:
  - Ensure the stack is not empty.
  - Store the value of the top element.
  - Move the stack pointer `*s` to the next element (`(*s)->next`), effectively removing the top element.
  - Free the memory allocated for the removed element.
  - Return the stored value.

**3. `int stackEmpty(const Stack \*s)`**

- **Purpose**: Checks if the stack is empty.
- **Returns**: `1` (true) if the stack is empty (`*s == 0`), `0` (false) otherwise.

**4. `void stackPrint(const Stack \*s)`**

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

---

## Queues

- A **Queue** is a linear data structure that follows the **First In, First Out (FIFO)** principle. Imagine a line at a ticket counter: the first person to join the line is the first to be served.

### Queue Operations

1. **Enqueue:** Add an element to the end of the queue.
2. **Dequeue:** Remove an element from the front of the queue.
3. **Front:** View the front element without removing it.
4. **IsEmpty:** Check if the queue is empty.

### Queue Implementation Using Linked List

Using a linked list to implement a queue allows for **dynamic memory allocation**, enabling the queue to grow or shrink as needed without a fixed size limit.

### C Code Example

#### Program Code

```C
```

### Program Output

```shell
```

