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



### What linked lists are and are not good for

- Linked lists are good for any task that involves inserting and deleting elements next to an element we already have a pointer to.
- Such operations can usually be done in O(1) time.
- They generally beat arrays (even resizable arrays) if we need to insert or delete in the middle of a list, since an array has to copy any elements above the insertion point to make room.
- If insertion or deletion always happen at the end, an array may be better.
- Linked lists are not good for any operation that requries random access, since reaching an arbitrary element of a linked list takes as much as O(n) time.
- For such applications, arrays are better if we don't need to insert in the middle.
- If we do, we should use some sort of tree.

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

- The idea of a ring buffer is to store the deque elements in an array, with a pointer to the first element and a length field that says how many elements are in the deque. The information needed to manage the array (which is allocated using `malloc`) is stored in a `struct`.

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
    
    // Adds the new element at the tail position
    rb->buffer[rb->tail] = data;
    
    // Updates the tail pointer to the next position, wrapping around if necessary
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
    // Iterates thru the buffer from the head to the tail, printing each element.
    for(int i = 0; i < rb->count; i++){
        // Uses modulo arithmetic to handle the circular nature of the buffer.
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



### C Implementation # 2

#### Program Code

`practice.h`

```C
#define INITIAL_SIZE 8
#define DEQUE_FRONT 0
#define DEQUE_BACK 1
#define DEQUE_EMPTY -1
struct deque
{
    size_t base;
    size_t length;
    size_t size;
    int *contents;
};
typedef struct deque Deque;

Deque *dequeCreate(void);
void dequePush(Deque *d, int direction, int value);
int dequePop(Deque *d, int direction);
int dequeIsEmpty(const Deque *d);
void dequeDestroy(Deque *d);
```

`functions.c`

```C
static Deque *dequeCreateInternal(size_t size){
    Deque *d = malloc(sizeof(Deque));
    assert(d);
    
    d->base = 0;
    d->length = 0;
    d->size = size;
    
    d->contents = malloc(sizeof(int) * d->size);
    assert(d->contents);
    
    return d;
}

Deque *dequeCreate(void){
    return dequeCreateInternal(INITIAL_SIZE);
}

void dequePush(Deque *d, int direction, int value){
    // replacement deque if we grow
    Deque *d2;
    
    // old contents of d
    int *oldContents;
    
    // first make sure we have space
    // resize if deque is full
    if(d->length == d->size){
        d2 = dequeCreateInternal(d->size * 2);
        
        while(!dequeIsEmpty(d)){
            dequePush(d, DEQUE_BACK, dequePop(d, DEQUE_FRONT));
        }
        
        // do a transplant from d2 to d
        // but save old contents so we can free them
        oldContents = d->contents;
        *d = *d2; // this is equivalent to copying the components one by one
        free(oldContents);
        free(d2);
    }
    
    // depending on driection, updates base and inserts the value
    if(direction == DEQUE_FRONT){
        // d->base is unsigned, so we have to check for zero first
        if(d->base == 0){
            d->base = d->size - 1;
        }else{
            d->base--;
        }
        
        d->length++;
        d->contents[d->base] = value;
    }else{
        d->contents[(d->base + d->length++) % d->size] = value;
    }
}

int dequePop(Deque *d, int direction){
    int retval;
    
    if(dequeEmpty(d)){
        return DEQUE_EMPTY;
    }
    
    if(direction == DEQUE_FRONT){
        // base goes up by one, length goes down by one
        retval = d->contents[d->base];
        
        d->base = (d->base + 1) % d->size;
        d->length--;
        
        return retval;
    }else{
        return d->contents[(d->base + --d->length) % d->size];
    }
}

int dequeIsEmpty(const Deque *d){
    return d->length == 0;
}

void dequeDestroy(Deque *d){
    free(d->contents);
    free(d);
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        fprintf(stderr, "Usage: %s <iterations>\n", argv[0]);
        return 1;
    }

    long iterations = atol(argv[1]);
    if (iterations <= 0)
    {
        fprintf(stderr, "Iterations must be a positive integer.\n");
        return 1;
    }

    Deque *d = dequeCreate();

    // push elements to the front and back
    for (long i = 0; i < iterations; i++)
    {
        dequePush(d, DEQUE_FRONT, i);
        dequePush(d, DEQUE_BACK, i + iterations);

        // Print only the first 10 iterations
        if (i < 10)
        {
            printf("Pushed Front: %ld, Back: %ld\n", i, i + iterations);
        }
    }

    // pop elements from the front and back
    for (long i = 0; i < iterations; i++)
    {
        int front = dequePop(d, DEQUE_FRONT);
        int back = dequePop(d, DEQUE_BACK);
        if (front != DEQUE_EMPTY && back != DEQUE_EMPTY)
        {
            if (i < 10)
            {
                printf("Pop from front: %d\n", front);
                printf("Pop from back: %d\n", back);
            }
        }
    }

    // check if the deque is empty
    printf("Deque is empty: %s\n", dequeIsEmpty(d) ? "Yes" : "No");

    // destroy the deque
    dequeDestroy(d);

    return 0;
}
```

`Makefile`

```makefile
# Compiler and standard
CC = clang
STD = -std=c23
# Compiler flags
CFLAGS = -Wall -Wextra -g
LIBS = -lpthread -lm -lssl -lcrypto

# Directories
LIBDIR = ./libs
OBJDIR = ./obj

# Iterations 
ITERATIONS = 10000000
VALGRIND_ITERATIONS = 100

all:practice

practice: $(OBJDIR)/practice.o $(LIBDIR)/libfunctions.a
	$(CC) $(STD) $(OBJDIR)/practice.o -L$(LIBDIR) -lfunctions -o practice $(LIBS)

$(OBJDIR)/practice.o: practice.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c practice.c -o $(OBJDIR)/practice.o 

$(OBJDIR)/functions.o: functions.c practice.h
	$(CC) $(STD) $(CFLAGS) -c functions.c -o $(OBJDIR)/functions.o 

$(OBJDIR)/functions_2.o: functions_2.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c functions_2.c -o $(OBJDIR)/functions_2.o

$(LIBDIR)/libfunctions.a: $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o 
	ar -rcs $(LIBDIR)/libfunctions.a $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o

run: practice
	./practice $(ITERATIONS)

valgrind: practice
	valgrind --leak-check=full ./practice $(VALGRIND_ITERATIONS)

clean:
	@echo "Removing everything except the source files..."
	rm -f $(OBJDIR)/*.o $(LIBDIR)/libfunctions.a practice
```



#### Program Output

- Ran 100 times to test the code

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 100
==15787== Memcheck, a memory error detector
==15787== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15787== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15787== Command: ./practice 100
==15787== 
Pushed Front: 0, Back: 100
Pushed Front: 1, Back: 101
Pushed Front: 2, Back: 102
Pushed Front: 3, Back: 103
Pushed Front: 4, Back: 104
Pushed Front: 5, Back: 105
Pushed Front: 6, Back: 106
Pushed Front: 7, Back: 107
Pushed Front: 8, Back: 108
Pushed Front: 9, Back: 109
Pop from front: 99
Pop from back: 199
Pop from front: 98
Pop from back: 198
Pop from front: 97
Pop from back: 197
Pop from front: 96
Pop from back: 196
Pop from front: 95
Pop from back: 195
Pop from front: 94
Pop from back: 194
Pop from front: 93
Pop from back: 193
Pop from front: 92
Pop from back: 192
Pop from front: 91
Pop from back: 191
Pop from front: 90
Pop from back: 190
Deque is empty: Yes
==15787== 
==15787== HEAP SUMMARY:
==15787==     in use at exit: 0 bytes in 0 blocks
==15787==   total heap usage: 13 allocs, 13 frees, 3,232 bytes allocated
==15787== 
==15787== All heap blocks were freed -- no leaks are possible
==15787== 
==15787== For lists of detected and suppressed errors, rerun with: -s
==15787== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

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

---

## Abstract data types

- Abstract data types are data types whose implementation is not visible to their user.
- From the outside, all the user knows about an ADT is what  operations can be performed on it and what those operations are supposed to do.
- ADTs have an outside and an inside.
  - The outside is called the **interface**. It consists of the minimal set of type and function declarations needed to use the ADT.
  - The inside is called the **implementation**. It consists of type and function definitions, and sometimes auxiliary data or helper functions, that are not visible to users of the ADT.
- This separation between interface and implementation is called an **abstraction barrier**, and allows the implementation to change without affecting the rest of the program.
- What joins the implementation to the interface is an **abstraction function**.
  - This is a function (in the mathematical sense) that takes any state of the implementation and trims off any irrelevant details to leave behind an idealized pictures of what the data type is doing.
  - For example, a linked list implementation translates to a sequence abstract data type by forgetting about the pointers used to hook up the elements and just keeping the sequence of elements themselves.
  - To exclude bad states of implementation (for example, a singly-linked list that loops back on itself instead of having a terminating null pointer), we may have a **representation invaraint**, which is just some property of the implementation that is always true.
    - Representation invariant are also useful for detecting when we've bungled our implementation, and a good debugging strategy for misbehaving abstract data type implementations is often to look for the first point at which they violated some property that we thought was an invariant. 

### A sequence type

```C
void seq_print(Sequence s, int limit){
    int i;
    
    for(i = seq_next(s); i < limit; i = seq_next(s)){
        printf("%d\n", i);
    }
}
```

- `seq_print` doesn't need to know anything at all about what a `Sequence` is or how `seq_next` works in order to print out all the values in the sequence until it hits one greater than or equal to limit. 
- This is a good thing. It means that we can use with any implementation of `Sequence` we like, and we don't have to change it if `Sequence` or `seq_next` changes.



### Interface

In C, the interface of an abstract data type will usually be declared in a header file, which is included both in the file that implements the ADT (so that the compiler can check that the declarations match up with the actual definition in the implementation).

```C
// stack.h
#ifndef STACK_H
#define STACK_H

// Forward declaration of the Stack structure (opaque pointer)
typedef struct Stack Stack;

// Function to create a new stack
Stack* createStack();

// Function to push an element onto the stack
void push(Stack* stack, int data);

// Function to pop an element from the stack
int pop(Stack* stack);

// Function to peek at the top element without removing it
int peek(Stack* stack);

// Function to check if the stack is empty
int isEmpty(Stack* stack);

// Function to destroy the stack and free memory
void destroyStack(Stack* stack);

#endif // STACK_H
```



### Implementation

The implementation of an ADT in C is typically contained in one (or sometimes more than one) `.c` file. This file can be compiled and linked into any program that needs to use the ADT.

```C
// stack.c
#include <stdio.h>
#include <stdlib.h>
#include "stack.h"

// Definition of the node structure
typedef struct Node {
    int data;
    struct Node* next;
} Node;

// Definition of the Stack structure
struct Stack {
    Node* top;
};

// Function to create a new stack
Stack* createStack() {
    Stack* stack = (Stack*) malloc(sizeof(Stack));
    if (!stack) {
        printf("Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
    stack->top = NULL;
    return stack;
}

// Function to push an element onto the stack
void push(Stack* stack, int data) {
    if (!stack) return;
    Node* newNode = (Node*) malloc(sizeof(Node));
    if (!newNode) {
        printf("Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
    newNode->data = data;
    newNode->next = stack->top;
    stack->top = newNode;
    printf("Pushed %d onto the stack.\n", data);
}

// Function to pop an element from the stack
int pop(Stack* stack) {
    if (!stack || !stack->top) {
        printf("Stack Underflow! Cannot pop.\n");
        exit(EXIT_FAILURE);
    }
    Node* temp = stack->top;
    int popped = temp->data;
    stack->top = temp->next;
    free(temp);
    printf("Popped %d from the stack.\n", popped);
    return popped;
}

// Function to peek at the top element without removing it
int peek(Stack* stack) {
    if (!stack || !stack->top) {
        printf("Stack is empty! Nothing to peek.\n");
        exit(EXIT_FAILURE);
    }
    return stack->top->data;
}

// Function to check if the stack is empty
int isEmpty(Stack* stack) {
    if (!stack) return 1;
    return (stack->top == NULL);
}

// Function to destroy the stack and free memory
void destroyStack(Stack* stack) {
    if (!stack) return;
    while (!isEmpty(stack)) {
        pop(stack);
    }
    free(stack);
    printf("Stack destroyed.\n");
}

```



### Using the ADT (Main Program)

```C
// main.c
#include <stdio.h>
#include "stack.h"

int main() {
    // Create a new stack
    Stack* stack = createStack();
    
    // Push elements onto the stack
    push(stack, 10);
    push(stack, 20);
    push(stack, 30);
    
    // Peek at the top element
    printf("Top element is %d\n", peek(stack)); // Output: 30
    
    // Pop elements from the stack
    pop(stack); // Output: Popped 30 from the stack.
    pop(stack); // Output: Popped 20 from the stack.
    
    // Check if stack is empty
    if (isEmpty(stack)) {
        printf("Stack is empty.\n");
    } else {
        printf("Stack is not empty.\n");
    }
    
    // Destroy the stack
    destroyStack(stack); // Output: Popped 10 from the stack.
                          //         Stack destroyed.
    
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Pushed 10 onto the stack.
Pushed 20 onto the stack.
Pushed 30 onto the stack.
Top element is 30
Popped 30 from the stack.
Popped 20 from the stack.
Stack is not empty
Popped 10 from the stack.
Stack destroyed.
```

### Benefits of Using ADTs

1. Abstraction:
   - **Focus on Interface:** Users interact with ADTs through their defined operations without needing to understand their internal workings.
2. Modularity:
   - **Separate Implementation:** Changes to the ADT's implementation do not affect code that uses the ADT, as long as the interface remains consistent.
3. Reusability:
   - **Reusable Components:** Once an ADT is implemented, it can be reused across different projects and programs.
4. Maintainability:
   - **Easier Maintenance:** Bugs and enhancements can be addressed within the ADT's implementation without altering the user's code.
5. Data Hiding and Encapsulation:
   - **Protect Data Integrity:** By hiding internal data structures, ADTs prevent unintended interference, ensuring that data is accessed and modified only through defined operations.

------

### Summary

- **Abstract Data Types (ADTs)** are **theoretical constructs** that define data structures by their **operations** and **behaviors** rather than their implementation.
- **Benefits of ADTs:**
  - Promote **abstraction**, **modularity**, **reusability**, **maintainability**, and **data hiding**.
- **Common ADTs:** Stack, Queue, Linked List, Tree.
- **Implementing ADTs in C:**
  - Use **structures** to define data layouts.
  - Use **functions** to define operations.
  - Utilize **header files** (`.h`) for declaring interfaces.
  - Employ **opaque pointers** to hide implementation details and enforce encapsulation.
- **Example Implementation:** A Stack ADT using a singly linked list, demonstrating how to separate interface from implementation and use opaque pointers for data hiding.

**Key Takeaways:**

- **Understand the Interface:** Know what operations an ADT should support based on its definition and use cases.
- **Separate Interface and Implementation:** Use header files and source files to define interfaces and implement functionalities separately.
- **Encapsulate Data:** Hide internal data structures from users to protect data integrity and allow implementation flexibility.

---

## Hash Table

A **Hash Table** (also known as a **Hash Map**) is a data structure that implements an **associative array**—a structure that can map **keys** to **values**. It allows for **efficient** insertion, deletion, and lookup operations, typically achieving **O(1)** time complexity on average.

**Key Characteristics:**

- **Key-Value Pairs:** Each entry in the hash table consists of a unique key and its associated value.
- **Hash Function:** A function that converts a key into an index in the underlying array.
- **Buckets:** The array that holds the data. Each bucket can store one or more key-value pairs.
- **Collision Handling:** Mechanisms to handle scenarios where multiple keys hash to the same index.

**Real-World Analogy:**

Imagine a library where books (values) are stored on shelves (buckets). Each book has a unique ISBN (key). The hash function determines which shelf a book should go on based on its ISBN. If two books have ISBNs that hash to the same shelf, collision handling ensures both books are stored without loss.

### Components of a Hash Table

1. **Keys and Values:**
   - **Key:** A unique identifier used to store and retrieve associated data.
   - **Value:** The data associated with a key.
2. **Hash Function:**
   - Converts a key into an index within the hash table's array.
   - Should distribute keys uniformly to minimize collisions.
3. **Buckets (Array):**
   - An array where each element is a bucket that can store key-value pairs.
   - The size of the array is typically a prime number to reduce collisions.
4. **Collision Handling Mechanism:**
   - **Separate Chaining:** Each bucket contains a linked list of entries that hash to the same index.
   - **Open Addressing:** Finds another slot within the array using probing methods (linear, quadratic, double hashing).

### Hash Functions

A **hash function** is crucial for the performance of a hash table. It must efficiently convert keys into valid array indices with minimal collisions.

**Properties of a Good Hash Function:**

- **Deterministic:** The same key always hashes to the same index.
- **Uniform Distribution:** Hashes keys uniformly across the array to minimize collisions.
- **Efficient:** Computes quickly, especially for large datasets.

**Simple Example:**

A basic hash function for integer keys might be:

```C
int hashFunction(int key, int size){
    return key % size;
}
```

- **Explanation:** It divides the key by the size of the hash table and returns the remainder as the index.

### Collision Handling Techniques

Collisions occur when two distinct keys hash to the same index. Effective collision handling ensures that all key-value pairs are stored and retrieved correctly.

#### Separate Chaining

**Description:**

- Each bucket in the array contains a linked list.
- All entries that hash to the same index are stored in the linked list for that bucket.

**Advantages:**

- Simple to implement.
- Efficient when the load factor (number of entries divided by number of buckets) is low.
- Can handle a large number of collisions gracefully.

**Disadvantages:**

- Requires additional memory for pointers in the linked lists.
- Performance degrades if many keys hash to the same bucket.

**Visualization:**

```yaml
Hash Table Array:
Index 0: [Key1, Value1] -> [Key2, Value2] -> NULL
Index 1: NULL
Index 2: [Key3, Value3] -> NULL
...
```

### Open Addressing

**Description:**

- All key-value pairs are stored directly within the array.
- When a collision occurs, the hash table probes for the next available slot using a probing sequence.

**Common Probing Methods:**

1. **Linear Probing:**
   - Moves sequentially through the array to find the next available slot.
   - **Formula:** `(hash + i) % size` where `i` is the probe number.
2. **Quadratic Probing:**
   - Uses a quadratic function to determine the probe sequence.
   - **Formula:** `(hash + i^2) % size`.
3. **Double Hashing:**
   - Uses a second hash function to determine the probe step size.
   - **Formula:** `(hash1 + i * hash2(key)) % size`.

**Advantages:**

- No additional memory overhead for pointers.
- Better cache performance due to data locality.

**Disadvantages:**

- More complex to implement.
- Clustering can occur, especially with linear probing.
- Deletion can be tricky to handle.

**Visualization:**

```yaml
Hash Table Array:
Index 0: [Key1, Value1]
Index 1: [Key2, Value2]
Index 2: NULL
...
```

### Basic Operations

#### Insertion

**Separate Chaining:**

1. Compute the hash index using the hash function.
2. Insert the key-value pair at the beginning of the linked list in the corresponding bucket.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. If the slot is occupied, probe the next slot sequentially.
3. Insert the key-value pair in the first available slot.

#### Search

**Separate Chaining:**

1. Compute the hash index.
2. Traverse the linked list in the corresponding bucket to find the key.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. If the key at the index matches, return the value.
3. If not, probe the next slot sequentially until the key is found or an empty slot is encountered.

#### Deletion

**Separate Chaining:**

1. Compute the hash index.
2. Traverse the linked list in the corresponding bucket to find the key.
3. Remove the node containing the key from the linked list.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. Find the key using the search method.
3. Remove the key-value pair and handle subsequent probes to maintain the integrity of the probe sequence.

### C Code Example: Hash Table with Separate Chaining

We'll implement a simple hash table in C using **Separate Chaining** to handle collisions. This approach uses an array of linked lists, where each linked list represents a bucket.

#### Structure Definitions

1. **Hash Table Node:**

   Each node in the linked list represents a key-value pair.

   ```c
   typedef struct HashNode {
       int key;
       int value;
       struct HashNode* next;
   } HashNode;
   ```

2. **Hash Table:**

   The hash table contains an array of pointers to `HashNode` linked lists.

   ```c
   typedef struct HashTable {
       int size;           // Number of buckets
       HashNode** buckets; // Array of pointers to HashNode
   } HashTable;
   ```

#### Hash Function

A simple hash function using the modulo operator:

```C
int hashFunction(int key, int size) {
    return key % size;
}
```

#### Program Code

`practice.h`

```C
typedef struct HashNode{
    int key;
    int value;
    struct HashNode *next;
}HashNode;

typedef struct HashTable{
    int size; // Number of buckets
    HashNode **buckets; // Array of pointers to HashNode
} HashTable;

int hashFunction(int key, int size);

HashTable *createHashTable(int size);
void insert(HashTable *table, int key, int value);
int search(HashTable *table, int key);
void deleteKey(HashTable *table, int key);
void display(HashTable *table);

void freeHashTable(HashTable *table);
```

`functions.c`

```C

int hashFunction(int key, int size)
{
    return key % size;
}

HashTable *createHashTable(int size)
{
    HashTable *table = malloc(sizeof(HashTable));
    if (!table)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }

    table->size = size;
    table->buckets = malloc(size * sizeof(HashNode *));
    if (!table->buckets)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }
    for (int i = 0; i < size; i++)
    {
        table->buckets[i] = NULL;
    }
    return table;
}
void insert(HashTable *table, int key, int value)
{
    int index = hashFunction(key, table->size);
    HashNode *newNode = malloc(sizeof(HashNode));

    if (!newNode)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }
    newNode->key = key;
    newNode->value = value;
    newNode->next = table->buckets[index];
    table->buckets[index] = newNode;
    printf("Inserted key %d with value %d at index %d\n", key, value, index);
}
int search(HashTable *table, int key)
{
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    while (node != NULL)
    {
        if (node->key == key)
        {
            return node->value;
        }
        node = node->next;
    }
    // key not found
    return -1;
}
void deleteKey(HashTable *table, int key)
{
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    HashNode *prev = NULL;
    while (node != NULL && node->key != key)
    {

        // place the current node in the prev variable
        prev = node;

        // move to the next node, now the current node is the next node
        node = node->next;
    }

    if (node == NULL)
    {
        printf("Key %d not found.\n", key);
        return;
    }

    if (prev == NULL)
    {
        // deleting the first node in the bucket
        table->buckets[index] = node->next;
    }
    else
    {
        // e.g set the 1st node to point to the 3rd node
        prev->next = node->next;
    }

    free(node);
    printf("Key %d deleted from index %d.\n", key, index);
}
void display(HashTable *table)
{
    printf("\nHash Table Contents:\n");
    for (int i = 0; i < table->size; i++)
    {
        printf("Bucket %d: ", i);
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            printf("(%d, %d) -> ", node->key, node->value);
            node = node->next;
        }
        printf("NULL\n");
    }
}

void freeHashTable(HashTable *table)
{
    for (int i = 0; i < table->size; i++)
    {
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            // save the current node in tmp
            HashNode *tmp = node;

            // set the current node to point to the next node
            node = node->next;

            // free the tmp node effectively freeing the previous node
            free(tmp);
        }
    }
    free(table->buckets);
    free(table);
}
```

`practice.c`

```C
nt main()
{
    // create a hash table with 7 buckets
    HashTable *table = createHashTable(7);

    // Insert key-value pairs
    insert(table, 10, 100);
    insert(table, 20, 200);
    insert(table, 15, 150);
    insert(table, 7, 70);
    insert(table, 8, 80);
    insert(table, 22, 220);
    insert(table, 1, 10);

    display(table);

    // search for keys
    int key = 15;
    int value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d\n", key, value);
    }
    else
    {
        printf("\nKey %d not found\n", key);
    }

    // attempting to search for a non-existent key
    key = 99;
    value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d\n", key, value);
    }
    else
    {
        printf("\nKey %d not found\n", key);
    }

    // delete a key
    deleteKey(table, 20);

    // display the updated hash table
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(table, 99);

    // free the hash table
    freeHashTable(table);

    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted key 10 with value 100 at index 3
Inserted key 20 with value 200 at index 6
Inserted key 15 with value 150 at index 1
Inserted key 7 with value 70 at index 0
Inserted key 8 with value 80 at index 1
Inserted key 22 with value 220 at index 1
Inserted key 1 with value 10 at index 1

Hash Table Contents:
Bucket 0: (7, 70) -> NULL
Bucket 1: (1, 10) -> (22, 220) -> (8, 80) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (10, 100) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL

Key 15 found with value 150

Key 99 found with value 1
Key 20 deleted from index 6.

Hash Table Contents:
Bucket 0: (7, 70) -> NULL
Bucket 1: (1, 10) -> (22, 220) -> (8, 80) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (10, 100) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Key 99 not found.
```



### C Code Example # 2 (Open Addressing) (Simpler)

#### Program Code

`practice.h`

```C

#define MAX_NAME 256
#define TABLE_SIZE 10

// any pointer that see this value will be treated as deleted
//  so any value I am looking for can't be in that node
#define DELETED_NODE (person *)(0xFFFFFFFFFFFFFFFFUL)
// same as defining DELETED_NODE as (person *)-1 for portability

typedef struct
{
    char name[MAX_NAME];
    int age;
} person;

unsigned int hash(char *name);
void init_hash_table();
void print_table();
bool hash_table_insert(person *p);

person *hash_table_lookup(char *name);
person *hash_table_delete(char *name);
```

`functions.c`

```C
person *hash_table[TABLE_SIZE];

unsigned int hash(char *name){
    int length = strnlen(name, MAX_NAME);
    unsigned int hash_value = 0;
    for(int i = 0; i < length; i++){
        hash_value += name[i];
        hash_value = (hash_value * name[i]) % TABLE_SIZE;
    }
    return hash_value;
}

// Initialize hash table by setting all slots to NULL
void init_hash_table(){
    for(int i = 0; i < TABLE_SIZE; i++){
        hash_table[i] = NULL;
    }
}

void print_table(){
    printf("Start\n");
    
    for(int i = 0; i < TABLE_SIZE; i++){
        if(hash_table[i] == NULL){
            printf("\t%i\t---\n", i);
        }else if(hash_table[i] == DELETED_NODE){
            printf("\t%i\t---<deleted>\n", i);
        }else{
            printf("\t%i\t%s\n", i, hash_table[i]->name);
        }
    }
    
    printf("End\n");
}

bool hash_table_insert(person *p){
    if(p == NULL){
        return false;
    }
    
    // get a random hash value between 0 and TABLE_SIZE and set it to index
    int index = hash(p->name);
    
    // Linear probing
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        
        // Look for empty space to fill in the value to address collisions
        if(hash_table[try] == NULL || hash_table[try] == DELETED_NODE){
            hash_table[try] = p;
            return true;
        }
    }
    return false;
}

person *hash_table_lookup(char *name){
    int index = hash(name);
    
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        if(hash_table[try] == NULL){
            return NULL;
        }
        
        // continue the lookup if we find a deleted node
        if(hash_table[try] == DELETED_NODE){
            continue;
        }
        
        if(strncmp(hash_table[try]->name, name, MAX_NAME) == 0){
            // return the matched name
            return hash_table[try];
        }
    }
    return NULL;
}

person *hash_table_delete(char *name){
    int index = hash(name);
    
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        if(hash_table[try] == NULL){
            return NULL;
        }
        if(hash_table[try] == DELETED_NODE){
            continue;
        }
        if(strncmp(hash_table[try]->name, name, MAX_NAME) == 0){
            person *tmp = hash->table[try];
            hash->table[try] = DELETED_NODE;
            return tmp;
        }
    }
    return NULL;
}
```

`practice.c`

```C
int main()
{
    init_hash_table();
    print_table();
    person jacob = {.name = "Jacob", .age = 25};
    person kate = {.name = "Kate", .age = 28};
    person sarah = {.name = "Sarah", .age = 32};
    person penc = {.name = "PENC", .age = 24};
    person john = {.name = "John", .age = 27};
    person moon = {.name = "Moon", .age = 26};
    person johnson = {.name = "Johnson", .age = 19};
    person joseph = {.name = "Joseph", .age = 21};
    person steve = {.name = "Steve", .age = 22};
    person ember = {.name = "Ember", .age = 56};

    hash_table_insert(&jacob);
    hash_table_insert(&kate);
    hash_table_insert(&sarah);
    hash_table_insert(&penc);
    hash_table_insert(&john);
    hash_table_insert(&moon);
    hash_table_insert(&johnson);
    hash_table_insert(&joseph);
    hash_table_insert(&steve);
    hash_table_insert(&ember);
    print_table();

    person *tmp = hash_table_lookup("PENC");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    tmp = hash_table_lookup("Jacob");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    hash_table_delete("Kate");
    tmp = hash_table_lookup("Kate");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    print_table();
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Start
	0	---
	1	---
	2	---
	3	---
	4	---
	5	---
	6	---
	7	---
	8	---
	9	---
End
Start
	0	John
	1	Kate
	2	Jacob
	3	PENC
	4	Sarah
	5	Moon
	6	Johnson
	7	Joseph
	8	Steve
	9	Ember
End
Found PENC
Found Jacob
Not found
Start
	0	John
	1	---<deleted>
	2	Jacob
	3	PENC
	4	Sarah
	5	Moon
	6	Johnson
	7	Joseph
	8	Steve
	9	Ember
End

```



### C Code Example # 3 (Chaining) (Using the Example # 2)

#### Program Code

`practice.h`

```C
#define MAX_NAME 256
#define TABLE_SIZE 10

typedef struct person
{
    char name[MAX_NAME];
    int age;
    struct person *next;
} person;

unsigned int hash(char *name);
void init_hash_table();

bool hash_table_insert(person *p);

person *hash_table_lookup(char *name);
person *hash_table_delete(char *name);
void print_table();
```

`functions.c`

```C
person *hash_table[TABLE_SIZE];

// computes an index based on the person's name
unsigned int hash(char *name)
{
    int length = strnlen(name, MAX_NAME);
    unsigned int hash_value = 0;
    for (int i = 0; i < length; i++)
    {
        hash_value += name[i];
        hash_value = (hash_value * name[i]) % TABLE_SIZE;
    }
    return hash_value;
}

// Initialize hash table by setting all slots to NULL
void init_hash_table()
{
    for (int i = 0; i < TABLE_SIZE; i++)
    {
        hash_table[i] = NULL;
    }
}

bool hash_table_insert(person *p)
{
    if (p == NULL)
    {
        return false;
    }

    // get a random hash value between 0 and TABLE_SIZE and set it to index
    int index = hash(p->name);

    // Insert at the beginning of the linked list for separate chaining
    // example: jacob.next = hash_table[2] which is NULL
    // hash_table[2] = &jacob
    p->next = hash_table[index];
    hash_table[index] = p;
    return true;
}

person *hash_table_lookup(char *name)
{
    int index = hash(name);
    person *tmp = hash_table[index];
    
    // traverse the list until the match is found
    while (tmp != NULL && strncmp(tmp->name, name, MAX_NAME) != 0)
    {
        tmp = tmp->next;
    }
    
    // return the matched name
    return tmp;
}


person *hash_table_delete(char *name)
{
    unsigned int index = hash(name);

    person *tmp = hash_table[index];
    person *prev = NULL;
    while (tmp != NULL && strncmp(tmp->name, name, MAX_NAME) != 0)
    {
        prev = tmp;
        tmp = tmp->next;
    }

    if (tmp == NULL)
    {
        return NULL; // Not found
    }

    if (prev == NULL)
    {
        // Deleting the first node in the linked list
        hash_table[index] = tmp->next;
    }
    else
    {
        // Deleting a node beyond the first in the linked list
        prev->next = tmp->next;
    }
    return tmp; // Return the deleted node
}

void print_table()
{
    printf("Start\n");
    for (int i = 0; i < TABLE_SIZE; i++)
    {
        if (hash_table[i] == NULL)
        {
            printf("\t%i\t---\n", i);
        }
        else
        {
            printf("\t%i\t", i);
            person *tmp = hash_table[i];
            while (tmp != NULL)
            {
                printf("%s - ", tmp->name);
                tmp = tmp->next;
            }
            printf("\n");
        }
    }
    printf("End\n");
}
```

- Delete Operation:
  - **Deleting "Kate"**:
    - **Hash Index**: `hash("Kate") = 2`.
    - **Search**: Traverse `hash_table[2]` to find "Kate".
    - **Deletion:**
      - Update `prev->next` to skip "Kate".
      - If "Kate" is the first node, update `hash_table[2]` to `kate.next`.
  - **Lookup "Kate" Again**:
    - **Result**: "Kate" not found.

`practice.c`

```C
int main()
{
    init_hash_table();
    print_table();
    person jacob = {.name = "Jacob", .age = 25};
    person kate = {.name = "Kate", .age = 28};
    person sarah = {.name = "Sarah", .age = 32};
    person penc = {.name = "PENC", .age = 24};
    person john = {.name = "John", .age = 27};
    person moon = {.name = "Moon", .age = 26};
    person johnson = {.name = "Johnson", .age = 19};
    person joseph = {.name = "Joseph", .age = 21};
    person steve = {.name = "Steve", .age = 22};
    person ember = {.name = "Ember", .age = 56};

    hash_table_insert(&jacob);
    hash_table_insert(&kate);
    hash_table_insert(&sarah);
    hash_table_insert(&penc);
    hash_table_insert(&john);
    hash_table_insert(&moon);
    hash_table_insert(&johnson);
    hash_table_insert(&joseph);
    hash_table_insert(&steve);
    hash_table_insert(&ember);
    print_table();

    person *tmp = hash_table_lookup("PENC");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    tmp = hash_table_lookup("Jacob");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    hash_table_delete("Kate");
    tmp = hash_table_lookup("Kate");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    print_table();
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Start
	0	---
	1	---
	2	---
	3	---
	4	---
	5	---
	6	---
	7	---
	8	---
	9	---
End
Start
	0	Joseph - Johnson - Moon - John - 
	1	Kate - 
	2	Jacob - 
	3	Steve - PENC - 
	4	Sarah - 
	5	---
	6	Ember - 
	7	---
	8	---
	9	---
End
Found PENC
Found Jacob
Not found
Start
	0	Joseph - Johnson - Moon - John - 
	1	---
	2	Jacob - 
	3	Steve - PENC - 
	4	Sarah - 
	5	---
	6	Ember - 
	7	---
	8	---
	9	---
End

```



### Understanding Dynamic Resizing and Rehashing

#### **Dynamic Resizing**

**Dynamic Resizing** refers to the ability of a hash table to adjust its number of buckets (i.e., its size) based on the number of elements it contains. When the number of elements grows, the hash table can **resize** to accommodate more elements, reducing the load factor and minimizing collisions. Conversely, if the number of elements decreases significantly, the table can shrink to save memory.

**Key Points:**

- **Load Factor (α):** Defined as `α = n/m`, where `n` is the number of elements and `m` is the number of buckets. A higher load factor increases the chance of collisions.
- **Threshold:** A predefined load factor threshold (commonly around `0.7`) triggers resizing. When `α > 0.7`, the table resizes to maintain performance.

#### **Rehashing**

**Rehashing** is the process of recalculating the hash indices for all existing elements and moving them into a new set of buckets after the table has been resized. This ensures that the elements are evenly distributed according to the new table size.

**Key Points:**

- **Necessary After Resizing:** Since the number of buckets changes, the hash indices of keys typically change, necessitating rehashing.
- **Efficiency:** Rehashing ensures that the hash table maintains its performance characteristics by reducing collisions.

### Implementing a Hash Table with Dynamic Resizing and Rehashing in C

#### Program Code

`practice.h`

```C
#define LOAD_FACTOR_THRESHOLD 0.7
typedef struct HashNode
{
    int key;
    int value;
    struct HashNode *next;
} HashNode;

typedef struct HashTable
{
    int size; // Number of buckets
    int count;
    HashNode **buckets; // Array of pointers to HashNode
} HashTable;

int hashFunction(int key, int size);

HashTable *createHashTable(int size);
// Insert key-value pair into hash table
void insert(HashTable **table_ref, int key, int value);

// Search for a key in the hash table
int search(HashTable *table, int key);

// Delete a key from the hash table
void deleteKey(HashTable **table_ref, int key);

// Resize and rehash the hash table
void resizeAndRehash(HashTable **table_ref, int new_size);
void display(HashTable *table);

void freeHashTable(HashTable *table);
```

`functions.c`

```C
int hashFunction(int key, int size)
{
    return key % size;
}

// Create a new hash table
HashTable *createHashTable(int size)
{
    HashTable *table = (HashTable *)malloc(sizeof(HashTable));
    if (!table)
    {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    table->size = size;
    table->count = 0;
    table->buckets = (HashNode **)malloc(size * sizeof(HashNode *));
    if (!table->buckets)
    {
        printf("Memory allocation failed for buckets.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++)
    {
        table->buckets[i] = NULL;
    }
    return table;
}

// Insert function with dynamic resizing
void insert(HashTable **table_ref, int key, int value)
{
    HashTable *table = *table_ref;
    int index = hashFunction(key, table->size);

    // Create a new node
    HashNode *newNode = (HashNode *)malloc(sizeof(HashNode));
    if (!newNode)
    {
        printf("Memory allocation failed for HashNode.\n");
        exit(EXIT_FAILURE);
    }
    newNode->key = key;
    newNode->value = value;
    newNode->next = table->buckets[index];
    table->buckets[index] = newNode;
    table->count++;

    printf("Inserted key %d with value %d at index %d.\n", key, value, index);

    // Calculate load factor
    double load_factor = (double)table->count / table->size;
    if (load_factor > LOAD_FACTOR_THRESHOLD)
    {
        // Resize to double the current size
        resizeAndRehash(table_ref, table->size * 2);
    }
}

// Search function
int search(HashTable *table, int key)
{
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    while (node != NULL)
    {
        if (node->key == key)
        {
            return node->value;
        }
        node = node->next;
    }
    // Key not found
    return -1;
}

// Delete function with optional resizing down
void deleteKey(HashTable **table_ref, int key)
{
    HashTable *table = *table_ref;
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    HashNode *prev = NULL;

    while (node != NULL && node->key != key)
    {
        prev = node;
        node = node->next;
    }

    if (node == NULL)
    {
        printf("Key %d not found. Cannot delete.\n", key);
        return;
    }

    if (prev == NULL)
    {
        // Deleting the first node in the bucket
        table->buckets[index] = node->next;
    }
    else
    {
        prev->next = node->next;
    }

    free(node);
    table->count--;
    printf("Key %d deleted from index %d.\n", key, index);

    // Optional: Resize down if load factor is too low
    double load_factor = (double)table->count / table->size;
    if (load_factor < (LOAD_FACTOR_THRESHOLD / 2) && table->size > 7)
    { // Minimum size of 7
        resizeAndRehash(table_ref, table->size / 2);
    }
}

// Resize and rehash function
void resizeAndRehash(HashTable **table_ref, int new_size)
{
    HashTable *old_table = *table_ref;
    HashTable *new_table = createHashTable(new_size);

    printf("Resizing from %d to %d buckets and rehashing...\n", old_table->size, new_size);

    for (int i = 0; i < old_table->size; i++)
    {
        HashNode *node = old_table->buckets[i];
        while (node != NULL)
        {
            insert(&new_table, node->key, node->value);
            node = node->next;
        }
    }
    
    // Free all original nodes in the old table
    for (int i = 0; i < old_table->size; i++)
    {
        HashNode *node = old_table->buckets[i];
        while (node != NULL)
        {
            HashNode *temp = node;
            node = node->next;
            free(temp);
        }
    }

    // Free the old table's buckets array
    free(old_table->buckets);
    // Free the old table structure
    free(old_table);
    // Update the table reference to point to the new table
    *table_ref = new_table;
    printf("Resizing and rehashing completed.\n");
}

// Display function
void display(HashTable *table)
{
    printf("\nHash Table Contents (Size: %d, Count: %d):\n", table->size, table->count);
    for (int i = 0; i < table->size; i++)
    {
        printf("Bucket %d: ", i);
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            printf("(%d, %d) -> ", node->key, node->value);
            node = node->next;
        }
        printf("NULL\n");
    }
}

void freeHashTable(HashTable *table)
{
    for (int i = 0; i < table->size; i++)
    {
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            HashNode *temp = node;
            node = node->next;
            free(temp);
        }
    }
    free(table->buckets);
    free(table);
    printf("Hash table memory freed.\n");
}
```

`practice.c`

```C
int main()
{
    // Create a hash table with an initial size of 7 buckets
    HashTable *table = createHashTable(7);

    // Insert key-value pairs
    insert(&table, 10, 100);
    insert(&table, 20, 200);
    insert(&table, 15, 150);
    insert(&table, 7, 70);
    insert(&table, 8, 80);
    insert(&table, 22, 220);
    insert(&table, 1, 10);

    // Display the hash table
    display(table);

    // Insert more elements to trigger resizing
    insert(&table, 17, 170);
    insert(&table, 27, 270);

    // Display the hash table after resizing
    display(table);

    // Search for existing and non-existing keys
    int key = 15;
    int value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d.\n", key, value);
    }
    else
    {
        printf("\nKey %d not found.\n", key);
    }

    key = 99;
    value = search(table, key);
    if (value != -1)
    {
        printf("Key %d found with value %d.\n", key, value);
    }
    else
    {
        printf("Key %d not found.\n", key);
    }

    // Delete some keys
    deleteKey(&table, 20);
    deleteKey(&table, 7);

    // Display the hash table after deletions
    display(table);

    // Delete more keys to potentially trigger resizing down
    deleteKey(&table, 10);
    deleteKey(&table, 15);
    deleteKey(&table, 8);

    // Display the hash table after further deletions
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(&table, 100);

    // Free the hash table
    freeHashTable(table);

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted key 10 with value 100 at index 3.
Inserted key 20 with value 200 at index 6.
Inserted key 15 with value 150 at index 1.
Inserted key 7 with value 70 at index 0.
Inserted key 8 with value 80 at index 1.
Resizing from 7 to 14 buckets and rehashing...
Inserted key 7 with value 70 at index 7.
Inserted key 8 with value 80 at index 8.
Inserted key 15 with value 150 at index 1.
Inserted key 10 with value 100 at index 10.
Inserted key 20 with value 200 at index 6.
Resizing and rehashing completed.
Inserted key 22 with value 220 at index 8.
Inserted key 1 with value 10 at index 1.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: NULL
Inserted key 17 with value 170 at index 3.
Inserted key 27 with value 270 at index 13.

Hash Table Contents (Size: 14, Count: 9):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL

Key 15 found with value 150.
Key 99 not found.
Key 20 deleted from index 6.
Key 7 deleted from index 7.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Bucket 7: NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL
Key 10 deleted from index 10.
Key 15 deleted from index 1.
Key 8 deleted from index 8.
Resizing from 14 to 7 buckets and rehashing...
Inserted key 1 with value 10 at index 1.
Inserted key 17 with value 170 at index 3.
Inserted key 22 with value 220 at index 1.
Inserted key 27 with value 270 at index 6.
Resizing and rehashing completed.

Hash Table Contents (Size: 7, Count: 4):
Bucket 0: NULL
Bucket 1: (22, 220) -> (1, 10) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (27, 270) -> NULL
Key 100 not found. Cannot delete.
Hash table memory freed.

```

#### Checking Memory Leak

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==8613== Memcheck, a memory error detector
==8613== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==8613== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==8613== Command: ./practice
==8613== 
Inserted key 10 with value 100 at index 3.
Inserted key 20 with value 200 at index 6.
Inserted key 15 with value 150 at index 1.
Inserted key 7 with value 70 at index 0.
Inserted key 8 with value 80 at index 1.
Resizing from 7 to 14 buckets and rehashing...
Inserted key 7 with value 70 at index 7.
Inserted key 8 with value 80 at index 8.
Inserted key 15 with value 150 at index 1.
Inserted key 10 with value 100 at index 10.
Inserted key 20 with value 200 at index 6.
Resizing and rehashing completed.
Inserted key 22 with value 220 at index 8.
Inserted key 1 with value 10 at index 1.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: NULL
Inserted key 17 with value 170 at index 3.
Inserted key 27 with value 270 at index 13.

Hash Table Contents (Size: 14, Count: 9):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL

Key 15 found with value 150.
Key 99 not found.
Key 20 deleted from index 6.
Key 7 deleted from index 7.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Bucket 7: NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL
Key 10 deleted from index 10.
Key 15 deleted from index 1.
Key 8 deleted from index 8.
Resizing from 14 to 7 buckets and rehashing...
Inserted key 1 with value 10 at index 1.
Inserted key 17 with value 170 at index 3.
Inserted key 22 with value 220 at index 1.
Inserted key 27 with value 270 at index 6.
Resizing and rehashing completed.

Hash Table Contents (Size: 7, Count: 4):
Bucket 0: NULL
Bucket 1: (22, 220) -> (1, 10) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (27, 270) -> NULL
Key 100 not found. Cannot delete.
Hash table memory freed.
==8613== 
==8613== HEAP SUMMARY:
==8613==     in use at exit: 0 bytes in 0 blocks
==8613==   total heap usage: 25 allocs, 25 frees, 1,584 bytes allocated
==8613== 
==8613== All heap blocks were freed -- no leaks are possible
==8613== 
==8613== For lists of detected and suppressed errors, rerun with: -s
==8613== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



### Use Cases of Hash Tables

Hash tables are versatile and widely used in various applications due to their efficient average-case performance for insertion, deletion, and search operations. Here are some common real-world use cases:

1. **Databases:**
   - **Indexing:** Hash indexes allow quick retrieval of records based on key values.
   - **Caching:** Frequently accessed data is stored in a hash table to speed up queries.
2. **Programming Languages:**
   - **Symbol Tables:** Compilers use hash tables to keep track of identifiers (variables, functions).
   - **Dictionaries:** Languages like Python and Java use hash tables for their dictionary data types.
3. **Networking:**
   - **Routing Tables:** Routers use hash tables to store and quickly access routing information.
   - **DNS Caching:** Caches domain name system (DNS) lookups using hash tables for faster resolution.
4. **Operating Systems:**
   - **Process Management:** Quick access to process information using process IDs as keys.
   - **Memory Management:** Hash tables can manage memory pages and their statuses.
5. **Web Development:**
   - **Session Management:** Storing user sessions with session IDs as keys.
   - **URL Routing:** Mapping URLs to handler functions efficiently.
6. **Game Development:**
   - **Collision Detection:** Efficiently managing and querying game objects.
   - **Resource Management:** Storing and accessing game assets like textures and sounds.
7. **Security:**
   - **Hash Tables for Passwords:** Storing hashed passwords for quick verification.
   - **Data Integrity:** Using hash functions to verify the integrity of data.
8. **Bioinformatics:**
   - **Genomic Data Storage:** Efficiently storing and querying large genomic datasets.



**Quadratic Probing** and **Double Hashing** are two effective **collision resolution techniques** used in **open addressing** hash tables. These methods aim to handle scenarios where multiple keys hash to the same index, ensuring efficient storage and retrieval of data without relying on additional data structures like linked lists (as in separate chaining).

### Quadratic Probing

**Quadratic Probing** is a collision resolution technique where the interval between probes increases quadratically. Instead of checking the next slot linearly, it uses a quadratic function to determine the sequence of indices to probe.

#### Advantages and Disadvantages

**Advantages:**

1. **Reduces Primary Clustering:** Unlike linear probing, quadratic probing reduces the clustering effect, where contiguous blocks of occupied slots form.
2. **Simpler Implementation:** Easier to implement compared to double hashing.

**Disadvantages:**

1. **Secondary Clustering:** While it reduces primary clustering, quadratic probing can still suffer from secondary clustering where keys with the same initial hash follow the same probe sequence.
2. **Not Guaranteed to Probe All Slots:** Depending on the table size and probe function, some slots might never be probed, potentially leading to failure in finding an empty slot even if the table isn't full.
3. **Requires Prime Table Size:** To ensure that all slots are probed, it's often recommended to use a prime number for the table size.

Index=(h(k)+i<sup>2</sup>) % m.

Suppose h(k) = k mod 7, m = 7, and we have keys 10. 17, 24.

- Insert 10:
  - 10 % 7 = 3;
  - Place at index 3.
- Insert 17:
  - 17 % 7 = 3 -> Collision;
  - Probe 1: (3 + 1<sup>2</sup>) % 7 = 4;
  - Place at index 4.
- Insert 24:
  - 24 % 7 = 3 -> Collision.
  - Probe 1: (3 + 1<sup>2</sup>) % 7 = 4 -> Occupied.
  - Probe 2: (3 + 2<sup>2</sup>) % 7 = 0.
  - Place at index 0.

### C Code Example (Quadratic Probing)

#### Program Code

`practice.h`

```C
#define INITIAL_SIZE 7
#define LOAD_FACTOR_THRESHOLD 0.5

typedef struct HashEntry
{
    int key;
    int value;
    bool isOccupied;
    bool isDeleted;
} HashEntry;

typedef struct HashTable
{
    int size;
    int count;
    HashEntry *table;
} HashTable;

int hashFunction(int key, int size);
HashTable *createHashTable(int size);
bool needsResize(HashTable *ht);

void resizeAndRehash(HashTable **ht, int new_size);

void insert(HashTable **ht, int key, int value);

int search(HashTable *ht, int key);
void deleteKey(HashTable **ht, int key);

void display(HashTable *ht);

void freeHashTable(HashTable *ht);
```

`functions.c`

```C
// Hash Function
int hashFunction(int key, int size)
{
    return key % size;
}

// Create a new hash table
HashTable *createHashTable(int size)
{
    HashTable *ht = (HashTable *)malloc(sizeof(HashTable));
    if (!ht)
    {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    ht->size = size;
    ht->count = 0;
    ht->table = (HashEntry *)malloc(sizeof(HashEntry) * size);
    if (!ht->table)
    {
        printf("Memory allocation failed for HashTable entries.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++)
    {
        ht->table[i].isOccupied = false;
        ht->table[i].isDeleted = false;
    }
    return ht;
}

// Check if the hash table needs resizing
bool needsResize(HashTable *ht)
{
    double load_factor = (double)ht->count / ht->size;
    return load_factor > LOAD_FACTOR_THRESHOLD;
}

// Resize and rehash the hash table
void resizeAndRehash(HashTable **ht_ref, int new_size)
{
    HashTable *old_ht = *ht_ref;
    HashTable *new_ht = createHashTable(new_size);

    for (int i = 0; i < old_ht->size; i++)
    {
        if (old_ht->table[i].isOccupied && !old_ht->table[i].isDeleted)
        {
            // Insert into new hash table
            // Using the same insert function to handle quadratic probing
            int key = old_ht->table[i].key;
            int value = old_ht->table[i].value;

            int index = hashFunction(key, new_ht->size);
            int j = 1;
            while (new_ht->table[index].isOccupied)
            {
                index = (hashFunction(key, new_ht->size) + j * j) % new_ht->size;
                j++;
            }
            new_ht->table[index].key = key;
            new_ht->table[index].value = value;
            new_ht->table[index].isOccupied = true;
            new_ht->table[index].isDeleted = false;
            new_ht->count++;
        }
    }

    // Free the old table
    free(old_ht->table);
    free(old_ht);

    // Update the reference
    *ht_ref = new_ht;
    printf("Resized and rehashed to new size %d.\n", new_size);
}

// Insert key-value pair using Quadratic Probing
void insert(HashTable **ht_ref, int key, int value)
{
    HashTable *ht = *ht_ref;

    if (needsResize(ht))
    {
        resizeAndRehash(ht_ref, ht->size * 2); // Double the size
        ht = *ht_ref;                          // Update the local reference after resizing
    }

    int index = hashFunction(key, ht->size);
    int original_index = index;
    int i = 1;

    while (ht->table[index].isOccupied && !ht->table[index].isDeleted && ht->table[index].key != key)
    {
        // handle collision, finding next available slot for example,
       // 24 % 7 = 3 -> Collision.
       // Probe 1: (3 + 1^^2) % 7 = 4 -> Occupied.
       // increment index to find the next available slot
      //Probe 2: (3 + 2^^2) % 7 = 0.
      //Place at index 0.
        index = (original_index + i * i) % ht->size;
        i++;
        if (i == ht->size)
        {
            printf("Hash table is full. Cannot insert key %d.\n", key);
            return;
        }
    }

    if (!ht->table[index].isOccupied || ht->table[index].isDeleted)
    {
        ht->table[index].key = key;
        ht->table[index].value = value;
        ht->table[index].isOccupied = true;
        ht->table[index].isDeleted = false;
        ht->count++;
        printf("Inserted key %d with value %d at index %d.\n", key, value, index);
    }
    else
    {
        // Key already exists, update the value
        ht->table[index].value = value;
        printf("Updated key %d with new value %d at index %d.\n", key, value, index);
    }
}

// Search for a key using Quadratic Probing
int search(HashTable *ht, int key)
{
    int index = hashFunction(key, ht->size);
    int original_index = index;
    int i = 1;

    while (ht->table[index].isOccupied)
    {
        if (!ht->table[index].isDeleted && ht->table[index].key == key)
        {
            return ht->table[index].value;
        }
        index = (original_index + i * i) % ht->size;
        i++;
        if (i == ht->size)
        {
            break; // Searched all slots
        }
    }
    return -1; // Not found
}

// Delete a key using Quadratic Probing
void deleteKey(HashTable **ht_ref, int key)
{
    HashTable *ht = *ht_ref;
    int index = hashFunction(key, ht->size);
    int original_index = index;
    int i = 1;

    while (ht->table[index].isOccupied)
    {
        if (!ht->table[index].isDeleted && ht->table[index].key == key)
        {
            ht->table[index].isDeleted = true;
            ht->count--;
            printf("Key %d deleted from index %d.\n", key, index);

            // Optional: Resize down if load factor is too low
            double load_factor = (double)ht->count / ht->size;
            if (load_factor < (LOAD_FACTOR_THRESHOLD / 2) && ht->size > INITIAL_SIZE)
            {
                resizeAndRehash(ht_ref, ht->size / 2); // Halve the size
            }
            return;
        }
        index = (original_index + i * i) % ht->size;
        i++;
        if (i == ht->size)
        {
            break; // Searched all slots
        }
    }
    printf("Key %d not found. Cannot delete.\n", key);
}

// Display the hash table
void display(HashTable *ht)
{
    printf("\nHash Table Contents (Size: %d, Count: %d):\n", ht->size, ht->count);
    for (int i = 0; i < ht->size; i++)
    {
        printf("Slot %d: ", i);
        if (ht->table[i].isOccupied && !ht->table[i].isDeleted)
        {
            printf("(%d, %d)", ht->table[i].key, ht->table[i].value);
        }
        else if (ht->table[i].isDeleted)
        {
            printf("DELETED");
        }
        else
        {
            printf("NULL");
        }
        printf("\n");
    }
}

// Free the hash table
void freeHashTable(HashTable *ht)
{
    free(ht->table);
    free(ht);
    printf("Hash table memory freed.\n");
}
```

`practice.c`

```C
int main()
{
    // Create a hash table with initial size
    HashTable *table = createHashTable(INITIAL_SIZE);

    // Insert key-value pairs
    insert(&table, 10, 100);
    insert(&table, 20, 200);
    insert(&table, 15, 150);
    insert(&table, 7, 70);
    insert(&table, 8, 80);
    insert(&table, 22, 220);
    insert(&table, 1, 10);

    // Display the hash table
    display(table);

    // Insert more elements to trigger resizing
    insert(&table, 17, 170);
    insert(&table, 27, 270);

    // Display the hash table after resizing
    display(table);

    // Search for keys
    int key = 15;
    int value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d.\n", key, value);
    }
    else
    {
        printf("\nKey %d not found.\n", key);
    }

    key = 99;
    value = search(table, key);
    if (value != -1)
    {
        printf("Key %d found with value %d.\n", key, value);
    }
    else
    {
        printf("Key %d not found.\n", key);
    }

    // Delete some keys
    deleteKey(&table, 20);
    deleteKey(&table, 7);

    // Display the hash table after deletions
    display(table);

    // Delete more keys to potentially trigger resizing down
    deleteKey(&table, 10);
    deleteKey(&table, 15);
    deleteKey(&table, 8);

    // Display the hash table after further deletions
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(&table, 100);

    // Free the hash table
    freeHashTable(table);
    return 0;
}

```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==14212== Memcheck, a memory error detector
==14212== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==14212== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==14212== Command: ./practice
==14212== 
Inserted key 10 with value 100 at index 3.
Inserted key 20 with value 200 at index 6.
Inserted key 15 with value 150 at index 1.
Inserted key 7 with value 70 at index 0.
Resized and rehashed to new size 14.
Inserted key 8 with value 80 at index 8.
Inserted key 22 with value 220 at index 9.
Inserted key 1 with value 10 at index 2.

Hash Table Contents (Size: 14, Count: 7):
Slot 0: NULL
Slot 1: (15, 150)
Slot 2: (1, 10)
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: (20, 200)
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: (22, 220)
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Inserted key 17 with value 170 at index 3.
Resized and rehashed to new size 28.
Inserted key 27 with value 270 at index 27.

Hash Table Contents (Size: 28, Count: 9):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: (20, 200)
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)

Key 15 found with value 150.
Key 99 not found.
Key 20 deleted from index 20.
Key 7 deleted from index 7.

Hash Table Contents (Size: 28, Count: 7):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: DELETED
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: DELETED
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)
Key 10 deleted from index 10.
Resized and rehashed to new size 14.
Key 15 deleted from index 2.
Key 8 deleted from index 8.

Hash Table Contents (Size: 14, Count: 4):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: DELETED
Slot 3: (17, 170)
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: NULL
Slot 8: DELETED
Slot 9: (22, 220)
Slot 10: NULL
Slot 11: NULL
Slot 12: NULL
Slot 13: (27, 270)
Key 100 not found. Cannot delete.
Hash table memory freed.
==14212== 
==14212== HEAP SUMMARY:
==14212==     in use at exit: 0 bytes in 0 blocks
==14212==   total heap usage: 9 allocs, 9 frees, 1,844 bytes allocated
==14212== 
==14212== All heap blocks were freed -- no leaks are possible
==14212== 
==14212== For lists of detected and suppressed errors, rerun with: -s
==14212== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



### Double Hashing

**Double Hashing** is an advanced collision resolution technique in open addressing that uses two distinct hash functions to determine the probe sequence. This method significantly reduces clustering and improves the uniformity of key distribution compared to linear and quadratic probing.

**Key Characteristics:**

- **Two Hash Functions:** Ensures that the probe sequence for each key is unique and less likely to overlap with others.
- **Reduced Clustering:** Minimizes primary and secondary clustering, enhancing performance.
- **Requires Secondary Hash Function to be Non-Zero and Relatively Prime to Table Size:** To ensure that all slots can be probed.

**Advantages:**

1. **Minimizes Clustering:** Double hashing significantly reduces both primary and secondary clustering, leading to more uniform key distribution.
2. **Better Performance:** Maintains efficient probe sequences, especially under high load factors.
3. **Flexible Probing:** Allows customization of the secondary hash function to suit specific needs.

**Disadvantages:**

1. **Complexity:** More complex to implement compared to linear and quadratic probing.
2. **Requires Two Good Hash Functions:** Both hash functions must be carefully designed to ensure they are independent and distribute keys uniformly.
3. **Cannot Probe All Slots If h2(k)h_2(k)h2(k) Shares Common Factors with mmm:** To ensure all slots can be probed, h2(k)h_2(k)h2(k) and mmm should be co-prime.

**Probe Sequence Formula:**

Index = (h<sub>1</sub>(k) + i * h<sub>2</sub>(k)) % m

Where:

- h<sub>1</sub>(k) is the primary hash function.
- h<sub>2</sub>(k) is the secondary hash function.
- m is the size of the hash table.
- i is the probe number (starting from 0).

**Example**:

Suppose h<sub>1</sub>(k) = k % 7, h<sub>2</sub>(k) = 5 - (k % 5), and m = 7.

- Insert 10:
  - h<sub>1</sub>(10) = 3.
  - Place at index 3.
- Insert 17:
  - h<sub>1</sub>(17) = 3 -> Collision
  - h<sub>2</sub>(17) = 5 - (17 % 5) = 5 - 2 = 3.
  - Probe 1: (3 + 1 * 3) % 7 = 6
  - Place at index 6.
- Insert 24:
  - h<sub>1</sub>(24) = 3 -> Collision.
  - h<sub>2</sub>(24) = 5 - (24 % 5) = 5 - 4 = 1
  - Probe 1: (3 + 1 * 1) % 7 = 4.
  - Place at index 4.

### C Code Example (Double Hashing)

#### Program Code

`practice.h`

```C
#define INITIAL_SIZE 7
#define LOAD_FACTOR_THRESHOLD 0.5

typedef struct HashEntry
{
    int key;
    int value;
    bool isOccupied;
    bool isDeleted;
} HashEntry;

typedef struct HashTable
{
    int size;
    int count;
    HashEntry *table;
} HashTable;

int hashFunction1(int key, int size);
int hashFunction2(int key, int size);
HashTable *createHashTable(int size);
bool needsResize(HashTable *ht);
void resizeAndRehash(HashTable **ht_ref, int new_size);
void insert(HashTable **ht_ref, int key, int value);
int search(HashTable *ht, int key);
void deleteKey(HashTable **ht_ref, int key);
void display(HashTable *ht);
void freeHashTable(HashTable *ht);
```

`functions.c`

```C
// Primary Hash Function
int hashFunction1(int key, int size) {
    return key % size;
}

// Secondary Hash Function
int hashFunction2(int key, int size) {
    return 5 - (key % 5); // Must be non-zero and relatively prime to size
}

// Create a new hash table
HashTable* createHashTable(int size) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (!ht) {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    ht->size = size;
    ht->count = 0;
    ht->table = (HashEntry*) malloc(sizeof(HashEntry) * size);
    if (!ht->table) {
        printf("Memory allocation failed for HashTable entries.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++) {
        ht->table[i].isOccupied = false;
        ht->table[i].isDeleted = false;
    }
    return ht;
}

// Check if the hash table needs resizing
bool needsResize(HashTable* ht) {
    double load_factor = (double) ht->count / ht->size;
    return load_factor > LOAD_FACTOR_THRESHOLD;
}

// Resize and rehash the hash table
void resizeAndRehash(HashTable** ht_ref, int new_size) {
    HashTable* old_ht = *ht_ref;
    HashTable* new_ht = createHashTable(new_size);

    for (int i = 0; i < old_ht->size; i++) {
        if (old_ht->table[i].isOccupied && !old_ht->table[i].isDeleted) {
            // Insert into new hash table using double hashing
            int key = old_ht->table[i].key;
            int value = old_ht->table[i].value;

            // Calculate new indices using new size
            int h1 = hashFunction1(key, new_ht->size);
            int h2 = hashFunction2(key, new_ht->size);
            int index = h1;
            int j = 1;

            while (new_ht->table[index].isOccupied) {
                index = (h1 + j * h2) % new_ht->size;
                j++;
                if (j == new_ht->size) {
                    printf("Failed to rehash key %d during resizing.\n", key);
                    break;
                }
            }

            if (!new_ht->table[index].isOccupied) {
                new_ht->table[index].key = key;
                new_ht->table[index].value = value;
                new_ht->table[index].isOccupied = true;
                new_ht->table[index].isDeleted = false;
                new_ht->count++;
            }
        }
    }

    // Free the old table
    free(old_ht->table);
    free(old_ht);

    // Update the reference
    *ht_ref = new_ht;
    printf("Resized and rehashed to new size %d.\n", new_size);
}

// Insert key-value pair using Double Hashing
void insert(HashTable** ht_ref, int key, int value) {
    HashTable* ht = *ht_ref;

    if (needsResize(ht)) {
        resizeAndRehash(ht_ref, ht->size * 2); // Double the size
        ht = *ht_ref; // Update the local reference after resizing
    }

    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied && !ht->table[index].isDeleted && ht->table[index].key != key) {
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            printf("Hash table is full. Cannot insert key %d.\n", key);
            return;
        }// Primary Hash Function
int hashFunction1(int key, int size) {
    return key % size;
}

// Secondary Hash Function
int hashFunction2(int key, int size) {
    return 5 - (key % 5); // Must be non-zero and relatively prime to size
}

// Create a new hash table
HashTable* createHashTable(int size) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (!ht) {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    ht->size = size;
    ht->count = 0;
    ht->table = (HashEntry*) malloc(sizeof(HashEntry) * size);
    if (!ht->table) {
        printf("Memory allocation failed for HashTable entries.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++) {
        ht->table[i].isOccupied = false;
        ht->table[i].isDeleted = false;
    }
    return ht;
}

// Check if the hash table needs resizing
bool needsResize(HashTable* ht) {
    double load_factor = (double) ht->count / ht->size;
    return load_factor > LOAD_FACTOR_THRESHOLD;
}

// Resize and rehash the hash table
void resizeAndRehash(HashTable** ht_ref, int new_size) {
    HashTable* old_ht = *ht_ref;
    HashTable* new_ht = createHashTable(new_size);

    for (int i = 0; i < old_ht->size; i++) {
        if (old_ht->table[i].isOccupied && !old_ht->table[i].isDeleted) {
            // Insert into new hash table using double hashing
            int key = old_ht->table[i].key;
            int value = old_ht->table[i].value;

            // Calculate new indices using new size
            int h1 = hashFunction1(key, new_ht->size);
            int h2 = hashFunction2(key, new_ht->size);
            int index = h1;
            int j = 1; // the probe

            while (new_ht->table[index].isOccupied) {
                index = (h1 + j * h2) % new_ht->size;
                j++;
                if (j == new_ht->size) {
                    printf("Failed to rehash key %d during resizing.\n", key);
                    break;
                }
            }

            if (!new_ht->table[index].isOccupied) {
                new_ht->table[index].key = key;
                new_ht->table[index].value = value;
                new_ht->table[index].isOccupied = true;
                new_ht->table[index].isDeleted = false;
                new_ht->count++;
            }
        }
    }

    // Free the old table
    free(old_ht->table);
    free(old_ht);

    // Update the reference
    *ht_ref = new_ht;
    printf("Resized and rehashed to new size %d.\n", new_size);
}

// Insert key-value pair using Double Hashing
void insert(HashTable** ht_ref, int key, int value) {
    HashTable* ht = *ht_ref;

    if (needsResize(ht)) {
        resizeAndRehash(ht_ref, ht->size * 2); // Double the size
        ht = *ht_ref; // Update the local reference after resizing
    }

    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1; // the probe

    while (ht->table[index].isOccupied && !ht->table[index].isDeleted && ht->table[index].key != key) {
        index = (h1 + j
    }

    if (!ht->table[index].isOccupied || ht->table[index].isDeleted) {
        ht->table[index].key = key;
        ht->table[index].value = value;
        ht->table[index].isOccupied = true;
        ht->table[index].isDeleted = false;
        ht->count++;
        printf("Inserted key %d with value %d at index %d.\n", key, value, index);
    } else {
        // Key already exists, update the value
        ht->table[index].value = value;
        printf("Updated key %d with new value %d at index %d.\n", key, value, index);
    }
}

// Search for a key using Double Hashing
int search(HashTable* ht, int key) {
    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied) {
        if (!ht->table[index].isDeleted && ht->table[index].key == key) {
            return ht->table[index].value;
        }
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            break; // Searched all slots
        }
    }
    return -1; // Not found
}

// Delete a key using Double Hashing
void deleteKey(HashTable** ht_ref, int key) {
    HashTable* ht = *ht_ref;
    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied) {
        if (!ht->table[index].isDeleted && ht->table[index].key == key) {
            ht->table[index].isDeleted = true;
            ht->count--;
            printf("Key %d deleted from index %d.\n", key, index);

            // Resize down if load factor is too low
            double load_factor = (double) ht->count / ht->size;
            if (load_factor < (LOAD_FACTOR_THRESHOLD / 2) && ht->size > INITIAL_SIZE) {
                resizeAndRehash(ht_ref, ht->size / 2); // Halve the size
            }
            return;
        }
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            break; // Searched all slots
        }
    }
    printf("Key %d not found. Cannot delete.\n", key);
}

// Display the hash table
void display(HashTable* ht) {
    printf("\nHash Table Contents (Size: %d, Count: %d):\n", ht->size, ht->count);
    for (int i = 0; i < ht->size; i++) {
        printf("Slot %d: ", i);
        if (ht->table[i].isOccupied && !ht->table[i].isDeleted) {
            printf("(%d, %d)", ht->table[i].key, ht->table[i].value);
        } else if (ht->table[i].isDeleted) {
            printf("DELETED");
        } else {
            printf("NULL");
        }
        printf("\n");
    }
}

// Free the hash table
void freeHashTable(HashTable* ht) {
    free(ht->table);
    free(ht);
    printf("Hash table memory freed.\n");
}
```

`practice.c`

```C
int main() {
    // Create a hash table with initial size
    HashTable* table = createHashTable(INITIAL_SIZE);

    // Insert key-value pairs
    insert(&table, 10, 100);
    insert(&table, 20, 200);
    insert(&table, 15, 150);
    insert(&table, 7, 70);
    insert(&table, 8, 80);
    insert(&table, 22, 220);
    insert(&table, 1, 10);

    // Display the hash table
    display(table);

    // Insert more elements to trigger resizing
    insert(&table, 17, 170);
    insert(&table, 27, 270);

    // Display the hash table after resizing
    display(table);

    // Search for keys
    int key = 15;
    int value = search(table, key);
    if (value != -1) {
        printf("\nKey %d found with value %d.\n", key, value);
    } else {
        printf("\nKey %d not found.\n", key);
    }

    key = 99;
    value = search(table, key);
    if (value != -1) {
        printf("Key %d found with value %d.\n", key, value);
    } else {
        printf("Key %d not found.\n", key);
    }

    // Delete some keys
    deleteKey(&table, 20);
    deleteKey(&table, 7);

    // Display the hash table after deletions
    display(table);

    // Delete more keys to potentially trigger resizing down
    deleteKey(&table, 10);
    deleteKey(&table, 15);
    deleteKey(&table, 8);

    // Display the hash table after further deletions
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(&table, 100);

    // Free the hash table
    freeHashTable(table);

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==15629== Memcheck, a memory error detector
==15629== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15629== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15629== Command: ./practice
==15629== 
Inserted key 10 with value 100 at index 3
Inserted key 20 with value 200 at index 6
Inserted key 15 with value 150 at index 1
Inserted key 7 with value 70 at index 0
Resized and rehashed to new size 14
Inserted key 8 with value 80 at index 8
Inserted key 22 with value 220 at index 11
Inserted key 1 with value 10 at index 5

Hash Table Contents (Size: 14, Count: 7):
Slot 0: NULL
Slot 1: (15, 150)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: (1, 10)
Slot 6: (20, 200)
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: (22, 220)
Slot 12: NULL
Slot 13: NULL
Inserted key 17 with value 170 at index 3
Resized and rehashed to new size 28
Inserted key 27 with value 270 at index 27

Hash Table Contents (Size: 28, Count: 9):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: (20, 200)
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)

Key 15 found with value 150.

Key 99 not found.
Key 20 deleted from index 20.
Key 7 deleted from index 7.

Hash Table Contents (Size: 28, Count: 7):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: DELETED
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: DELETED
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)
Key 10 deleted from index 10.
Resized and rehashed to new size 14
Key 15 deleted from index 6.
Key 8 deleted from index 8.

Hash Table Contents (Size: 14, Count: 4):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: (17, 170)
Slot 4: NULL
Slot 5: NULL
Slot 6: DELETED
Slot 7: NULL
Slot 8: DELETED
Slot 9: NULL
Slot 10: NULL
Slot 11: (22, 220)
Slot 12: NULL
Slot 13: (27, 270)
Key 100 not found. Cannot delete.
Hash table memory freed.
==15629== 
==15629== HEAP SUMMARY:
==15629==     in use at exit: 0 bytes in 0 blocks
==15629==   total heap usage: 9 allocs, 9 frees, 1,844 bytes allocated
==15629== 
==15629== All heap blocks were freed -- no leaks are possible
==15629== 
==15629== For lists of detected and suppressed errors, rerun with: -s
==15629== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



---

## Big O Notations of Stacks, Queues, Deques, Linked Lists,  Ring Buffer & Hash Table

Big O notation is used to describe the performance or complexity of algorithms and data structures, particularly in terms of time and space.

### **1. Stack**

A **stack** is a Last-In-First-Out (LIFO) data structure. It supports operations primarily at one end called the "top."

**Common Operations:**

- Push (Add an element to the top):
  - **Time Complexity:** O(1)
- Pop (Remove the top element):
  - **Time Complexity:** O(1)
- Peek/Top (View the top element without removing it):
  - **Time Complexity:** O(1)
- isEmpty (Check if the stack is empty):
  - **Time Complexity:** O(1)

**Implementation Notes:**

- Typically implemented using dynamic arrays (like Python lists) or linked lists.
- Both array-based and linked list-based implementations offer constant time complexity for the primary operations.

------

### **2. Queue**

A **queue** is a First-In-First-Out (FIFO) data structure. It allows insertion at the rear and removal from the front.

**Common Operations:**

- Enqueue (Add an element to the rear):
  - **Time Complexity:** O(1)
- Dequeue (Remove the front element):
  - **Time Complexity:** O(1)
- Front/Peek (View the front element without removing it):
  - **Time Complexity:** O(1)
- isEmpty (Check if the queue is empty):
  - **Time Complexity:** O(1)

**Implementation Notes:**

- Can be implemented using linked lists (with pointers to both head and tail) or circular buffers.
- Both implementations ensure constant time operations for enqueue and dequeue.

------

### **3. Deque (Double-Ended Queue)**

A **deque** allows insertion and deletion at both ends (front and rear).

**Common Operations:**

- Push Front (Add an element to the front):
  - **Time Complexity:** O(1)
- Push Back (Add an element to the rear):
  - **Time Complexity:** O(1)
- Pop Front (Remove the front element):
  - **Time Complexity:** O(1)
- Pop Back (Remove the rear element):
  - **Time Complexity:** O(1)
- Front/Peek Front (View the front element):
  - **Time Complexity:** O(1)
- Back/Peek Back (View the rear element):
  - **Time Complexity:** O(1)
- isEmpty (Check if the deque is empty):
  - **Time Complexity:** O(1)

**Implementation Notes:**

- Commonly implemented using doubly linked lists or dynamic arrays with circular buffering.
- Both ends are accessible in constant time.

------

### **4. Linked List**

A **linked list** is a linear data structure where each element (node) points to the next (and possibly the previous) node.

There are two primary types:

- **Singly Linked List:** Each node points to the next node.
- **Doubly Linked List:** Each node points to both the next and the previous nodes.

**Common Operations:**

- **Insertion:**
  - At Head:
    - **Time Complexity:** O(1)
  - At Tail:
    - Time Complexity:
      - **Singly Linked List:** O(n) (unless a tail pointer is maintained)
      - **Doubly Linked List:** O(1) (if tail pointer is maintained)
  - At a Given Position:
    - **Time Complexity:** O(n) (requires traversal)
- **Deletion:**
  - From Head:
    - **Time Complexity:** O(1)
  - From Tail:
    - Time Complexity:
      - **Singly Linked List:** O(n) (requires traversal)
      - **Doubly Linked List:** O(1) (if tail pointer is maintained)
  - From a Given Position:
    - **Time Complexity:** O(n) (requires traversal)
- **Search:**
  - **Time Complexity:** O(n)
- **Access (Retrieve element at a specific position):**
  - **Time Complexity:** O(n)
- **isEmpty (Check if the list is empty):**
  - **Time Complexity:** O(1)

**Implementation Notes:**

- **Singly Linked Lists** are simpler and use less memory per node but have limitations in backward traversal.
- **Doubly Linked Lists** allow efficient bidirectional traversal and can perform certain deletions more efficiently but use more memory.
- Maintaining pointers to both head and tail can optimize certain operations to constant time.

#### **Additional Notes**

- **Space Complexity:** Generally, all these data structures require O(n) space, where n is the number of elements stored.
- **Implementation Choices:** The actual performance can depend on specific implementation details. For example, using a dynamic array for a stack or queue can lead to occasional O(n) operations when resizing is needed, but these are amortized to O(1).
- **Use Cases:**
  - **Stacks** are ideal for scenarios like function call management, undo mechanisms in applications, and depth-first search algorithms.
  - **Queues** are suitable for scheduling tasks, breadth-first search, and handling asynchronous data.
  - **Deques** are versatile and can be used to implement both stacks and queues, as well as for algorithms that require access from both ends.
  - **Linked Lists** are useful when you need efficient insertions and deletions from the middle of the list without reallocating or reorganizing the entire structure.

### Ring Buffer

| Operation                          | Time Complexity |
| ---------------------------------- | --------------- |
| Enqueue (Add an element)           | O(1)            |
| Dequeue (Remove an element)        | O(1)            |
| Peek/Front (View first element)    | O(1)            |
| IsFull (Check if buffer is full)   | O(1)            |
| IsEmpty (Check if buffer is empty) | O(1)            |
| Access by Index                    | O(1)            |



### Hash Table

| Operation                        | Time Complexity | Worst Case |
| -------------------------------- | --------------- | ---------- |
| Insert (Add a key-value pair)    | O(1)            | O(n)       |
| Delete (Remove a key-value pair) | O(1)            | O(n)       |
| Search (Retrieve value by key)   | O(1)            | O(n)       |
| Access (Check if key exists)     | O(1)            | O(n)       |
| Iteration over All Elements      | O(n)            | O(n)       |

---

## Generic Containers

In the C programming language, **generic containers** refer to data structures that can store elements of any data type. Unlike languages that natively support generics (such as C++ with templates or Java with generics), C does not provide built-in mechanisms for creating truly type-agnostic data structures. However, developers have devised various techniques to implement generic behavior in C, enabling the creation of versatile and reusable containers.

- **Generic containers** are data structures designed to handle multiple data types without requiring specific implementations for each type. 
- They enhance code reusability and flexibility, allowing developers to write more abstract and maintainable code.
- In C, achieving genericity involves creating containers that can operate on data of any type, typically by abstracting the data type through pointers or other mechanisms.

### Techniques to Implement Generic Containers in C

#### 1. Using `void*` Pointers

The most common method to achieve genericity in C is by using `void*` (pointer to `void`) types. Since `void*` can point to any data type, it allows functions and data structures to handle different types uniformly.

**Pros:**

- Simple to implement.
- Highly flexible.

**Cons:**

- Lack of type safety.
- Requires explicit casting, which can lead to runtime errors.
- Potential performance overhead due to pointer dereferencing.

#### 2. Macros

C preprocessor macros can generate type-specific code during compilation. By defining macros that take the data type as a parameter, developers can create type-safe generic containers.

**Pros:**

- Type safety is enforced at compile-time.
- No runtime overhead.

**Cons:**

- Code can become harder to read and debug.
- Increased compilation time.
- Potential for code bloat due to multiple instances of similar code.

#### 3. Code Generation

Using external scripts or tools to generate type-specific implementations from a generic template. Tools like `m4`, `GPP`, or custom scripts can automate this process.

**Pros:**

- Maintains type safety.
- Reduces manual coding effort.

**Cons:**

- Adds complexity to the build process.
- Requires maintaining separate template files.

#### 4. `_Generic` Keyword (C11)

Introduced in the C11 standard, the `_Generic` keyword allows for compile-time type selection, enabling a form of generic programming by selecting different code paths based on the type of a variable.

**Pros:**

- Type safety is maintained.
- No runtime overhead.
- Cleaner syntax compared to macros.

**Cons:**

- Limited to selecting existing functions or operations.
- Not as flexible as true generic programming found in other languages.

### Common Generic Containers in C

#### 1. Generic Linked Lists

A linked list where each node contains a `void*` pointer to data, allowing storage of any data type.

**Structure Example:**

```c
typedef struct Node {
    void* data;
    struct Node* next;
} Node;

typedef struct LinkedList {
    Node* head;
    Node* tail;
    size_t size;
} LinkedList;
```

#### 2. Dynamic Arrays (Vectors)

Dynamic arrays that can resize and hold elements of any type via `void*` pointers.

**Structure Example:**

```c
typedef struct Vector {
    void** items;
    size_t capacity;
    size_t size;
} Vector;
```

#### 3. Stacks and Queues

Stacks and queues implemented using `void*` pointers to allow generic data storage.

**Structure Example (Stack):**

```c
typedef struct Stack {
    void** items;
    size_t capacity;
    size_t top;
} Stack;
```

#### 4. Hash Tables

Hash tables that use `void*` for keys and/or values, enabling storage of various data types.

**Structure Example:**

```c
typedef struct HashTableEntry {
    void* key;
    void* value;
    struct HashTableEntry* next;
} HashTableEntry;

typedef struct HashTable {
    HashTableEntry** buckets;
    size_t num_buckets;
} HashTable;
```



### C Code Example

A simple implementation of a generic linked list using `void*`:

`practice.h`

```C
typedef struct Node
{
    void *data;
    struct Node *next;
} Node;

Node *create_node(void *data);
void append(Node **head, void *data);
void print_int_list(Node *head);

void free_list(Node *head);
```

`functions.c`

```C
Node *create_node(void *data)
{
    Node *new_node = malloc(sizeof(Node));
    if (new_node == NULL)
    {
        printf("Error: malloc failed\n");
        exit(1);
    }
    new_node->data = data;
    new_node->next = NULL;
    return new_node;
}
void append(Node **head, void *data)
{
    Node *new_node = create_node(data);
    if (*head == NULL)
    {
        *head = new_node;
        return;
    }

    Node *current = *head;
    while (current->next != NULL)
    {
        current = current->next;
    }
    current->next = new_node;
}
void print_int_list(Node *head)
{
    Node *current = head;
    while (current != NULL)
    {
        printf("%d -> ", *(int *)current->data);
        current = current->next;
    }
    printf("NULL\n");
}

void free_list(Node *head)
{
    Node *current = head;
    while (current != NULL)
    {
        Node *tmp = current;
        current = current->next;
        free(tmp);
    }
}
```

`practice.c`

```C
int main()
{
    Node *head = NULL;
    int a = 1, b = 2, c = 3;

    append(&head, &a);
    append(&head, &b);
    append(&head, &c);

    print_int_list(head);

    free_list(head);
    return 0;
}

```



```shell
chan@CMA:~/C_Programming/practice$ make valgrind
clang -std=c23 -Wall -Wextra -g -c practice.c -o ./obj/practice.o 
clang -std=c23 -Wall -Wextra -g -c functions.c -o ./obj/functions.o 
clang -std=c23 -Wall -Wextra -g -c functions_2.c -o ./obj/functions_2.o
ar -rcs ./libs/libfunctions.a ./obj/functions.o ./obj/functions_2.o
clang -std=c23 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto
valgrind --leak-check=full ./practice 
==12231== Memcheck, a memory error detector
==12231== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==12231== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==12231== Command: ./practice
==12231== 
1 -> 2 -> 3 -> NULL
==12231== 
==12231== HEAP SUMMARY:
==12231==     in use at exit: 0 bytes in 0 blocks
==12231==   total heap usage: 4 allocs, 4 frees, 1,072 bytes allocated
==12231== 
==12231== All heap blocks were freed -- no leaks are possible
==12231== 
==12231== For lists of detected and suppressed errors, rerun with: -s
==12231== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



### Advantages and Disadvantages

#### Advantages

1. **Reusability:** Write once, use with any data type.
2. **Maintainability:** Easier to maintain a single generic implementation rather than multiple type-specific versions.
3. **Flexibility:** Can handle various data types without modification.

#### Disadvantages

1. **Type Safety:** Using `void*` can lead to runtime errors if not managed carefully.
2. **Performance Overhead:** Indirection through pointers can incur performance penalties.
3. **Complexity:** Implementing and using generic containers can be more complex, especially with macros or code generation.
4. **Debugging Difficulty:** Errors related to type casting or pointer misuse can be harder to trace.

---

## Object-Oriented Programming with C

- In some circumstances, it make sense to design data structures or functions to work on any type that supplies the right operations.
- To make this work, we will attach the functions for manipulating an object to the object itself.
- This is the central idea behind object-oriented programming.

`practice.h`

- Objects in this system have three methods: `clone`, which makes a copy of the object, `print`, which sends a representation of the object to `stdout`, and `destroy`, which frees the object.
- Each of these methods takes the object itself as its first argument `self`, since C provides no other mechanism to tell these functions which object to work on.

```C
struct object
{
    const struct methods *methods;
};

typedef struct object Object;

struct methods
{
    Object *(*clone)(const Object *self);
    void (*print)(const Object *self);
    void (*destroy)(Object *self);
};

Object *intObjectCreate(int value);
Object *stringObjectCreate(const char *value);

struct intObject
{
    struct methods *methods;
    int value;
};

struct stringObject
{
    struct methods *methods;
    char *value;
};

static void printInt(const Object *s);
static Object *cloneInt(const Object *s);
static void destroyInt(Object *s);

static struct methods intObjectMethods = {
    cloneInt,
    printInt,
    destroyInt};

static void printInt(const Object *self)
{
    printf("%d", ((struct intObject *)self)->value);
}

static Object *cloneInt(const Object *self)
{
    return intObjectCreate(((struct intObject *)self)->value);
}

static void destroyInt(Object *self)
{
    free(self);
}

static void printString(const Object *s);
static Object *cloneString(const Object *s);
static void destroyString(Object *s);

static struct methods stringObjectMethods = {
    cloneString,
    printString,
    destroyString};

static void printString(const Object *self)
{
    printf("%s", ((struct stringObject *)self)->value);
}

static Object *cloneString(const Object *self)
{
    return stringObjectCreate(((struct stringObject *)self)->value);
}

static void destroyString(Object *self)
{
    free(((struct stringObject *)self)->value);
    free(self);
}

struct elt
{
    struct elt *next;
    Object *value;
};

typedef struct elt *Stack;

Stack *stackCreate(void);

void stackDestroy(Stack *s);
void stackPush(Stack *s, Object *value);

Object *stackPop(Stack *s);

int stackNotEmpty(const Stack *s);

void stackPrint(const Stack *s);
```

`functions.c`

- Internally, this stack will use the `clone` method to ensure that it gets its own copy of anything pushed onto the stack in `stackPush` to protect against the caller later destroying or modifying the object being pushed.
- The `print` method to print objects in `stackPrint`, and the `destroy` method to clean up in `stackDestroy`.

```C
Object *intObjectCreate(int value)
{
    struct intObject *self = malloc(sizeof(struct intObject));
    assert(self);

    self->methods = &intObjectMethods;
    self->value = value;

    return (Object *)self;
}

Object *stringObjectCreate(const char *value)
{
    struct stringObject *self = malloc(sizeof(struct stringObject));
    assert(self);

    self->methods = &stringObjectMethods;
    self->value = strdup(value);
    return (Object *)self;
}

Stack *stackCreate(void)
{
    Stack *s = malloc(sizeof(Stack));
    assert(s);

    *s = 0;
    return s;
}

void stackDestroy(Stack *s)
{
    Object *o;
    while (stackNotEmpty(s))
    {
        o = stackPop(s);
        o->methods->destroy(o);
    }
    free(s);
}
void stackPush(Stack *s, Object *value)
{
    struct elt *e = malloc(sizeof(struct elt));
    e->next = *s;
    e->value = value->methods->clone(value);

    // set the stack pointer to point to the new element which put it to the top of the stack
    *s = e;
}

Object *stackPop(Stack *s)
{
    // save the current top element in e
    struct elt *e = *s;

    // save the value of the top element in ret
    Object *ret = e->value;

    // set the stack pointer to point to the next element of the top element
    *s = e->next;

    // free the top element,effectively getting rid of the top element
    free(e);

    // return popped value
    return ret;
}

int stackNotEmpty(const Stack *s)
{
    return *s != 0;
}

void stackPrint(const Stack *s)
{
    for (struct elt *e = *s; e; e = e->next)
    {
        e->value->methods->print(e->value);
        putchar(' ');
    }
    putchar('\n');
}
```

- A messy line in C like `e->value->methods->print(e->value)` would look in C++ like `e->value.print()`.

`practice.c`

```C
#define N (3)
int main(int argc, char *argv[])
{
    char str[] = "hi";
    Object *o;

    int n = N;
    if (argc >= 2)
    {
        n = atoi(argv[1]);
    }

    Stack *s = stackCreate();

    for (int i = 0; i < n; i++)
    {
        // push a string onto the stack
        // creates a string obj, pushes it onto the stack and then destroy the original obj
        str[0] = 'a' + i;
        o = stringObjectCreate(str);
        stackPush(s, o);
        o->methods->destroy(o);
        stackPrint(s);

        // push an int onto the stack
        o = intObjectCreate(i);
        stackPush(s, o);
        o->methods->destroy(o);
        stackPrint(s);
    }

    while (stackNotEmpty(s))
    {
        // pops each obj from the stack, prints it within square brackets, destroys it, and displays the current state of the stack
        o = stackPop(s);
        putchar('[');
        o->methods->print(o);
        o->methods->destroy(o);
        fputs("] ", stdout);
        stackPrint(s);
    }

    stackDestroy(s);
    return 0;
}
```

- Why `o->methods->destroy(o);` is necessary after each push operation:
  - **The Lifecycle of Objects in the Stack**
    1. **Creation:**
       - We create an object (`o`) using either `intObjectCreate` or `stringObjectCreate`. This allocates memory for the object and initializes its fields.
    2. **Pushing onto the Stack:**
       - When we call `stackPush(s, o);`, the stack performs the following:
         - **Cloning the Object:** It uses the `clone` method (`o->methods->clone(o)`) to create a **duplicate** of the object.
         - **Storing the Clone:** The cloned object is what gets stored in the stack's top node.
       - **Original Object (`o`):** After cloning, the original object (`o`) is **no longer needed** by the stack since the stack has its own copy.
    3. **Destroying the Original Object:**
       - To prevent **memory leaks**, we must **free** the memory allocated for the original object once it's no longer needed.
       - This is achieved by calling `o->methods->destroy(o);`, which deallocates the memory used by `o`.

#### Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==24481== Memcheck, a memory error detector
==24481== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==24481== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==24481== Command: ./practice
==24481== 
ai 
0 ai 
bi 0 ai 
1 bi 0 ai 
ci 1 bi 0 ai 
2 ci 1 bi 0 ai 
[2] ci 1 bi 0 ai 
[ci] 1 bi 0 ai 
[1] bi 0 ai 
[bi] 0 ai 
[0] ai 
[ai] 
==24481== 
==24481== HEAP SUMMARY:
==24481==     in use at exit: 0 bytes in 0 blocks
==24481==   total heap usage: 26 allocs, 26 frees, 1,338 bytes allocated
==24481== 
==24481== All heap blocks were freed -- no leaks are possible
==24481== 
==24481== For lists of detected and suppressed errors, rerun with: -s
==24481== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



## Recursion

- Recursion is when a function calls itself.
- Some programming languages (particularly functional programming languages like Scheme, ML, or Haskell use recursion as a basic tool for implementing algorithms that in other languages would typically be expressed using iteration (loops)).
- Procedural languages like C tend to emphasize iteration over recursion, but can support recursion as well.

`practice.h`

```C
void printRangeIterative(int start, int stop);
void printRangeRecursive(int start, int stop);
void printRangeRecursiveReversed(int start, int stop);
void printRangeRecursiveSplit(int start, int stop);
```

`functions.c`

```C
void printRangeIterative(int start, int stop)
{
    for (int i = start; i < stop; i++)
    {
        printf("%d\n", i);
    }
}
void printRangeRecursive(int start, int stop)
{
    if (start < stop)
    {
        printf("%d\n", start);
        printRangeRecursive(start + 1, stop);
    }
}
void printRangeRecursiveReversed(int start, int stop)
{
    if (start < stop)
    {
        printRangeRecursiveReversed(start + 1, stop);
        printf("%d\n", start);
    }
}
void printRangeRecursiveSplit(int start, int stop)
{
    int mid;

    if (start < stop)
    {
        mid = (start + stop) / 2;
        printRangeRecursiveSplit(start, mid);
        printf("%d\n", mid);
        printRangeRecursiveSplit(mid + 1, stop);
    }
}
```

`practice.c`

```C
#define Noisy(x) (puts(#x), x)

int main(int argc, char *argv[])
{
    if (argc != 1)
    {
        fprintf(stderr, "Usage: %s\n", argv[0]);
        return 1;
    }

    Noisy(printRangeIterative(0, 10));
    Noisy(printRangeRecursive(0, 10));
    Noisy(printRangeRecursiveReversed(0, 10));
    Noisy(printRangeRecursiveSplit(0, 10));
    return 0;
}
```

`Output`

```shell
chan@CMA:~/C_Programming/practice$ ./practice
printRangeIterative(0, 10)
0
1
2
3
4
5
6
7
8
9
printRangeRecursive(0, 10)
0
1
2
3
4
5
6
7
8
9
printRangeRecursiveReversed(0, 10)
9
8
7
6
5
4
3
2
1
0
printRangeRecursiveSplit(0, 10)
0
1
2
3
4
5
6
7
8
9

```

- The function `printRangeRecusive` is an example of solving a problem using a divide and conquer approach.
- When we hit the bottom, the call stack will look something like this:

```C
printRangeRecursive(0, 10)
    printRangeRecursive(1, 10)
    printRangeRecursive(2, 10)
    printRangeRecursive(3, 10)
    printRangeRecursive(4, 10)
    printRangeRecursive(5, 10)
    printRangeRecursive(6, 10)
    printRangeRecursive(7, 10)
    printRangeRecursive(8, 10)
    printRangeRecursive(9, 10)
    printRangeRecursive(10, 10)
```

- This works because each call to `printRangeRecursive` gets its own parameters and its own variables separate from the others, even the ones that are still in prograss.
- So each will print out `start` and then call another copy in to print `start+1` etc.
- In the last call, we finally fail the test `start<stop`, so the function exits, then its parent exits, and so on until we unwind all the calls on the stack back to the first one.
- In `printRangeRecursiveReversed`, the calling pattern is exactly the same, but now instead of printing `start` on the way down, we print `start` on the way back, after making the recursive call.
  - This means that in `printRecursiveReversed(0, 10)`, 0 is printed only after the results of `printRangeRecursiveReversed(1, 10)`, which gives us the countdown effect.
- `printRecursiveSplit` takes a much more aggressive approach to dividing up the problem, it splits a range [0, 10] as two ranges [0, 5) and [6, 10) separated by a midpoint 5.

```C
printRangeRecursiveSplit(0, 10)
    printRangeRecursiveSplit(0, 5)
    printRangeRecursiveSplit(0, 2)
    printRangeRecursiveSplit(0, 1)
    printRangeRecursiveSplit(0, 0)
    printRangeRecursiveSplit(1, 1)
    printRangeRecursiveSplit(2, 2)
    printRangeRecursiveSplit(3, 5)
    printRangeRecursiveSplit(3, 4)
    printRangeRecursiveSplit(3, 3)
    printRangeRecursiveSplit(4, 4)
    printRangeRecursiveSplit(5, 5)
    printRangeRecursiveSplit(6, 10)   
    ... etc.
```

- the computation has the structure of a tree instead of a list, so it is not so obvious how one might rewrite this procedure as a loop.

### Blowing out the stack

- Blowing out the stack is what happens when a recursion is too deep.
- Typically, the OS puts a hard limit on how big the stack can grow, on the assumption that any program that grows the stack too much has gone insane and needs to be killed before it does more damage.
- One of the ways this can happen is if we forget the base case, but it can also happen if we just try to use a recursive function to do too much.
- For example, if we call `printRangeRecursive(0, 1000000)`, we will probably get a segmentation fault after the first 100, 000 numbers or so.
- For this reason, it's best to try to avoid linear recursions like the one in `printRangeRecursive`, where the depth of the recursion is proportional to the number of things we are doing.
- Much safer are even splits like `printRangeRecursiveSplit`, since the depth of the stack will now be only logarithmic in the number of things we are doing.
- Fortunately, linear recursions are often **tail-recursive**, where the recursive call is the last thing the recursive function does.

### Failure to make progress

Sometimes we end up blowing out the stack because we though we were recursing on a smaller instance of the problem, but in fact we weren't.

```C
void printRangeRecursiveSplitBad(int start, int stop){
    int mid;
    if(start == stop){
        print("%d\n", start);
    }else{
        mid = (start + stop) / 2;
        printRangeRecursiveSplitBad(start, mid);
        printRangeRecursiveSplitBad(mid, stop);
    }
}
```

- This will get stack on as simple a call as `printRangeRecursiveSplitBad(0, 1)`.
- It will set `mid` to 0.
- While the recursive call to `printRangeRecursiveSplitBad(0, 0)` will work just fine, the recursive call to `printRangeRecursiveSplitBad(0, 1)` will put us back where we started, giving us infinite recursion.



### Tail-recursion and iteration

- **Tail recursion** is when a recursive function calls itself only once, and as the last thing it does.
- The `printRangeRecursive` function is an example of a tail-recursive function.

```C
void printRangeRecursive(int start, int stop){
    if(start < stop){
        printf("%d\n", start);
        printRangeRecursive(start + 1, stop);
    }
}
```

- The nice thing about tail-recursive functions is that we can always translate them directly into iterative functions.
- The reason is that when we do the tail call, we are effectively replacing the current copy of the function with a new copy with new arguments.
- So rather than keeping around the old zombie parent copy which has no purpose other than to wait for the child to return and then return itself, we can reuse it by assigning new values to its arguments and jumping back to the top of the function.



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

---

## Trees

A **tree** is a way to organize data hierarchically. Think of it like a family tree:

- **Root**: The top person in the family.
- **Nodes**: Each person (including the root).
- **Edges**: The connections between people (like parent to child).

```markdown
      Root
     /    \
  Child1  Child2
   /  \
Grand1 Grand2

```

### Why Use Trees?

- **Organize Data**: Like folders in our computer.
- **Fast Searching**: Quickly find items.
- **Hierarchical Relationships**: Represent parent-child relationships.

### Understanding Tree Terminology

- **Node**: An individual element of a tree containing data.
- **Root**: The topmost node of a tree, with no parent.
- **Internal Node**: A node that has one or more children.
- **Leaf (or External Node)**: A node with no children.
- **Subtree**: A tree entirely contained within another tree, rooted at a specific node.
- **Depth/Height**: The level of a node within the tree; the root has depth 0, its children depth 1, and so on.

### Basic Tree Implementation in C

#### Program Code

`practice.h`

```C
typedef struct Node
{
    int data;
    struct Node *left;
    struct Node *right;
} TreeNode;

TreeNode *createNode(int value);
void inOrder(TreeNode *root);

void freeTree(TreeNode *root);

```

`functions.c`

```C
TreeNode *createNode(int value)
{
    TreeNode *newNode = malloc(sizeof(TreeNode));
    if (!newNode)
    {
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// in-order traversal: left, root, right
void inOrder(TreeNode *root)
{
    if (root != NULL)
    {
        inOrder(root->left); // visit left child
        printf("%d ", root->data); // visit node itself
        inOrder(root->right); // visit right child
    }
}

void freeTree(TreeNode *root)
{
    if (root == NULL)
    {
        return;
    }
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(1);
    root->left = createNode(2);
    root->right = createNode(3);
    root->left->left = createNode(4);
    root->left->right = createNode(5);
    
    // Now the tree looks like this:
    //       1
    //     /   \
    //    2     3
    //   / \
    //  4   5

    // traverse and print the tree
    inOrder(root); // output: 4 2 5 1 3
    printf("\n");
    freeTree(root);
    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==23562== Memcheck, a memory error detector
==23562== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==23562== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==23562== Command: ./practice
==23562== 
4 2 5 1 3 
==23562== 
==23562== HEAP SUMMARY:
==23562==     in use at exit: 0 bytes in 0 blocks
==23562==   total heap usage: 6 allocs, 6 frees, 1,144 bytes allocated
==23562== 
==23562== All heap blocks were freed -- no leaks are possible
==23562== 
==23562== For lists of detected and suppressed errors, rerun with: -s
==23562== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

### Key Points to Remember

- **Node Structure**: Each node holds data and pointers to its children.
- **Root**: The starting point of the tree.
- **Pointers**: Used to connect nodes (like links).
- **Traversal**: Ways to visit all nodes (in-order, pre-order, post-order).
  - **in-order:** left, root, right
  - **pre-order:** root, left, right
  - **post-order:** left, right, root

### What is a Binary Search Tree (BST)?

A **Binary Search Tree (BST)** is a type of binary tree where each node has at most two children, and it maintains a specific order:

- **Left Subtree**: All nodes have values **less than** the parent node.
- **Right Subtree**: All nodes have values **greater than** the parent node.

This property makes BSTs efficient for operations like searching, insertion, and deletion.

- Structurally, a complete binary tree consists of either a single node (a leaf) or a root node with a left and right **subtree**, each of which is itself either a leaf or a root node with two subtrees.

**Visual Representation**

```markdown
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

- **Example**: Searching for the number `7` involves comparing with `8` (go left), then `3` (go right), then `6` (go right), and finally finding `7`.



### Common Operations on BSTs

#### a. Insertion

**Goal**: Add a new node to the BST while maintaining its properties.

**Algorithm**:

1. Start at the **root**.
2. Compare the **value** to insert with the current node.
3. If the value is **less**, go to the **left child**; if **greater**, go to the **right child**.
4. Repeat until you find a `NULL` spot and insert the new node there.

```C
#include <stdio.h>
#include <stdlib.h>

// Define the structure for a tree node
struct Node {
    int data;
    struct Node* left;
    struct Node* right;
} TreeNode;

// Function to create a new node
TreeNode* createNode(int value) {
    struct Node* newNode = malloc(sizeof(struct Node));
    if (!newNode) {
        printf("Memory error\n");
        return NULL;
    }
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into BST
TreeNode* insert(TreeNode* root, int value) {
    if (root == NULL) {
        // Tree is empty, return new node
        return createNode(value);
    }

    if (value < root->data) {
        // Insert in the left subtree
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        // Insert in the right subtree
        root->right = insert(root->right, value);
    }
    // If value == root->data, do not insert duplicates
    return root;
}
```

#### b. Searching

**Goal**: Find whether a value exists in the BST.

**Algorithm**:

1. Start at the **root**.
2. Compare the **target value** with the current node.
3. If equal, **found**.
4. If the target is **less**, search the **left subtree**; if **greater**, search the **right subtree**.
5. Repeat until found or reach `NULL`.

```C
// Function to search a value in BST
TreeNode* search(TreeNode* root, int key) {
    if (root == NULL || root->data == key)
        return root;

    if (key < root->data)
        return search(root->left, key);
    else
        return search(root->right, key);
}

```

#### c. Traversal

Traversing a tree means visiting all its nodes in a specific order. The three common depth-first traversals are:

1. **In-Order Traversal**: Left → Root → Right
2. **Pre-Order Traversal**: Root → Left → Right
3. **Post-Order Traversal**: Left → Right → Root

```C
// In-Order Traversal
void inOrder(struct Node* root) {
    if (root != NULL) {
        inOrder(root->left);
        printf("%d ", root->data);
        inOrder(root->right);
    }
}

// Pre-Order Traversal
void preOrder(struct Node* root) {
    if (root != NULL) {
        printf("%d ", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}

// Post-Order Traversal
void postOrder(struct Node* root) {
    if (root != NULL) {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d ", root->data);
    }
}
```

**Example Usage**:

```C
int main() {
    TreeNode* root = NULL;
    root = insert(root, 8);
    insert(root, 3);
    insert(root, 10);
    insert(root, 1);
    insert(root, 6);
    insert(root, 14);
    insert(root, 4);
    insert(root, 7);
    insert(root, 13);
    
    //       8
    //		/ \
    //     3  10
    //   /  \  \
    //  1   6   14
    //	   / \   /
    //    4   7  13 

    printf("In-Order Traversal: ");
    inOrder(root); // Output: 1 3 4 6 7 8 10 13 14
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root); // Output: 8 3 1 6 4 7 10 14 13
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root); // Output: 1 4 7 6 3 13 14 10 8
    printf("\n");

    // Searching for a value
    int key = 7;
    TreeNode* found = search(root, key);
    if (found)
        printf("Value %d found in the BST.\n", key);
    else
        printf("Value %d not found in the BST.\n", key);

    return 0;
}
```

#### d. Deletion

**Goal**: Remove a node from the BST while maintaining its properties.

**Cases to Handle**:

1. **Node to be deleted is a leaf** (no children): Simply remove it.
2. **Node has one child**: Replace the node with its child.
3. **Node has two children**: Find the **in-order successor** (smallest value in the right subtree), copy its value to the node, and delete the successor.

```C
// Function to find the minimum value node in a BST
TreeNode* findMin(TreeNode* root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}

// Function to delete a node from BST
TreeNode* deleteNode(struct Node* root, int key) {
    if (root == NULL) return root;

    if (key < root->data) {
        // Key is in the left subtree
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->data) {
        // Key is in the right subtree
        root->right = deleteNode(root->right, key);
    }
    else {
        // Node found

        // Case 1: No child
        if (root->left == NULL && root->right == NULL) {
            free(root);
            root = NULL;
        }
        // Case 2: One child (right)
        else if (root->left == NULL) {
            struct Node* temp = root;
            root = root->right;
            free(temp);
        }
        // Case 2: One child (left)
        else if (root->right == NULL) {
            struct Node* temp = root;
            root = root->left;
            free(temp);
        }
        // Case 3: Two children
        else {
            TreeNode* temp = findMin(root->right);
            root->data = temp->data;
            root->right = deleteNode(root->right, temp->data);
        }
    }
    return root;
}

```



### Example Code: Comprehensive BST Implementation

#### Program Code

`practice.h`

```C
typedef struct Node
{
    int data;
    struct Node *left;
    struct Node *right;
} TreeNode;

TreeNode *createNode(int value);

TreeNode *insert(TreeNode *root, int value);
TreeNode *search(TreeNode *root, int key);
void inOrder(TreeNode *root);
void preOrder(TreeNode *root);
void postOrder(TreeNode *root);

TreeNode *findMin(TreeNode *root);
TreeNode *deleteNode(TreeNode *root, int key);
void freeTree(TreeNode *root);
```

`functions.c`

```C
TreeNode *createNode(int value)
{
    TreeNode *newNode = malloc(sizeof(TreeNode));
    if (!newNode)
    {
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}

TreeNode *insert(TreeNode *root, int value)
{
    if (root == NULL)
    {
        return createNode(value);
    }

    if (value < root->data)
    {
        root->left = insert(root->left, value);
    }
    else if (value > root->data)
    {
        root->right = insert(root->right, value);
    }
	
    // duplicate values are not inserted
    return root;
}

TreeNode *search(TreeNode *root, int key)
{
    if (root == NULL || root->data == key)
    {
        return root;
    }

    if (key < root->data)
    {
        return search(root->left, key);
    }
    else
    {
        return search(root->right, key);
    }
}

void inOrder(TreeNode *root)
{
    if (root != NULL)
    {
        inOrder(root->left);
        printf("%d ", root->data);
        inOrder(root->right);
    }
}

void preOrder(TreeNode *root)
{
    if (root != NULL)
    {
        printf("%d ", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}

void postOrder(TreeNode *root)
{
    if (root != NULL)
    {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d ", root->data);
    }
}

TreeNode *findMin(TreeNode *root)
{
    while (root->left != NULL)
    {
        root = root->left;
    }
    return root;
}
TreeNode *deleteNode(TreeNode *root, int key)
{
    if (root == NULL)
    {
        return root;
    }

    if (key < root->data)
    {
        // key is in the left subtree
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->data)
    {
        // key is in the right subtree
        root->right = deleteNode(root->right, key);
    }
    else
    {
        // Node found
        
        
        // Case 1: No child
        if (root->left == NULL && root->right == NULL)
        {
            free(root);
            root = NULL;
        }
        // Case 1: One child (right)
        else if (root->left == NULL)
        {
            TreeNode *temp = root;
            // replace the node with its child
            root = root->right;
            free(temp);
        }
        // case 2: One child (left)
        else if(root->right == NULL){
            TreeNode *temp = root;
            // replace the node with its child
            root = root->left;
            free(temp);
        }
        // case 3: Two children
        // get the inorder successor (smallest in the right subtree)
        else{
            TreeNode *temp = findMin(root->right);
            root->data = temp->data;
            
            // recursion to make sure we go thru the node to be deleted have no children
            root->right = deleteNode(root->right, temp->data);
        }
    }
    return root;
}

void freeTree(TreeNode *root)
{
    if (root == NULL)
    {
        return;
    }
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}
```

**Example Scenario of Delete Opeartions**:

Consider deleting a node with value `50` from the following BST:

```css
        50
       /  \
     30    70
     / \   / \
   20  40 60  80
```

- **Step 1**: Locate `50` (root).
- **Step 2**: `50` has two children.
- **Step 3**: Find inorder successor (`60`).
- **Step 4**: Replace `50`'s data with `60`.
- **Step 5**: Delete node `60`, which has no children.

```css
        60
       /  \
     30    70
     / \     \
   20 40     80
```

- The `deleteNode` function efficiently removes a specified node from a BST while maintaining the tree's structural properties. 
- By handling different cases based on the node's children and utilizing the inorder successor for nodes with two children, the function ensures that the BST remains valid after deletion.

`practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(1);
    root = insert(root, 50);
    insert(root, 30);
    insert(root, 20);
    insert(root, 40);
    insert(root, 70);
    insert(root, 60);
    insert(root, 80);

    printf("In-Order Traversal: ");
    inOrder(root); // 20 30 40 50 60 70 80
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root);
    printf("\n");

    // Search for a value
    int key = 40;
    TreeNode *found = search(root, key);
    if (found)
    {
        printf("Value %d found in the BST.\n", key);
    }
    else
    {
        printf("Value %d not found in the BST.\n", key);
    }

    // Delete a node
    root = deleteNode(root, 20);
    root = deleteNode(root, 30);
    root = deleteNode(root, 50);

    printf("In-Order Traversal after deletion:");
    inOrder(root);
    printf("\n");

    printf("Pre-Order Traversal after deletion:");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal after deletion:");
    postOrder(root);
    printf("\n");

    freeTree(root);

    return 0;
}
```

**Visual Representation of BST Operations**:

**Building the BST**

**Insertion Order:** 1, 50, 30, 20, 40, 70, 60, 80

#### **Step-by-Step Insertion**

1. **Insert 1**

   ```css
   1
   ```

   

2. **Insert 50**

   ```css
     1
      \
      50
   ```

3. **Insert 30**

   ```css
     1
      \
      50
     /
    30
   ```

4. **Insert 20**

   ```css
      1
       \
       50
      /
     30
    /
   20
   ```

   

5. **Insert 40**

   ```css
      1
       \
       50
      /
     30
    /  \
   20  40
   ```

   

6. **Insert 70**

   ```css
      1
       \
       50
      /  \
     30   70
    /  \
   20  40
   ```

   

7. **Insert 60**

   ```css
      1
       \
       50
      /  \
     30   70
    /  \  /
   20  40 60
   ```

   

8. **Insert 80**

   ```css
        1
         \
         50
        /  \
       30   70
      /  \  / \
     20  40 60 80
   ```

**Traversals Before Deletion**:

1. In-Order Traversal (Left, Root, Right)

   ```css
   1 20 30 40 50 60 70 80
   ```

**Visualization**:

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**In-Order Traversal Steps:**

1. Visit left subtree of 1 (None)
2. Visit 1
3. Visit left subtree of 50:
   - Visit left subtree of 30:
     - Visit left subtree of 20 (None)
     - Visit 20
     - Visit right subtree of 20 (None)
   - Visit 30
   - Visit right subtree of 30:
     - Visit left subtree of 40 (None)
     - Visit 40
     - Visit right subtree of 40 (None)
4. Visit 50
5. Visit right subtree of 50:
   - Visit left subtree of 70:
     - Visit left subtree of 60 (None)
     - Visit 60
     - Visit right subtree of 60 (None)
   - Visit 70
   - Visit right subtree of 70:
     - Visit left subtree of 80 (None)
     - Visit 80
     - Visit right subtree of 80 (None)

**Pre-Order Traversal (Root, Left, Right)**:

```css
1 50 30 20 40 70 60 80
```

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**Pre-Order Traversal Steps:**

1. Visit 1
2. Visit 50
3. Visit 30
4. Visit 20
5. Visit 40
6. Visit 70
7. Visit 60
8. Visit 80



**Post-Order Traversal (Left, Right, Root)**

```css
20 40 30 60 80 70 50 1
```

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**Post-Order Traversal Steps:**

1. Visit left subtree of 1 (None)
2. Visit right subtree of 1:
   - Visit left subtree of 50:
     - Visit left subtree of 30:
       - Visit left subtree of 20 (None)
       - Visit right subtree of 20 (None)
       - Visit 20
     - Visit right subtree of 30:
       - Visit left subtree of 40 (None)
       - Visit right subtree of 40 (None)
       - Visit 40
     - Visit 30
   - Visit right subtree of 50:
     - Visit left subtree of 70:
       - Visit left subtree of 60 (None)
       - Visit right subtree of 60 (None)
       - Visit 60
     - Visit right subtree of 70:
       - Visit left subtree of 80 (None)
       - Visit right subtree of 80 (None)
       - Visit 80
     - Visit 70
   - Visit 50
3. Visit 1

**Searching for a Value**:

**Search Key:** 40

**Search Path:**

1. Start at root (1)
2. 40 > 1 → Move to right child (50)
3. 40 < 50 → Move to left child (30)
4. 40 > 30 → Move to right child (40)
5. **Found 40**

**Deletions**:

**Nodes to Delete:** 20, 30, 50

**Delete 20**

**Original Tree:**

```css
        1
         \
         50
        /  \
       30   70
      /  \  / \
     20  40 60 80
```

**Deletion Process:**

- **Node 20** is a **leaf node** (no children).
- Remove node 20 directly.

**Tree After Deleting 20:**

```css
    1
     \
     50
    /  \
   30   70
     \  / \
     40 60 80
```

**Delete 30**

**Original Tree:**

```css
        1
         \
         50
        /  \
       30   70
         \  / \
         40 60 80
```

**Deletion Process:**

- **Node 30** has **one child** (40).
- Replace node 30 with its child (40).

**Tree After Deleting 30:**

```css
        1
         \
         50
        /  \
       40   70
           / \
          60 80
```

**Delete 50**

**Original Tree:**

```css
        1
         \
         50
        /  \
       40   70
           / \
          60 80
```

**Deletion Process:**

- **Node 50** has **two children** (40 and 70).
- Find **Inorder Successor** (smallest node in the right subtree): **60**.
- Replace node 50's data with 60.
- Delete node 60 (which is now a leaf node).

**Tree After Deleting 50:**

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Traversals After Deletions**:

**In-Order Traversal**

```css
1 40 60 70 80
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Pre-Order Traversal**

```css
1 60 40 70 80
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Post-Order Traversal**

```css
40 80 70 60 1
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Final Tree Structure**

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

#### Explanation of `FreeTree()`

Consider the following BST:

```css
        50
       /  \
     30    70
    / \    / \
  20  40  60 80
```

- **Node Values:** 50 (root), 30, 70, 20, 40, 60, 80

Let's walk through how `freeTree` deallocates each node in the BST using **Post-Order Traversal**.

**a. Initial Call**

- **Function Call:** `freeTree(root)` where `root` points to node `50`.

**b. Processing the Left Subtree of 50**

1. **Call:** `freeTree(30)`
2. Call: `freeTree(20)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(20)` → Node `20` is deallocated.
3. Call: `freeTree(40)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(40)` → Node `40` is deallocated.
4. **Action:** `free(30)` → Node `30` is deallocated.

**State of the Tree**:

```css
        50
          \
           70
          / \
        60 80
```

**Processing the Right Subtree of 50**

1. **Call:** `freeTree(70)`
2. Call: `freeTree(60)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(60)` → Node `60` is deallocated.
3. Call: `freeTree(80)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(80)` → Node `80` is deallocated.
4. **Action:** `free(70)` → Node `70` is deallocated.

**State of the Tree:**

```css
        50
```

**Final Action on Root**

- **Action:** `free(50)` → Node `50` is deallocated.

**State of the Tree:**

```css
    (Empty Tree)
```

**Visual Representation of the Deallocation Process**

```css
freeTree(50)
├─ freeTree(30)
│  ├─ freeTree(20)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ freeTree(NULL)
│  ├─ Action: free(20)
│  ├─ freeTree(40)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(40)
│  └─ Action: free(30)
├─ freeTree(70)
│  ├─ freeTree(60)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(60)
│  ├─ freeTree(80)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(80)
│  └─ Action: free(70)
└─ Action: free(50)
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==32744== Memcheck, a memory error detector
==32744== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==32744== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==32744== Command: ./practice
==32744== 
In-Order Traversal: 1 20 30 40 50 60 70 80 
Pre-Order Traversal: 1 50 30 20 40 70 60 80 
Post-Order Traversal: 20 40 30 60 80 70 50 1 
Value 40 found in the BST.
In-Order Traversal after deletion:1 40 60 70 80 
Pre-Order Traversal after deletion:1 60 40 70 80 
Post-Order Traversal after deletion:40 80 70 60 1 
==32744== 
==32744== HEAP SUMMARY:
==32744==     in use at exit: 0 bytes in 0 blocks
==32744==   total heap usage: 9 allocs, 9 frees, 1,216 bytes allocated
==32744== 
==32744== All heap blocks were freed -- no leaks are possible
==32744== 
==32744== For lists of detected and suppressed errors, rerun with: -s
==32744== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



#### Adding Height to our program

```C
int treeHeight(TreeNode *root){
    if(root == NULL){
        return -1; // height of empty tree is -1
    }
    
    int leftHeight = treeHieght(root->left);
    int rightHeight = treeHeight(root->right);
    return (leftHeight > rightHeight ? leftHeight : rightHeight) + 1;
}
```

In `practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(50);
    insert(root, 30);
    insert(root, 20);
    insert(root, 40);
    insert(root, 70);
    insert(root, 60);
    insert(root, 80);
    root = insert(root, 1);

    printf("In-Order Traversal: ");
    inOrder(root); // 20 30 40 50 60 70 80
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root);
    printf("\n");

    // Search for a value
    int key = 40;
    TreeNode *found = search(root, key);
    if (found)
    {
        printf("Value %d found in the BST.\n", key);
    }
    else
    {
        printf("Value %d not found in the BST.\n", key);
    }

    int height = treeHeight(root);
    printf("Height of the tree: %d\n", height);

    // Delete a node
    root = deleteNode(root, 20);
    root = deleteNode(root, 30);
    root = deleteNode(root, 50);

    printf("In-Order Traversal after deletion:");
    inOrder(root);
    printf("\n");

    printf("Pre-Order Traversal after deletion:");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal after deletion:");
    postOrder(root);
    printf("\n");

    height = treeHeight(root);
    printf("Height of the tree after deletion: %d\n", height);

    freeTree(root);

    return 0;
}
```

`Output`

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==12706== Memcheck, a memory error detector
==12706== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==12706== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==12706== Command: ./practice
==12706== 
In-Order Traversal: 1 20 30 40 50 60 70 80 
Pre-Order Traversal: 50 30 20 1 40 70 60 80 
Post-Order Traversal: 1 20 40 30 60 80 70 50 
Value 40 found in the BST.
Height of the tree: 3
In-Order Traversal after deletion:1 40 60 70 80 
Pre-Order Traversal after deletion:60 40 1 70 80 
Post-Order Traversal after deletion:1 40 80 70 60 
Height of the tree after deletion: 2
==12706== 
==12706== HEAP SUMMARY:
==12706==     in use at exit: 0 bytes in 0 blocks
==12706==   total heap usage: 9 allocs, 9 frees, 1,216 bytes allocated
==12706== 
==12706== All heap blocks were freed -- no leaks are possible
==12706== 
==12706== For lists of detected and suppressed errors, rerun with: -s
==12706== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



#### Additional Concepts

- The **size** of a tree is the number of nodes: a leaf by itself has size 1.

##### Height of the Tree

- The height of a tree is the number of edges on the longest path from the root to a leaf.
- 0 for a leaf, at least one in any larger tree.
- The **depth** of a node is the length of the path from the root to that node.
- The **height** of a node is the height of the subtree of which it is the root, i.e, the length of the longest path from that node to some leaf below it.
- A node `u` is an **ancestor** of a node `v` if `v` is contained in the subtree rooted at `u`.
  - We may write equivalently that `v` is a descendant of `u`.
- Every node is both an ancestor and a descendent of itself.

##### Key Takeways of BST:

- **BST Properties**: Left child < parent < right child.
- **Efficiency**: BST operations (search, insert, delete) have an average time complexity of **O(log n)**, but can degrade to **O(n)** if the tree becomes unbalanced.
- **Traversal Methods**: Essential for accessing and processing all nodes in the tree.
- **Recursive Thinking**: Many tree operations are naturally implemented using recursion.



### Heap

A **heap** is a special **binary tree** that satisfies two main properties:

1. **Complete Binary Tree**: All levels of the tree are fully filled except possibly the last level, which is filled from left to right.
2. **Heap Property:**
   - **Max-Heap**: Every parent node is **greater than or equal to** its child nodes.
   - **Min-Heap**: Every parent node is **less than or equal to** its child nodes.

#### Visual Representation:

**Max-Heap Example:**

```css
      10
     /  \
    9    8
   / \  /
  7  6 5
```

**Min-Heap Example:**

```css
       1
     /   \
    3     5
   / \   /
  7   9 8
```

#### Types of Heaps

1. **Max-Heap**: The largest element is at the root. Useful when we need quick access to the maximum element.
2. **Min-Heap**: The smallest element is at the root. Useful when we need quick access to the minimum element.

**Applications:**

- **Priority Queues**: Heaps efficiently manage elements with priorities.
- **Heap Sort**: An efficient sorting algorithm.
- **Graph Algorithms**: Such as Dijkstra’s shortest path. (pronounced dike.struh).

#### Why Use a Heap?

- **Efficient Operations**: Heaps allow insertion and deletion of the root element in **O(log n)** time.
- **Access to Extremes**: Quickly access the maximum or minimum element.
- **Space-Efficient**: Can be efficiently represented using arrays.

#### Heap Representation in C

Heaps are typically implemented using **arrays** because of their complete binary tree property. Here's how:

#### Array Representation:

- **Root Node**: Stored at index `0`.
- For any node at index `i`:
  - **Left Child**: `2*i + 1`
  - **Right Child**: `2*i + 2`
  - **Parent**: `(i - 1) / 2`

#### Example:

Consider the max-heap:

```css
      10
     /  \
    9    8
   / \  /
  7  6 5
```

**Array Representation**: `[10, 9, 8, 7, 6, 5]`

If we apply the formula above, 

- let's say we want to find the left and right child of `9`, `9` is at index `1` so, the left child = 2 * 1 + 1 = 3, and if we check at index 3, we can see `7` which is the left child of 9. Similarly, the left child of `9` = 2 * 1 + 2 = 4, and at index `4`, we can see the value `6` which is the right child of `9`as we can see it in the tree.
- let's say we want to find the parent of `7` and `8`, `7` is at index 3, its parent = 3 - 1 / 2 = 1, at index 1, the value is `9`, so we now know that the parent of `7` is `9`. 
  - Let's check for the parent of `8`, `8` is at index 2, so its parent = 2 -1 / 2 = 0, which is `10` at index 0. So the parent of `8` is `10`.

#### Common Heap Operations

##### Insertion

**Goal**: Add a new element to the heap while maintaining the heap property.

**Steps**:

1. **Add** the new element at the **end** of the array.
2. **Heapify Up**: Compare the new element with its parent and **swap** if necessary. Repeat until the heap property is restored.

##### Deletion (Extract-Max or Extract-Min)

**Goal**: Remove the root element (maximum for max-heap, minimum for min-heap).

**Steps**:

1. **Replace** the root with the **last** element in the array.
2. **Remove** the last element.
3. **Heapify Down**: Compare the new root with its children and **swap** with the larger child (for max-heap) or smaller child (for min-heap). Repeat until the heap property is restored.

##### Heapify

**Heapify** is the process of adjusting the heap to maintain the heap property after insertion or deletion.



### Max-Heap

#### Example Code in C (Max-Heap)

Let’s implement a **Max-Heap** in C with the following functionalities:

1. **Insert** an element.
2. **Extract-Max** (remove and return the maximum element).
3. **Display** the heap.

##### Program Code

`practice.h`

```C
typedef struct
{
    int data[MAX_SIZE];
    int size;
} MaxHeap;

void initHeap(MaxHeap *heap);
void heapifyUp(MaxHeap *heap, int index);
void insertHeap(MaxHeap *heap, int value);
void heapifyDown(MaxHeap *heap, int index);
int extractMax(MaxHeap *heap);
void displayHeap(MaxHeap *heap);
```

`functions.c`

```C
// Initialize the heap
void initHeap(MaxHeap *heap)
{
    heap->size = 0;
}

// Heapify up to maintain heap property
void heapifyUp(MaxHeap *heap, int index)
{
    // Checks if the current index is not the root of the heap. In C, any non-zero value is considered true, and the root of the heap is at index 0. So, index being non-zero means the current node is not the root.
    // && Compares the current node's value with its parent's value. The parent of any node at index i in a zero-based array representation of a heap is at index (i - 1) / 2.
    if (index && heap->data[index] > heap->data[(index - 1) / 2])
    {
        // Swap current node with parent
        int temp = heap->data[index];
        heap->data[index] = heap->data[(index - 1) / 2];
        heap->data[(index - 1) / 2] = temp;

        // Recursively heapify the parent node
        heapifyUp(heap, (index - 1) / 2);
    }
}

// Insert a new element into the heap
void insertHeap(MaxHeap *heap, int value)
{
    if (heap->size == MAX_SIZE)
    {
        printf("Heap is full!\n");
        return;
    }

    // Insert the new element at the end
    heap->data[heap->size] = value;
    heap->size++;

    // Heapify up to maintain heap property
    heapifyUp(heap, heap->size - 1);
}

// Heapify down to maintain heap property
void heapifyDown(MaxHeap *heap, int index)
{
    int largest = index;
    int left = 2 * index + 1;  // Left child index
    int right = 2 * index + 2; // Right child index

    // If left child exists and is greater than current largest
    if (left < heap->size && heap->data[left] > heap->data[largest])
        largest = left;

    // If right child exists and is greater than current largest
    if (right < heap->size && heap->data[right] > heap->data[largest])
        largest = right;

    // If largest is not the current index
    if (largest != index)
    {
        // Swap
        int temp = heap->data[index];
        heap->data[index] = heap->data[largest];
        heap->data[largest] = temp;

        // Recursively heapify the affected sub-tree
        heapifyDown(heap, largest);
    }
}

// Extract the maximum element from the heap
int extractMax(MaxHeap *heap)
{
    if (heap->size <= 0)
        return -1; // Indicates heap is empty

    if (heap->size == 1)
    {
        heap->size--;
        return heap->data[0];
    }

    // Store the maximum value
    int root = heap->data[0];

    // Move the last element to root
    heap->data[0] = heap->data[heap->size - 1];
    heap->size--;

    // Heapify down from root to maintain heap property
    heapifyDown(heap, 0);

    return root;
}

// Display the heap
void displayHeap(MaxHeap *heap)
{
    for (int i = 0; i < heap->size; i++)
    {
        printf("%d ", heap->data[i]);
    }
    printf("\n");
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    MaxHeap heap;
    initHeap(&heap);

    // Insert elements into the heap
    insertHeap(&heap, 10);
    insertHeap(&heap, 20);
    insertHeap(&heap, 5);
    insertHeap(&heap, 30);
    insertHeap(&heap, 40);
    insertHeap(&heap, 15);

    printf("Heap elements after insertion: ");
    displayHeap(&heap); // Output: 40 30 15 10 20 5

    // extract the maximum element
    int max = extractMax(&heap);
    printf("Extracted max: %d\n", max); // Output: 40

    printf("Heap elements after extracting max: ");
    displayHeap(&heap); // Output: 30 20 15 10 5

    // insert a new element
    insertHeap(&heap, 25);
    printf("Heap elements after inserting 25: ");
    displayHeap(&heap); // Output: 30 20 25 10 5 15
    return 0;
}
```

###### **Step-by-Step Heap Operations**

**a. Initial Insertions**

**Inserted Elements:** `40, 30, 15, 10, 20, 5`

**Heap Array Representation:**

```css
Index: 0  1  2  3  4  5
Value: 40 30 15 10 20 5
```

**Heap Structure**:

```css
        40
       /  \
     30    15
    /  \   /
  10   20 5
```

**Verification:**

- **40 ≥ 30** and **40 ≥ 15**
- **30 ≥ 10** and **30 ≥ 20**
- **15 ≥ 5**

**Result:** Valid Max Heap

**b. Extracting Max (40)**

**Operation:** Remove the maximum element (40).

**Steps:**

1. **Replace Root with Last Element:**
   - Move `5` to the root.
   - Heap Array: `[5, 30, 15, 10, 20]`
2. **Heapify Down from Root (Index 0):**
   - **Compare 5 with children `30` (Index 1) and `15` (Index 2).**
   - **30** is the larger child. Swap `5` with `30`.
   - Heap Array after swap: `[30, 5, 15, 10, 20]`
3. **Heapify Down from Child (Index 1):**
   - **Compare 5 with children `10` (Index 3) and `20` (Index 4).**
   - **20** is the larger child. Swap `5` with `20`.
   - Heap Array after swap: `[30, 20, 15, 10, 5]`

**Heap Structure After Extraction:**

```css
        30
       /  \
     20    15
    /  \
  10    5
```

**Verification:**

- **30 ≥ 20** and **30 ≥ 15**
- **20 ≥ 10** and **20 ≥ 5**
- **15** has no children

**Result:** Valid Max Heap

**c. Inserting 25**

**Steps**:

1. **Insert at the End:**
   - Add `25` at index `5`.
   - Heap Array: `[30, 20, 15, 10, 5, 25]`
2. **Heapify Up from Index 5:**
   - **Parent Index:** `(5 - 1) / 2 = 2` (Value `15`)
   - **Compare 25 with 15.** Since `25 > 15`, swap them.
   - Heap Array after swap: `[30, 20, 25, 10, 5, 15]`
3. **Heapify Up from New Parent Position (Index 2):**
   - **Parent Index:** `(2 - 1) / 2 = 0` (Value `30`)
   - **Compare 25 with 30.** Since `25 < 30`, no further swaps.

**Heap Structure After Inserting `25`:**

```css
        30
       /  \
     20    25
    /  \   /
  10    5 15
```

**Heap Array Representation:**

```css
Index: 0  1  2  3  4  5
Value: 30 20 25 10 5 15
```

**Verification:**

- **30 ≥ 20** and **30 ≥ 25**
- **20 ≥ 10** and **20 ≥ 5**
- **25 ≥ 15**

**Result:** **Valid Max Heap**

##### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Heap elements after insertion: 40 30 15 10 20 5 
Extracted max: 40
Heap elements after extracting max: 30 20 15 10 5 
Heap elements after inserting 25: 30 20 25 10 5 15 
```

### Min-Heap

A **Min-Heap** is a special type of **binary tree** that satisfies the following two main properties:

1. **Complete Binary Tree**: All levels of the tree are fully filled except possibly the last level, which is filled from left to right.
2. **Heap Property:**
   - **Min-Heap**: Every parent node is **less than or equal to** its child nodes.

**Visual Representation:**

```css
       5
      / \
     9   8
    / \  /
  15 10 12

```

In this min-heap:

- The smallest element (`5`) is at the root.
- Every parent node is smaller than its children.

**Complete Binary Tree**:

- Ensures that the heap is balanced, leading to efficient operations.
- Can be efficiently represented using arrays.

**Heap Property**:

- **Min-Heap**: For any given node `N`, the value of `N` is less than or equal to the values of its children.
- This property ensures that the smallest element is always at the root of the heap.

#### Min-Heap Usage

Min-heaps are widely used due to their efficient performance for specific operations:

- **Priority Queues**: Quickly access and remove the element with the highest priority (smallest element in min-heap).
- **Graph Algorithms**: Such as Dijkstra’s shortest path algorithm.
- **Heap Sort**: An efficient sorting algorithm.
- **Scheduling Algorithms**: Managing tasks based on priority.

#### Common operations of Min-Heap

##### a. Insertion

**Goal**: Add a new element to the heap while maintaining the heap property.

**Steps**:

1. **Add** the new element at the **end** of the array.
2. **Heapify Up**: Compare the new element with its parent and **swap** if it's smaller. Repeat until the heap property is restored.

##### b. Extraction (Extract-Min)

**Goal**: Remove and return the smallest element (the root) from the heap.

**Steps**:

1. **Replace** the root with the **last** element in the array.
2. **Remove** the last element.
3. **Heapify Down**: Compare the new root with its children and **swap** with the smaller child if necessary. Repeat until the heap property is restored.

##### c. Heapify

**Heapify** is the process of adjusting the heap to maintain the heap property after insertion or extraction.

- **Heapify Up**: Used after insertion.
- **Heapify Down**: Used after extraction.

#### **Array Representation:**

- **Root Node**: Stored at index `0`.
- For any node at index `i`:
  - **Left Child**: `2*i + 1`
  - **Right Child**: `2*i + 2`
  - **Parent**: `(i - 1) / 2`

🌟 **Example:**

Consider the min-heap:

```css
       5
      / \
     9   8
    / \  /
  15 10 12
```

**Array Representation**: `[5, 9, 8, 15, 10, 12]`

#### Example Code in C (Min-Heap)

`practice.h`

```C
#define MAX_SIZE 100

typedef struct
{
    int data[MAX_SIZE];
    int size;
} MinHeap;

void initHeap(MinHeap *heap);
void heapifyUp(MinHeap *heap, int index);
void insertHeap(MinHeap *heap, int key);
void heapifyDown(MinHeap *heap, int index);
int extractMin(MinHeap *heap);
void displayHeap(MinHeap *heap);
```

`functions.c`

```C
// Initialize the heap
void initHeap(MinHeap *heap)
{
    heap->size = 0;
}

// Heapify up to maintain heap property
void heapifyUp(MinHeap *heap, int index)
{
    // Checks if the current index is not the root of the heap. In C, any non-zero value is considered true, and the root of the heap is at index 0. So, index being non-zero means the current node is not the root.
    // && Compares the current node's value with its parent's value. The parent of any node at index i in a zero-based array representation of a heap is at index (i - 1) / 2.
    if (index && heap->data[index] < heap->data[(index - 1) / 2])
    {
        // Swap current node with parent
        int temp = heap->data[index];
        heap->data[index] = heap->data[(index - 1) / 2];
        heap->data[(index - 1) / 2] = temp;

        // Recursively heapify the parent node
        heapifyUp(heap, (index - 1) / 2);
    }
}

// Insert a new element into the heap
void insertHeap(MinHeap *heap, int value)
{
    if (heap->size == MAX_SIZE)
    {
        printf("Heap is full!\n");
        return;
    }

    // Insert the new element at the end
    heap->data[heap->size] = value;
    heap->size++;

    // Heapify up to maintain heap property
    heapifyUp(heap, heap->size - 1);
}

// Heapify down to maintain heap property
void heapifyDown(MinHeap *heap, int index)
{
    int smallest = index;
    int left = 2 * index + 1;  // Left child index
    int right = 2 * index + 2; // Right child index

    // If left child exists and is smaller than current smallest
    if (left < heap->size && heap->data[left] < heap->data[smallest])
        smallest = left;

    // If right child exists and is smaller than current smallest
    if (right < heap->size && heap->data[right] < heap->data[smallest])
        smallest = right;

    // If smallest is not the current index
    if (smallest != index)
    {
        // Swap
        int temp = heap->data[index];
        heap->data[index] = heap->data[smallest];
        heap->data[smallest] = temp;

        // Recursively heapify the affected sub-tree
        heapifyDown(heap, smallest);
    }
}

// Extract the minimum element from the heap
int extractMin(MinHeap *heap)
{
    if (heap->size <= 0)
        return -1; // Indicates heap is empty

    if (heap->size == 1)
    {
        heap->size--;
        return heap->data[0];
    }

    // Store the minimum value
    int root = heap->data[0];

    // Move the last element to root
    heap->data[0] = heap->data[heap->size - 1];
    heap->size--;

    // Heapify down from root to maintain heap property
    heapifyDown(heap, 0);

    return root;
}

// Display the heap
void displayHeap(MinHeap *heap)
{
    for (int i = 0; i < heap->size; i++)
    {
        printf("%d ", heap->data[i]);
    }
    printf("\n");
}
```

`practice.c`

```C
// Main function to demonstrate heap operations
int main() {
    MinHeap heap;
    initHeap(&heap);

    // Insert elements into the heap
    insertHeap(&heap, 10);
    insertHeap(&heap, 20);
    insertHeap(&heap, 5);
    insertHeap(&heap, 30);
    insertHeap(&heap, 40);
    insertHeap(&heap, 15);

    printf("Heap elements after insertion: ");
    displayHeap(&heap); // Expected Output: 5 10 15 30 40 20

    // Extract the minimum element
    int min = extractMin(&heap);
    printf("Extracted min: %d\n", min); // Expected Output: 5

    printf("Heap elements after extraction: ");
    displayHeap(&heap); // Expected Output: 10 20 15 30 40

    // Insert a new element
    insertHeap(&heap, 25);
    printf("Heap elements after inserting 25: ");
    displayHeap(&heap); // Expected Output: 10 20 15 30 40 25

    return 0;
}
```



#### Code Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Heap elements after insertion: 5 20 10 30 40 15 
Extracted min: 5
Heap elements after extraction: 10 20 15 30 40 
Heap elements after inserting 25: 10 20 15 30 40 25 

```

##### **Step-by-Step Explanation:**

1. **After Insertion**:

   - Inserted elements in the order: `10`, `20`, `5`, `30`, `40`, `15`.
   - The heap organizes itself to maintain the min-heap property.
   - Resulting heap: `[5, 10, 15, 30, 40, 20]`

   ```css
           5
         /   \
       10     15
      /  \    /
    30   40  20
   ```

   - **Root Node (Index 0):** `5`

   - **Left Child of 5 (Index 1):** `10`

   - **Right Child of 5 (Index 2):** `15`

   - **Left Child of 10 (Index 3):** `30`

   - **Right Child of 10 (Index 4):** `40`

   - **Left Child of 15 (Index 5):** `20`

   

2. **After Extracting Min (5)**:

   - The smallest element `5` is removed.
   - The last element `20` is moved to the root.
   - Heapify down adjusts the heap to maintain the min-heap property.
   - Resulting heap: `[10, 20, 15, 30, 40]`

   ```css
           10
         /    \
       20      15
      /  \
    30   40
   ```

   - **Root Node (Index 0):** `10` (after replacing `5` with the last element `20` and heapifying down)

   - **Left Child of 10 (Index 1):** `20`

   - **Right Child of 10 (Index 2):** `15`

   - **Left Child of 20 (Index 3):** `30`

   - **Right Child of 20 (Index 4):** `40`

3. **After Inserting 25**:

   - Inserted `25` into the heap.
   - Heapify up ensures the min-heap property is maintained.
   - Resulting heap: `[10, 20, 15, 30, 40, 25]`

   ```css
           10
         /    \
       20      15
      /  \    /
    30   40  25
   ```

   - **Root Node (Index 0):** `10`

   - **Left Child of 10 (Index 1):** `20`

   - **Right Child of 10 (Index 2):** `15`

   - **Left Child of 20 (Index 3):** `30`

   - **Right Child of 20 (Index 4):** `40`

   - **Left Child of 15 (Index 5):** `25`

**Min-Heap Property**:

- The smallest element is always at the root.
- Every parent node is smaller than or equal to its child nodes.

**Array Representation**:

- Efficiently represents a complete binary tree.
- Parent and child relationships can be easily calculated using indices.

**Heap Operations**:

- **Insertion**: O(log n) time complexity due to heapify up.
- **Extraction (Extract-Min)**: O(log n) time complexity due to heapify down.
- **Display**: O(n) time complexity to traverse the heap.

**Applications**:

- Priority queues, graph algorithms, heap sort, and more.

### Key Takeaways

- **Heap Property:**
  - **Max-Heap**: Parent ≥ Children
  - **Min-Heap**: Parent ≤ Children
- **Complete Binary Tree**: Efficiently represented using arrays.
- **Heap Operations:**
  - **Insertion**: O(log n)
  - **Extraction**: O(log n)
  - The time to complete the whole heapifyUp and heapifyDown operation is proportional to the depth of the heap, which will typically be O(log n).
- **Applications:**
  - **Priority Queues**: Managing tasks with priorities.
  - **Heap Sort**: An efficient sorting algorithm.
  - **Graph Algorithms**: Such as finding the shortest path.

---

## Heapsort

**Heap Sort** is a comparison-based sorting algorithm that utilizes a binary heap data structure to sort elements. It's an **in-place** and **unstable** sorting algorithm with a time complexity of **O(n log n)** in all cases (worst, average, and best).

### How Heap Sort Works

Heap Sort involves two main phases:

1. **Building a Heap:**
   - Convert the array into a heap (Max-Heap for ascending sort).
   - A Max-Heap ensures that the largest element is at the root.
2. **Extracting Elements:**
   - Swap the root (largest element) with the last element in the heap.
   - Reduce the heap size by one.
   - Heapify the root to maintain the heap property.
   - Repeat until the heap size is greater than one.

### Heap Sort Implementation in C

Below is a complete implementation of Heap Sort in C, using a Max-Heap to sort an array in ascending order.

#### Program Code

`practice.h`

```C
void heapify(int arr[], int n, int i);
void heapSort(int arr[], int n);
void printArray(int arr[], int n);
```

`functions.c`

```C
// func to heapify a subtree rooted at index i
void heapify(int arr[], int n, int i)
{
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    // if left child exists and is greater than root
    if (left < n && arr[left] > arr[largest])
    {
        largest = left;
    }
	
    // if right child exists and is greater than root
    if (right < n && arr[right] > arr[largest])
    {
        largest = right;
    }
	
    // if largest is not root, swap and continue heapifying
    if (largest != i)
    {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        heapify(arr, n, largest);
    }
}

// func to perform heap sort
void heapSort(int arr[], int n)
{
    // build a max-heap
    for (int i = n / 2 - 1; i >= 0; i--)
    {
        heapify(arr, n, i);
    }
	
    // one by one extract elements from heap
    // Swap the root (maximum element) with the last element.
    //Reduce the heap size by one and heapify the root to maintain the max heap.
    for (int i = n - 1; i > 0; i--)
    {
        // move current root to end (swap)
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
	
        
        // heapify the reduced heap
        heapify(arr, i, 0);
    }
}

void printArray(int arr[], int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original array: \n");
    printArray(arr, n);

    heapSort(arr, n);

    printf("Sorted array using Heap Sort: \n");
    printArray(arr, n);
    return 0;
}

```

#### Code Breakdown:

1. **Heapify Function:**

   - Ensures that the subtree rooted at index `i` satisfies the Max-Heap property.
   - Compares the parent node with its left and right children.
   - Swaps with the larger child if necessary and recursively heapifies the affected subtree.

2. **Heap Sort Function:**

   - Sort the array in ascending order by leveraging the max heap property to efficiently extract the largest elements and place them at the end of the array.

   - Building the Max-Heap:
     - Starts from the last non-leaf node (`n/2 - 1`) and heapifies each node up to the root.
   - Extracting Elements:
     - Swaps the root of the heap (largest element) with the last element in the array.
     - Reduces the heap size by one and heapifies the root to maintain the heap property.
     - Repeats until the heap size is reduced to one.

3. **Print Array Function:**

   - Simple utility to print the elements of the array.

4. **Main Function:**

   - Demonstrates the Heap Sort by sorting a sample array.

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Original array: 
12 11 13 5 6 7 
Sorted array using Heap Sort: 
5 6 7 11 12 13 
```

#### Visual Representation

1. **Original Array:**

   ```css
   [12, 11, 13, 5, 6, 7]
   ```

2. **Building the Max-Heap:**

   - Start heapifying from the last non-leaf node.

   - After heapify operations, the Max-Heap looks like:

     ```css
           13
         /    \
       12      7
      /  \    /
     5    6  11
     ```

   - **Array Representation:** `[13, 12, 11, 5, 6, 7]`

3. **Extracting Elements:**

   - Swap root `13` with last element `7`.

   - Heapify the root:

     ```css
          12
         /  \
        7    11
       /  \    
      5    6  
     ```

   - **Array Representation:** `[12, 7, 11, 5, 6, 13]`

   - Continue this process until the array is sorted.

4. **Final Sorted Array:**

   ```css
   [5, 6, 7, 11, 12, 13]
   ```

#### Program Output #2

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Original array: 
4 10 3 5 1 
Sorted array using Heap Sort: 
1 3 4 5 10 
```

#### Step-by-Step Heap Sort Example

**Example Array:** `[4, 10, 3, 5, 1]`

**Goal:** Sort the array in ascending order.

**Initial Array**

```css
Index: 0  1   2  3  4
Value: 4, 10, 3, 5, 1
```

**Building the Max Heap**

**Step 1:** Identify the last non-leaf node.

- **Formula:** `last_non_leaf = n / 2 - 1`
- **Calculation:** `5 / 2 - 1 = 1` (integer division)

**Start Heapifying from Index 1 to 0.**

**Heapify at Index 1**

- **Subtree Root:** `10`
- **Left Child (Index 3):** `5`
- **Right Child (Index 4):** `1`
- **Largest:** `10` (no swap needed)

**Heap After Heapify at Index 1:**

```css
[4, 10, 3, 5, 1]
```

**Heapify at Index 0**

- **Subtree Root:** `4`
- **Left Child (Index 1):** `10`
- **Right Child (Index 2):** `3`
- **Largest:** `10` (swap with root)

**Swap 4 and 10:**

```css
[10, 4, 3, 5, 1]
```

**Heapify at Index 1 (After Swap):**

- **Subtree Root:** `4`
- **Left Child (Index 3):** `5`
- **Right Child (Index 4):** `1`
- **Largest:** `5` (swap with root)

**Swap 4 and 5:**

```css
[10, 5, 3, 4, 1]
```

**Heap After Building Max Heap:**

```css
[10, 5, 3, 4, 1]
```

**Sorting the Heap**

Now, we repeatedly extract the maximum element and rebuild the heap.

**Heap Size:** `5`

**Iteration 1: i = 4**

1. **Swap Root with Last Element:**

```css
[1, 5, 3, 4, 10]
```

1. **Heapify Root (Index 0) with Heap Size = 4:**

- **Subtree Root:** `1`
- **Left Child (Index 1):** `5`
- **Right Child (Index 2):** `3`
- **Largest:** `5` (swap with root)

**Swap 1 and 5:**

```css
[5, 1, 3, 4, 10]
```

1. **Heapify at Index 1:**

- **Subtree Root:** `1`
- **Left Child (Index 3):** `4`
- **Right Child (Index 4):** Out of bounds
- **Largest:** `4` (swap with root)

**Swap 1 and 4:**

```css
[5, 4, 3, 1, 10]
```

**Array After Iteration 1:**

```css
[5, 4, 3, 1, 10]
```

**Iteration 2: i = 3**

1. **Swap Root with Last Unsorted Element of Heap which in this case is 5:**

```css
[1, 4, 3, 5, 10]
```

1. **Heapify Root (Index 0) with Heap Size = 3:**

- **Subtree Root:** `1`
- **Left Child (Index 1):** `4`
- **Right Child (Index 2):** `3`
- **Largest:** `4` (swap with root)

**Swap 1 and 4:**

```css
[4, 1, 3, 5, 10]
```

1. **Heapify at Index 1:**

- **Subtree Root:** `1`
- **Left Child (Index 3):** Out of bounds
- **Right Child (Index 4):** Out of bounds
- **Largest:** `1` (no swap needed)

**Array After Iteration 2:**

```css
[4, 1, 3, 5, 10]
```

**Iteration 3: i = 2**

1. **Swap Root with Last Unsorted Element (4 in this case) of Heap:**

```css
[3, 1, 4, 5, 10]
```

1. **Heapify Root (Index 0) with Heap Size = 2:**

- **Subtree Root:** `3`
- **Left Child (Index 1):** `1`
- **Right Child (Index 2):** Out of bounds
- **Largest:** `3` (no swap needed)

**Array After Iteration 3:**

```css
[3, 1, 4, 5, 10]
```

**Iteration 4: i = 1**

1. **Swap Root with Last Unsorted Element (1 in this case) of Heap:**

```css
[1, 3, 4, 5, 10]
```

1. **Heapify Root (Index 0) with Heap Size = 1:**

- **Only one element in heap; no action needed.**

**Array After Iteration 4:**

```css
[1, 3, 4, 5, 10]
```

**Final Sorted Array:**

```css
[1, 3, 4, 5, 10]
```



#### Visual Representation

Let's visualize the Heap Sort process using the example array `[4, 10, 3, 5, 1]`.

###### Step 1: Initial Array

```css
Index: 0  1   2  3  4
Value: 4, 10, 3, 5, 1
```

###### Step 2: Building the Max Heap

**Heapify at Index 1:**

- Subtree:

  ```css
      10
     /  \
    5    1
  ```

- **No swap needed as 10 > 5 and 10 > 1.**

**Heapify at Index 0:**

- Subtree:

  ```css
      4
     / \
   10   3
  / \
  ```

5 1

```cs
- **Largest is 10; swap 4 and 10:**
```

```css
  10
 /  \
4    3

```

/
5 1

~~~css
- **Now heapify at Index 1:**
- **Subtree:**
  ```
      4
     / \
    5    1
  ```
- **Largest is 5; swap 4 and 5:**
  ```
      10
     /  \
    5    3
   / \
  4    1
  ```

**Max Heap Built:**

~~~

```css
    10
   /  \
  5    3
 / \
4    1
```

```css
**Array Representation:** `[10, 5, 3, 4, 1]`

### Step 3: Sorting the Heap

**Iteration 1:**

- **Swap 10 and 1:**
```

```css
    1
   /  \
  5    3
 / \
4    10
```

```css
- **Heap Size Reduced to 4. Heapify Root:**
- **Subtree:**
```
~~~css
- **Heap Size Reduced to 4. Heapify Root:**
- **Subtree:**
  ```
      1
     /  \
    5    3
   /
  4
  ```
- **Largest is 5; swap 1 and 5:**
  ```
      5
     /  \
    1    3
   /
  4
  ```
- **Heapify at Index 1:**
  - **Subtree:**
    ```
      1
     /
    4
    ```
  - **Largest is 4; swap 1 and 4:**
    ```
        5
       /  \
      4    3
     /
    1
    ```
- **Array After Iteration 1:** `[5, 4, 3, 1, 10]`

**Iteration 2:**

- **Swap 5 and 1:**

~~~


```css

    1
   /  \
  4    3
 /
5
```

```css
- **Heap Size Reduced to 3. Heapify Root:**
- **Subtree:**
```
      1
     /  \
    4    3
  ```
- **Largest is 4; swap 1 and 4:**
  ```
      4
     /  \
    1    3
  ```
- **Array After Iteration 2:** `[4, 1, 3, 5, 10]`

**Iteration 3:**

- **Swap 4 and 3:**

  ```
        3
       /  \
      1    4
    ```
- Heap Size Reduced to 2. Heapify Root:

- Subtree:

  ```css
     3
     /
    1
  ```

- **Largest is 3; no swap needed.**

- **Array After Iteration 3:** `[3, 1, 4, 5, 10]`

**Iteration 4:**

- Swap 3 and 1:

  ```css
        1
       /
      3
  ```

- **Heap Size Reduced to 1. No heapify needed.**

- **Final Array:** `[1, 3, 4, 5, 10]`

------

### Time and Space Complexity

**Heapify Function:**

- Time Complexity:

   O(log n)

  - Each heapify call may traverse the height of the heap, which is log₂n for a binary heap.

- Space Complexity:

   O(1)

  - Operates in-place without additional memory.

**HeapSort Function:**

- Time Complexity:

   O(n log n)

  - Building the max heap takes O(n) time.
  - Extracting elements and heapifying takes O(n log n) time.

- Space Complexity:

   O(1)

  - Sorting is done in-place without additional memory.

**Overall Heap Sort:**

- **Time Complexity:** O(n log n)
- **Space Complexity:** O(1)

#### Advantages and Disadvantages

**Advantages:**

1. **Efficiency:** Heap Sort has a guaranteed O(n log n) time complexity, making it efficient for large datasets.
2. **In-Place Sorting:** Requires only a constant amount of additional space.
3. **No Recursive Calls Needed:** Can be implemented iteratively to avoid stack overflows.

**Disadvantages:**

1. **Not Stable:** Heap Sort does not preserve the relative order of equal elements.
2. **Poor Cache Performance:** Compared to algorithms like Quick Sort, Heap Sort may have less efficient cache usage due to its access patterns.
3. **Implementation Complexity:** Building and maintaining the heap requires careful index calculations.

---

## Augmented trees

- Augmented trees are an extension of standard binary search trees (BSTs) that store additional information in their nodes to efficiently support a broader range of queries and operations. 
- By augmenting a BST with extra data, we can solve more complex problems without compromising the tree's fundamental properties, such as search, insertion, and deletion operations.

An **augmented tree** is a binary search tree that, in addition to the standard key and value, stores extra information (often called "augmented data") in each node that caches values that might otherwise be expensive to compute.

- This might include information like the size of a subtree (which can be useful for ranking values, where we want to determine how many elements of the trees are smaller), the height of a subtree, or other summary information like the sum of all the keys in a subtree.

- This additional data is designed to answer more complex queries efficiently. 
- The augmented data must be maintained correctly during tree modifications (insertions and deletions) to ensure the tree remains accurate.

**Key Characteristics:**

- **Binary Search Tree Properties:** Nodes are organized such that for any given node, all keys in the left subtree are less than the node's key, and all keys in the right subtree are greater.
- **Augmented Data:** Extra fields in each node that provide additional information, such as subtree sizes, aggregate values, or other domain-specific data.
- **Efficient Query Support:** The augmented data allows for efficient implementation of operations that would otherwise require traversing the entire tree.

### Common Use Cases

Augmented trees are used in various applications, including:

- **Order-Statistic Trees:** Support operations like finding the k-th smallest element or determining the rank of an element.
- **Interval Trees:** Efficiently manage intervals and support queries like finding all intervals that overlap with a given interval.
- **Range Trees:** Support multi-dimensional range queries.
- **Segment Trees:** Efficiently perform range queries and updates on arrays.
- Storing the height field can be useful for balancing, as in AVL trees.

### Implementing an Augmented Binary Search Tree in C

Let's walk through implementing an **Order-Statistic Tree** in C as an example of an augmented tree. An Order-Statistic Tree allows us to:

- Find the k-th smallest element in the tree.
- Determine the rank (position) of a given element.

To achieve this, each node will store the size of its subtree.

**Key Operations**:

1. **Insert:** Adds a new key to the tree while maintaining the BST property and updating the `size` attribute.
2. **kthSmallest:** Retrieves the k-th smallest element by leveraging the `size` attribute.
3. **getRank:** Determines the rank of a given key in the tree.

#### Program Code

`practice.h`

```C
typedef struct Node
{
    int key;
    int size;
    struct Node *left, *right;
} Node;

Node *createNode(int key);
int getSize(Node *node);
void updateSize(Node *node);
Node *insert(Node *root, int key);
Node *kthSmallest(Node *root, int k);
int getRank(Node *root, int key);
void inorderTraversal(Node *root);
void freeTree(Node *root);
```

`functions.c`

```C
Node *createNode(int key)
{
    Node *newNode = malloc(sizeof(Node));
    if (!newNode)
    {
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }

    newNode->key = key;
    newNode->size = 1;
    newNode->left = newNode->right = NULL;
    return newNode;
}
int getSize(Node *node)
{
    return node ? node->size : 0;
}
void updateSize(Node *node)
{
    if (node)
    {
        node->size = 1 + getSize(node->left) + getSize(node->right);
    }
}

//Navigate the tree as in a standard BST insertion.
// After inserting the new node, update the size of each node on the path back to the root.
// This ensures that every node correctly reflects the total number of nodes in its subtree.
Node *insert(Node *root, int key)
{
    if (root == NULL)
    {
        return createNode(key);
    }

    if (key < root->key)
    {
        root->left = insert(root->left, key);
    }
    else if (key > root->key)
    {
        root->right = insert(root->right, key);
    }
    else
    {
        printf("Duplicate key %d not inserted.\n", key);
        return root;
    }

    updateSize(root);
    return root;
}

Node *kthSmallest(Node *root, int k)
{
    if (root == NULL)
    {
        return NULL;
    }

    // At each node, determine the size of its left subtree (leftSize).
    int leftSize = getSize(root->left);

    // If k equals leftSize + 1, the current node is the k-th smallest.
    if (k == leftSize + 1)
    {
        return root;
    }
    // if k is less than or equal to leftSize, recurse into the left subtree.
    else if (k <= leftSize)
    {
        return kthSmallest(root->left, k);
    }
    else
    {
        // adjusting k to exclude the left subtree and the current node
        return kthSmallest(root->right, k - leftSize - 1);
    }
}

int getRank(Node *root, int key)
{
    if (root == NULL)
    {
        return 0; // key not found
    }

    // Compare key with root->key:
    // Less: The rank resides in the left subtree.
   // Greater: Count all nodes in the left subtree and the current node, then search in the right subtree.
  // Equal: The rank is the number of nodes in the left subtree plus one (for the current node).
    if (key < root->key)
    {
        return getRank(root->left, key);
    }
    else if (key > root->key)
    {
        return 1 + getSize(root->left) + getRank(root->right, key);
    }
    else
    {
        return getSize(root->left) + 1;
    }
}
void inorderTraversal(Node *root)
{
    if (root != NULL)
    {
        inorderTraversal(root->left);
        printf("%d ", root->key);
        inorderTraversal(root->right);
    }
}

void freeTree(Node *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

- Visualization of `kthSmallest()`:

  1. **Base Case:** If the current `root` is `NULL`, return `NULL` (k is out of bounds).

  2. **Compute Left Subtree Size:** Determine the number of nodes in the left subtree (`leftSize`).

  3. **Compare `k` with `leftSize + 1`:**

     - **Equal:** The current node (`root`) is the k-th smallest.

     - **Less:** The k-th smallest lies in the left subtree.

     - **Greater:** The k-th smallest lies in the right subtree, adjusting `k` to exclude the left subtree and the current node.

Consider the following BST with the `size` attribute for each node indicating the number of nodes in its subtree (including itself):

- The `size` attribute in each node of the Binary Search Tree (BST) represents the **total number of nodes** in the subtree rooted at that particular node, **including the node itself**.

```css
             20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/     \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```

**Step-by-Step Execution:**

1. **Start at Root (20):**
   - `leftSize = size of left subtree of 20 (15 (size=3)) = 3` (nodes: 15, 10, 18)
   - `k = 4`
   - Compare:
     - `4 == 3 + 1` → `if(k == leftSize + 1) return root` →**True**
   - **Result:** The current node **20** is the 4th smallest element.

**Another Example:** Find the 5th smallest element (`k = 5`).

1. **Start at Root (20):**
   - `leftSize = 3`
   - `k = 5`
   - Compare:
     - `5 > 3 + 1` → **True**
   - Move to **right subtree** with `k = 5 - 3 - 1 = 1`
2. **Move to Node (25):**
   - `leftSize = 1` (node: 22)
   - `k = 1`
   - Compare:
     - `1 == 1 + 1` → **False**
     - `1 <= 1` → **True**
   - Move to **left subtree** with `k = 1`
3. **Move to Node (22):**
   - `leftSize = 0`
   - `k = 1`
   - Compare:
     - `1 == 0 + 1` → **True**
   - **Result:** The current node **22** is the 5th smallest element.

- **Visualization of `getRank()`**

```css
             20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/      \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```

**Objective:** Find the rank of key `22`.

**Step-by-Step Execution:**

1. Start at Root (20):
   - `key = 22`
   - `22 > 20`
   - Calculate: `1 (current node) + size of left subtree (3) = 4`
   - Move to **right subtree** with `key = 22`
2. Move to Node (25):
   - `key = 22`
   - `22 < 25`
   - Move to **left subtree**
3. Move to Node (22):
   - `key = 22 == 22`
   - Calculate: `size of left subtree (0) + 1 = 1`
   - **Result:** Rank = `4 (from previous step) + 1 = 5`

**Interpretation:** There are **5** elements in the BST that are less than or equal to `22`.

**Another Example:** Find the rank of key `15`.

1. Start at Root (20):
   - `15 < 20`
   - Move to **left subtree**
2. Move to Node (15):
   - `15 == 15`
   - Calculate: `size of left subtree of 15 (which is 10(size = 1)) (1) + 1 = 2`
   - **Result:** Rank = `2`

**Interpretation:** There are **2** elements in the BST that are less than or equal to `15`.

**Summary of Both Functions**

| Function      | Purpose                           | Time Complexity  |
| ------------- | --------------------------------- | ---------------- |
| `kthSmallest` | Find the k-th smallest element    | O(log n) average |
| `getRank`     | Find the number of elements ≤ key | O(log n) average |

`practice.c`

```C
int main(int argc, char *argv[])
{
    Node *root = NULL;
    int keys[] = {20, 15, 25, 10, 18, 22, 30};
    int n = sizeof(keys) / sizeof(keys[0]);

    // insert keys into the tree
    for (int i = 0; i < n; i++)
    {
        root = insert(root, keys[i]);
    }

    printf("In-order traversal of the tree:\n");
    inorderTraversal(root);

    // Find the 3rd smallest element
    int k = 3;
    Node *kth = kthSmallest(root, k);
    if (kth != NULL)
    {
        printf("\n%d-th smallest element is %d\n", k, kth->key);
    }
    else
    {
        printf("\nThere are fewer than %d elements in the tree.\n", k);
    }

    // Get the rank of a specific key
    int keyToFind = 18;
    int rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    freeTree(root);
    root = NULL;// avoid dangling pointer
    printf("The augmented tree has been successfully freed.\n");
    return 0;
}
```

**Visualization Diagrams**

**A. Finding the k-th Smallest Element (`k = 3`)**

```css
Step 1: Start at Root (20)
- leftSize = 3
- k = 3
- Compare: 3 == 3 + 1? No
- 3 <= 3? Yes
- Move to Left Subtree (15) with k = 3

Step 2: At Node (15)
- leftSize = 1
- k = 3
- Compare: 3 == 1 + 1? No
- 3 > 1 + 1? Yes
- Move to Right Subtree (18) with k = 3 - 1 - 1 = 1

Step 3: At Node (18)
- leftSize = 0
- k = 1
- Compare: 1 == 0 + 1? Yes
- **Result:** 18
```

Visualization:

```css
            20
           /  
         15   
           \   
            18  
```

**Finding the Rank of Key `22`**

```css
  			 20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/      \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```



```css
Step 1: Start at Root (20)
- key = 22 > 20
- Add 1 (current node) + leftSize (3) = 4
- Move to Right Subtree (25) with key = 22

Step 2: At Node (25)
- key = 22 < 25
- Move to Left Subtree (22)

Step 3: At Node (22)
- key = 22 == 22
- Add 0 (leftSize) + 1 = 1
- **Total Rank:** 4 + 1 = 5
```

**visualization**

```css
            20
              \
               25
              /
            22
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==14232== Memcheck, a memory error detector
==14232== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==14232== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==14232== Command: ./practice
==14232== 
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 18 is 3
The augmented tree has been successfully freed.
==14232== 
==14232== HEAP SUMMARY:
==14232==     in use at exit: 0 bytes in 0 blocks
==14232==   total heap usage: 8 allocs, 8 frees, 1,192 bytes allocated
==14232== 
==14232== All heap blocks were freed -- no leaks are possible
==14232== 
==14232== For lists of detected and suppressed errors, rerun with: -s
==14232== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

chan@CMA:~/C_Programming/practice$ ./practice
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 22 is 5
The augmented tree has been successfully freed.

```

#### Adding a delete function in our Augmented Trees

```C
Node *findMin(Node *root)
{
    while (root->left != NULL)
    {
        root = root->left;
    }
    return root;
}

Node *deleteNode(Node *root, int key)
{
    if (root == NULL)
    {
        return root;
    }

    if (key < root->key)
    {
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->key)
    {
        root->right = deleteNode(root->right, key);
    }
    else
    {
        // Node with only one child or no child
        if (root->left == NULL)
        {
            Node *temp = root->right;
            free(root);
            return temp;
        }
        else if (root->right == NULL)
        {
            Node *temp = root->left;
            free(root);
            return temp;
        }

        // Node with two children: Get the inorder successor (smallest in the right subtree)
        Node *temp = findMin(root->right);

        // copy the inorder successor's content to this node
        root->key = temp->key;

        // delete the inorder successor
        root->right = deleteNode(root->right, temp->key);
    }
    updateSize(root);
    return root;
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    Node *root = NULL;
    int keys[] = {20, 15, 25, 10, 18, 22, 30};
    int n = sizeof(keys) / sizeof(keys[0]);

    // insert keys into the tree
    for (int i = 0; i < n; i++)
    {
        root = insert(root, keys[i]);
    }

    printf("In-order traversal of the tree:\n");
    inorderTraversal(root);

    // Find the 3rd smallest element
    int k = 3;
    Node *kth = kthSmallest(root, k);
    if (kth != NULL)
    {
        printf("\n%d-th smallest element is %d\n", k, kth->key);
    }
    else
    {
        printf("\nThere are fewer than %d elements in the tree.\n", k);
    }

    // Get the rank of a specific key
    int keyToFind = 22;
    int rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    // Delete a node from the tree
    int keyToDelete = 15;
    printf("\nDeleting key %d from the tree.\n", keyToDelete);
    root = deleteNode(root, keyToDelete);

    printf("In-order traversal of the tree after deletion:\n");
    inorderTraversal(root);
    printf("\n");

    // verify the rank after deletion
    rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d after deletion is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    freeTree(root);
    root = NULL; // avoid dangling pointer
    printf("The augmented tree has been successfully freed.\n");
    return 0;
}
```

Initial BST Before Deletion:

```css
        20(size=7)
       /          \
  15(size=3)      25(size=3)
  /      \        /      \
10(size=1)18(size=1)22(size=1)30(size=1)
```

**Delete Key `15`:**

- **Node `15`** has two children: `10` (left) and `18` (right).
- **Inorder Successor:** The smallest node in the right subtree of `15`, which is `18`.
- **Action:** Replace `15` with `18` and adjust pointers accordingly.

BST After Deletion of Key `15`

```css
        20(size=6)
       /          \
  18(size=2)      25(size=3)
  /              /      \
10(size=1)     22(size=1) 30(size=1)
```



#### Output after adding a delete function

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==20431== Memcheck, a memory error detector
==20431== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20431== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20431== Command: ./practice
==20431== 
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 22 is 5

Deleting key 15 from the tree.
In-order traversal of the tree after deletion:
10 18 20 22 25 30 
Rank of 22 after deletion is 4
The augmented tree has been successfully freed.
==20431== 
==20431== HEAP SUMMARY:
==20431==     in use at exit: 0 bytes in 0 blocks
==20431==   total heap usage: 8 allocs, 8 frees, 1,192 bytes allocated
==20431== 
==20431== All heap blocks were freed -- no leaks are possible
==20431== 
==20431== For lists of detected and suppressed errors, rerun with: -s
==20431== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

#### Final BST Structure After Deletion of Key 15

```css
            20(size=6)
           /          \
      18(size=2)      25(size=3)
      /              /      \
    10(size=1)     22(size=1) 30(size=1)
```



### Maintaining Augmented Data During Operations

When augmenting a tree with additional data, it's crucial to ensure that this data remains accurate after every modification (insertion, deletion, rotation in balanced trees, etc.). Here are general guidelines:

1. **Insertion:**
   - After inserting a new node, backtrack and update the augmented data (`size` in our example) for each ancestor node.
2. **Deletion:**
   - After removing a node, backtrack and update the augmented data for each ancestor node.
3. **Rotations (in Balanced Trees):**
   - When performing rotations (e.g., in AVL or Red-Black Trees), update the augmented data for the nodes involved in the rotation since their subtrees have changed.
4. **Balancing:**
   - If we're using a balanced tree variant (like AVL or Red-Black Trees), ensure that the balancing operations also maintain the augmented data.

**Example:** In our Order-Statistic Tree, after any insertion or deletion, we called `updateSize(node)` to recalculate the `size` based on the sizes of its left and right subtrees.



### Conclusion

Augmented trees are powerful data structures that extend the capabilities of standard binary search trees by storing additional information within each node. This augmentation enables efficient execution of complex queries and operations that would otherwise be inefficient or cumbersome.

In C, implementing an augmented tree involves:

1. **Designing the Node Structure:** Incorporate fields for both the standard BST properties and the augmented data.
2. **Implementing Core Operations:** Modify insertion, deletion, and other tree operations to maintain the augmented data correctly.
3. **Leveraging Augmented Data:** Utilize the extra information to perform advanced queries efficiently.

---

## Balanced Trees

A **balanced tree** is a type of binary search tree (BST) where the height difference between the left and right subtrees of any node is constrained. This balance ensures that the tree remains approximately balanced, providing efficient operation times—typically **O(log n)** for search, insertion, and deletion—where **n** is the number of nodes in the tree.

In contrast, an unbalanced BST can degrade to a linked list in the worst case, leading to **O(n)** operation times.

### **Why Balance Matters**

- **Efficiency:** Balanced trees maintain a low height, ensuring that operations remain fast.
- **Predictability:** The performance of balanced trees is more consistent, avoiding worst-case scenarios.
- **Scalability:** They handle large datasets effectively, making them suitable for databases, file systems, and other applications requiring efficient data retrieval and manipulation.

### Types of Balanced Trees

Several types of balanced trees exist, each with its own balancing criteria and use cases. The most common balanced trees include:

#### **a. AVL Trees**

- **Definition:** Named after inventors Adelson-Velsky and Landis, AVL trees are height-balanced binary search trees.
- **Balancing Criteria:** For every node, the height difference between its left and right subtrees (balance factor) is at most **1**.
- **Operations:** Ensure balance through rotations during insertions and deletions.
- **Use Cases:** Situations requiring frequent lookups and insertions, such as in-memory databases.

#### **b. Red-Black Trees**

- **Definition:** A type of self-balancing BST where each node has an extra bit for denoting the color of the node, either red or black.
- Balancing Criteria:
  - Each node is either red or black.
  - The root is always black.
  - Red nodes cannot have red children (no two reds in a row).
  - Every path from a node to its descendant NULL nodes has the same number of black nodes.
- **Operations:** Maintain balance through color changes and rotations.
- **Use Cases:** Widely used in language libraries (e.g., C++ STL's `map` and `set`) due to their good balance between insertion/deletion and lookup operations.

#### **c. Other Balanced Trees**

- **Splay Trees:** Self-adjusting trees with amortized **O(log n)** operations, where recently accessed elements are quick to access again.
- **B-Trees and B+ Trees:** Balanced trees optimized for systems that read and write large blocks of data, commonly used in databases and filesystems.
- **Treaps:** Combination of binary search trees and heaps, maintaining both BST and heap properties.

### Tree rotations

- The problem is that as we insert new nodes, some paths through the tree may become very long.
- So we need to be able to shrink the long paths by moving nodes elsewhere in the tree.
- The idea is to notice that there may be many binary search trees that contain the same data and that we can transform one into another by a local modification called a **rotation**.

There are two primary types:

1. **Left Rotation (LL Rotation)**: Rotates the tree to the left when the right subtree is taller.
2. **Right Rotation (RR Rotation)**: Rotates the tree to the right when the left subtree is taller.

**Types of Rotations**

1. **Left-Left (LL) Rotation**: When a node is inserted in the left subtree of the left child.
2. **Right-Right (RR) Rotation**: When a node is inserted in the right subtree of the right child.
3. **Left-Right (LR) Rotation**: When a node is inserted in the right subtree of the left child.
4. **Right-Left (RL) Rotation**: When a node is inserted in the left subtree of the right child.

![Screenshot from 2024-12-17 20-36-35](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-17 20-36-35.png)

- If A < x < B < y < C, then both versions of this tree have the binary search tree property. By doing the rotation in one direction, we move A up and C down; in the other direction, we move A down and C up.
- So rotations can be used to transfer depth from the leftmost grandchild of a node to the right most and vice versa.
- But what if it's the middle grandchild B that's the problem?
- A single rotation as above doesn't move B up or down.
- To move B, we have to reposition it so that it's on the end of something.
- We do this by splitting B into two subtrees B1 and B2 and doing two rotations that split the two subtrees while moving both up.

![Screenshot from 2024-12-17 22-00-43](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-17 22-00-43.png)

#### Left Rotation

```css
      X
       \
        Y
       / \
     T2  T3
```

- **Identify the Imbalanced Node (`X`)**:
  - The node `X` has a right child `Y` that causes the imbalance.
- **Perform the Rotation**:
  - **Step 1**: `Y` becomes the new root of the subtree.
  - **Step 2**: `X` becomes the left child of `Y`.
  - **Step 3**: The left child of `Y` (`T2`) becomes the right child of `X`.
- **Update Pointers**:
  - Ensure that the parent of `X` now points to `Y` instead of `X`.
  - Update the subtree pointers accordingly.

**Before and After**

**Before Left Rotation:**

```css
      X
       \
        Y
       / \
     T2  T3
```

After Left Rotation:

```css
        Y
       / \
      X   T3
       \
        T2
```



#### Right Rotation

```css

	A
   / \
   B  C	
  / \
 D   E
```

We start with the pointer reference to Node A. Then we'll want a pointer to node B. After that, set A's left pointer to point to B's right child. 

```css
   A
  /
 E
```

Then change B's right pointer to Node A and we have successfully done a right rotation.

```css
B
 \
  A
```

If we rearrange the nodes ,this is what we would end up with

```css
          B
		 / \
		D   A
		   / \
		  E   C
```

#### LL Rotation

**LL Rotation** is a specific case of **Right Rotation** used in **AVL Trees** when a node becomes unbalanced due to the insertion of a node in the **left subtree of its left child**.

**Step-by-Step Process**

```css
        Z
       /
      Y
     /
    X
```

1. **Identify the Imbalance**:
   - The imbalance occurs at node `Z` because a node was inserted into the left subtree of `Z`'s left child `Y`.
2. **Perform Right Rotation on `Z`**:
   - This single right rotation restores balance.
3. **Resulting Structure**:
   - The left child `Y` becomes the new root of the subtree.
   - The original root `Z` becomes the right child of `Y`.
   - The subtree `T2` (originally `Y`'s right child) becomes `Z`'s left child.

**After LL Rotation**:

```css
      Y
     / \
    X   Z
```

#### RR Rotation

**RR Rotation** is a specific case of **Left Rotation** used in **AVL Trees** when a node becomes unbalanced due to the insertion of a node in the **right subtree of its right child**.

**Step-by-Step Process**

```css
    X
     \
      Y
       \
        Z
```

1. **Identify the Imbalance**:
   - The imbalance occurs at node `X` because a node was inserted into the right subtree of `X`'s right child `Y`.
2. **Perform Left Rotation on `X`**:
   - This single left rotation restores balance.
3. **Resulting Structure**:
   - The right child `Y` becomes the new root of the subtree.
   - The original root `X` becomes the left child of `Y`.
   - The subtree `T2` (originally `Y`'s left child) becomes `X`'s right child.

**After RR rotation**:

```css
      Y
     / \
    X   Z
```

#### When to Use Each Rotation

Understanding **when** to apply each rotation is as important as knowing **how** to perform them. Here's when each rotation is typically used:

1. **Left Rotation**:
   - Applied when a node's right subtree is heavier.
   - Commonly used in AVL Trees and Red-Black Trees to rebalance the tree after insertions or deletions that skew the tree to the right.
2. **Right Rotation**:
   - Applied when a node's left subtree is heavier.
   - Used in AVL Trees and Red-Black Trees to rebalance the tree after operations that skew the tree to the left.
3. **LL Rotation**:
   - A specific case of right rotation used in AVL Trees when the imbalance is caused by the left subtree of the left child.
   - Simple single right rotation suffices to rebalance.
4. **RR Rotation**:
   - A specific case of left rotation used in AVL Trees when the imbalance is caused by the right subtree of the right child.
   - Simple single left rotation suffices to rebalance.

#### Double Rotations

Double rotations come into play when a single rotation isn't sufficient to rebalance the tree. Specifically, they address **zig-zag** imbalance patterns where a node's child is heavy on the opposite side of its own imbalance.

**Scenarios Requiring Double Rotations:**

1. **Left-Right (LR) Rotation**: Occurs when a node has a left child that is right-heavy.
2. **Right-Left (RL) Rotation**: Occurs when a node has a right child that is left-heavy.

##### Left-Right (LR) Rotation

An **Left-Right (LR) Rotation** is a combination of a **Left Rotation** followed by a **Right Rotation**. It is used to fix an imbalance where a node's left child is right-heavy.

**Step-by-Step Process:**

**Before LR Rotation:**

```css
        Z
       /
      Y
       \
        X
```

1. **Identify the Imbalanced Node (`Z`)**:

   - Node `Z` has a left child `Y`.
   - Node `Y` has a right child `X`.
   - This creates a zig-zag pattern: `Z` → `Y` (left) → `X` (right).

2. **First Rotation - Left Rotation on `Y`**:

   - Perform a **Left Rotation** on `Y` to transform the subtree into a straight line.
   - After this rotation, `X` becomes the left child of `Z`, and `Y` becomes the left child of `X`.

   ```css
           Z
          /
         X
        /
       Y
   ```

   

3. **Second Rotation - Right Rotation on `Z`**:

   - Perform a **Right Rotation** on `Z` to elevate `X` as the new root of the subtree.
   - `Z` becomes the right child of `X`, and `Y` remains as the left child of `X`.

   ```css
         X
        / \
       Y   Z
   ```

   

##### Right-Left (RL) Rotation:

A **Right-Left (RL) Rotation** is a combination of a **Right Rotation** followed by a **Left Rotation**. It is used to fix an imbalance where a node's right child is left-heavy.

**Step-by-Step Process:**

**Before RL Rotation:**

```css
    Z
     \
      Y
     /
    X
```

1. **Identify the Imbalanced Node (`Z`)**:

   - Node `Z` has a right child `Y`.
   - Node `Y` has a left child `X`.
   - This creates a zig-zag pattern: `Z` → `Y` (right) → `X` (left).

2. **First Rotation - Right Rotation on `Y`**:

   - Perform a **Right Rotation** on `Y` to transform the subtree into a straight line.
   - After this rotation, `X` becomes the right child of `Z`, and `Y` becomes the right child of `X`.

   ```css
       Z
        \
         X
          \
           Y
   ```

   

3. **Second Rotation - Left Rotation on `Z`**:

   - Perform a **Left Rotation** on `Z` to elevate `X` as the new root of the subtree.
   - `Z` becomes the left child of `X`, and `Y` remains as the right child of `X`.

   ```css
         X
        / \
       Z   Y
   ```

##### When to Use Double Rotations

Understanding **when** to apply each double rotation is crucial for maintaining tree balance. Here's when to use each:

1. **Left-Right (LR) Rotation**:
   - **Condition**: A node `Z` has a left child `Y`, and `Y` has a right child `X`.
   - **Imbalance**: Insertion in the right subtree of the left child.
   - **Solution**: Apply an LR Rotation to rebalance.
2. **Right-Left (RL) Rotation**:
   - **Condition**: A node `Z` has a right child `Y`, and `Y` has a left child `X`.
   - **Imbalance**: Insertion in the left subtree of the right child.
   - **Solution**: Apply an RL Rotation to rebalance.

These double rotations address the zig-zag imbalance patterns that single rotations cannot fix, ensuring the tree remains balanced and operations remain efficient.

####  Why Use Tree Rotations?

Tree rotations are employed to:

- **Maintain Balance:** Prevent trees from becoming skewed (degenerate), ensuring operations remain efficient.
- **Restore Properties:** In self-balancing trees like AVL trees, red-black trees, and treaps, rotations help maintain specific invariants or properties after insertions and deletions.
- **Optimize Performance:** Balanced trees guarantee that operations such as search, insert, and delete have optimal time complexities (O(log n)).

### Implementing an AVL Tree in C

#### Program Code

`practice.h`

```C
typedef struct AVLNode
{
    int key;
    struct AVLNode *left;
    struct AVLNode *right;
    int height;
} AVLNode;

AVLNode *createNode(int key);
int height(AVLNode *node);

int getBalance(AVLNode *node);

int max(int a, int b);

AVLNode *rightRotate(AVLNode *y);

AVLNode *leftRotate(AVLNode *x);

AVLNode *leftRightRotate(AVLNode *node);

AVLNode *rightLeftRotate(AVLNode *node);

AVLNode *insert(AVLNode *node, int key);

AVLNode *minValueNode(AVLNode *node);

AVLNode *deleteNode(AVLNode *root, int key);

void inorderTraversal(AVLNode *root);

void freeTree(AVLNode *root);
```

`functions.c`

```C
AVLNode *createNode(int key)
{
    AVLNode *node = malloc(sizeof(AVLNode));
    if (!node)
    {
        printf("Memory allocation failed\n");
        exit(1);
    }

    node->key = key;
    node->left = node->right = NULL;
    node->height = 1;
    return node;
}
int height(AVLNode *node)
{
    return node ? node->height : 0;
}

// Function to get balance factor of node
int getBalance(AVLNode *node)
{
    return node ? height(node->left) - height(node->right) : 0;
}

int max(int a, int b)
{
    return a > b ? a : b;
}

AVLNode *rightRotate(AVLNode *y)
{
    AVLNode *x = y->left;
    AVLNode *T2 = x->right;

    // Perform rotation
    x->right = y;
    y->left = T2;

    // Update heights
    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;

    // Return new root
    return x;
}

AVLNode *leftRotate(AVLNode *x)
{
    AVLNode *y = x->right;
    AVLNode *T2 = y->left;

    // Perform rotation
    y->left = x;
    x->right = T2;

    // Update heights
    x->height = max(height(x->left), height(y->right)) + 1;
    y->height = max(height(y->left), height(y->right)) + 1;

    // Return new root
    return y;
}

AVLNode *leftRightRotate(AVLNode *node)
{
    node->left = leftRotate(node->left);
    return rightRotate(node);
}

AVLNode *rightLeftRotate(AVLNode *node)
{
    node->right = rightRotate(node->right);
    return leftRotate(node);
}

AVLNode *insert(AVLNode *node, int key)
{
    // 1. Perform the normal BST insertion
    if (node == NULL)
    {
        return createNode(key);
    }

    if (key < node->key)
    {
        node->left = insert(node->left, key);
    }
    else if (key > node->key)
    {
        node->right = insert(node->right, key);
    }
    else
    {
        // equal keys not allowed in BST
        return node;
    }

    // 2. Update height of this ancestor node
    node->height = 1 + max(height(node->left), height(node->right));

    // 3. Get the balance factor to check whether this node became unbalanced
    int balance = getBalance(node);

    // 4. If unbalanced, then there are 4 cases

    // Positive Balance (>1): Left-heavy.
	// Negative Balance (<-1): Right-heavy.
    // Left Left Case
    if (balance > 1 && key < node->left->key)
    {
        return rightRotate(node);
    }

    // Right Right Case
    if (balance < -1 && key > node->right->key)
    {
        return leftRotate(node);
    }

    // left right case
    if (balance > 1 && key > node->left->key)
    {
        return leftRightRotate(node);
    }
	
    
    // Right left case
    if (balance < -1 && key < node->right->key)
    {
        return rightLeftRotate(node);
    }

    // 5. Return the (unchanged) node pointer
    return node;
}

AVLNode *minValueNode(AVLNode *node)
{
    AVLNode *current = node;

    // Loop down to find the leftmost leaf
    while (current->left != NULL)
    {
        current = current->left;
    }
    return current;
}

AVLNode *deleteNode(AVLNode *root, int key)
{
    // 1. Perform standard BST delete
    if (root == NULL)
    {
        return root;
    }

    if (key < root->key)
    {
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->key)
    {
        root->right = deleteNode(root->right, key);
    }
    else
    {
        // Node with only one child or no child
        if ((root->left == NULL) || (root->right == NULL))
        {
            AVLNode *temp = root->left ? root->left : root->right;

            // No child case
            if (temp == NULL)
            {
                temp = root;
                root = NULL;
            }
            else // One child case
            {
                *root = *temp; // Copy the contents of the non-empty child
            }
            free(temp);
        }
        else
        {
            // Node with two children: Get the inorder successor (smallest in the right subtree)
            AVLNode *temp = minValueNode(root->right);
            // Copy the inorder successor's data to this node
            root->key = temp->key;
            
            // Delete the inorder successor
            root->right = deleteNode(root->right, temp->key);
        }
    }

    // If the tree had only one node then return
    if (root == NULL)
    {
        return root;
    }

    // 2. Update height of the current node
    root->height = 1 + max(height(root->left), height(root->right));

    // 3. Get the balance factor
    int balance = getBalance(root);

    // 4. If the node is unbalanced, then there are 4 cases

    // Left Left Case
    if (balance > 1 && getBalance(root->left) >= 0)
    {
        return rightRotate(root);
    }
    
    // Left Right Case
    if (balance > 1 && getBalance(root->left) < 0)
    {
        return leftRightRotate(root);
    }

    // Right Right Case
    if (balance < -1 && getBalance(root->right) <= 0)
    {
        return leftRotate(root);
    }

    // Right Left Case
    if (balance < -1 && getBalance(root->right) > 0)
    {
        return rightLeftRotate(root);
    }

    return root;
}
void inorderTraversal(AVLNode *root)
{
    if (root != NULL)
    {
        inorderTraversal(root->left);
        printf("%d ", root->key);
        inorderTraversal(root->right);
    }
}

void freeTree(AVLNode *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

`insert()`:

- **Left Left (LL) Case:**
  - **Condition:** `balance > 1` and `key < node->left->key`
  - **Explanation:** The imbalance is caused by an insertion in the **left subtree's left** side.
  - **Action:** Perform a **Right Rotation** to balance.
- **Right Right (RR) Case:**
  - **Condition:** `balance < -1` and `key > node->right->key`
  - **Explanation:** The imbalance is caused by an insertion in the **right subtree's right** side.
  - **Action:** Perform a **Left Rotation** to balance.
- **Left Right (LR) Case:**
  - **Condition:** `balance > 1` and `key > node->left->key`
  - **Explanation:** The imbalance is caused by an insertion in the **left subtree's right** side.
  - **Action:** Perform a **Left Rotation** on the left child, followed by a **Right Rotation** on the current node.
- **Right Left (RL) Case:**
  - **Condition:** `balance < -1` and `key < node->right->key`
  - **Explanation:** The imbalance is caused by an insertion in the **right subtree's left** side.
  - **Action:** Perform a **Right Rotation** on the right child, followed by a **Left Rotation** on the current node.

**`deleteNode()`**

- **Left Left (LL) Case:**
  - **Condition:** `balance > 1` and `getBalance(root->left) >= 0`
  - **Explanation:** The imbalance is due to deletion in the **left subtree's left** side.
  - **Action:** Perform a **Right Rotation**.
- **Left Right (LR) Case:**
  - **Condition:** `balance > 1` and `getBalance(root->left) < 0`
  - **Explanation:** The imbalance is due to deletion in the **left subtree's right** side.
  - **Action:** Perform a **Left Rotation** on the left child, followed by a **Right Rotation** on the current node.
- **Right Right (RR) Case:**
  - **Condition:** `balance < -1` and `getBalance(root->right) <= 0`
  - **Explanation:** The imbalance is due to deletion in the **right subtree's right** side.
  - **Action:** Perform a **Left Rotation**.
- **Right Left (RL) Case:**
  - **Condition:** `balance < -1` and `getBalance(root->right) > 0`
  - **Explanation:** The imbalance is due to deletion in the **right subtree's left** side.
  - **Action:** Perform a **Right Rotation** on the right child, followed by a **Left Rotation** on the current node.

`practice.c`

```C
int main(int argc, char *argv[])
{
    AVLNode *root = NULL;

    root = insert(root, 10);
    root = insert(root, 20);
    root = insert(root, 30);
    root = insert(root, 40);
    root = insert(root, 50);
    root = insert(root, 25);

    printf("Inorder traversal of the constructed AVL tree is\n");
    inorderTraversal(root);
    printf("\n");

    root = deleteNode(root, 40);
    printf("Inorder traversal of the AVL tree after deletion of 40\n");
    inorderTraversal(root);
    printf("\n");

    freeTree(root);
    root = NULL;
    printf("Tree has been freed\n");
    return 0;
}

```

##### **Detailed Breakdown**

1. **Initial Insertions:**

   - **Insert 10:**

     ```css
     10 (size=1)
     ```

   - **Insert 20:**

     ```css
       10 (1)
         \
         20 (1)
     ```

   - **Insert 30:**
     After balancing (Left Rotation on node 10):

     ```css
         20 (2)
        /    \
     10 (1)  30 (1)
     ```

   - **Insert 40:**

       ```css
           20 (3)
          /    \
       10 (1)  30 (2)
                 \
                 40 (1)
       ```

     

   - **Insert 50:**
     After balancing (Left Rotation on node 20):

      ```css
            30 (4)
           /     \
        20 (3)   40 (2)
       /          \
      10 (1)        50 (1)
      ```

     

   - **Insert 25:**

       ```css
             30 (5)
            /     \
         20 (4)   40 (2)
        /    \      \
       10 (1) 25 (1) 50 (1)
       ```

     

2. **Deletion Operation:**

   - **Delete Key `40`:**

     - **Node `40`** has one child: `50`.

     - **Action:** Replace node `40` with its child `50`.

     - **Resulting Tree:**

       ```css
           30 (4)
          /     \
       20 (3)   50 (1)
        /    \
       10(1) 25(1)
       ```

   - Size Updates:
     - **Node `30`:**
       - Left Subtree Size: `3` (nodes `20`, `10`, `25`)
       - Right Subtree Size: `1` (node `50`)
       - **Total Size:** `1 (itself)` + `3` + `1` = `5`
     - **Node `20`:**
       - Left Subtree Size: `1` (node `10`)
       - Right Subtree Size: `1` (node `25`)
       - **Total Size:** `1 (itself)` + `1` + `1` = `3`
     - **Nodes `10`, `25`, `50`:**
       - **Size:** `1` each

------

**Final In-Order Traversal**

```css
10, 20, 25, 30, 50
```

------

**Visualization Summary**

- **After Inserting Keys `10`, `20`, `30`, `40`, `50`, `25`:**

  The AVL tree balances itself after each insertion to maintain optimal height, ensuring that the tree remains balanced for efficient operations.

- **After Deleting Key `40`:**

  - **Replacement Strategy:**
    Since node `40` has only one child (`50`), it is directly replaced by its child.
  - **Resulting Structure:**
    The tree remains balanced with node `30` as the new root, maintaining the AVL property.

------

**Final Tree Diagram**

```css
        30 (size=5)
       /        \
  20 (size=3)    50 (size=1)
  /      \
10 (1)   25 (1)
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==20719== Memcheck, a memory error detector
==20719== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20719== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20719== Command: ./practice
==20719== 
Inorder traversal of the constructed AVL tree is
10 20 25 30 40 50 
Inorder traversal of the AVL tree after deletion of 40
10 20 25 30 50 
Tree has been freed
==20719== 
==20719== HEAP SUMMARY:
==20719==     in use at exit: 0 bytes in 0 blocks
==20719==   total heap usage: 7 allocs, 7 frees, 1,216 bytes allocated
==20719== 
==20719== All heap blocks were freed -- no leaks are possible
==20719== 
==20719== For lists of detected and suppressed errors, rerun with: -s
==20719== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

---

## 2-3 Trees

A **2-3 Tree** is a balanced search tree where every internal node has either one key and two children (**2-node**) or two keys and three children (**3-node**). All leaves are at the same depth, ensuring the tree remains balanced. This balance guarantees that operations like search, insertion, and deletion can be performed in logarithmic time relative to the number of elements in the tree.

2-3 Trees are a subset of **B-Trees** of order 3 and are **foundational** in understanding more complex tree structures like Red-Black Trees.



### Properties of 2-3 Trees

Understanding the fundamental properties of 2-3 Trees is crucial for their implementation:

1. **Node Types:**
   - **2-Node:** Contains one key and two children.
   - **3-Node:** Contains two keys and three children.
2. **Balanced Tree:**
   - All leaf nodes are at the same depth, ensuring the tree is height-balanced.
3. **Ordering:**
   - For a **2-node** with key `k`, all keys in the left subtree are less than `k`, and all keys in the right subtree are greater than `k`.
   - For a  3-node with keys  `k1` and  `k2` ( `k1 < k2`):
     - All keys in the leftmost subtree are less than `k1`.
     - Keys in the middle subtree are between `k1` and `k2`. `k1` < values < `k2`
     - Keys in the rightmost subtree are greater than `k2`.
4. **No Duplicate Keys:**
   - Each key in the tree is unique.

### Advantages of 2-3 Trees

2-3 Trees offer several benefits:

- **Balanced Structure:** Guarantees `O(log n)` time complexity for search, insertion, and deletion.
- **Simpler Implementation Compared to B-Trees:** Easier to understand and implement for educational purposes.
- **Consistent Performance:** Avoids the worst-case scenarios of unbalanced trees, such as linked lists.

### **Visual Representation:**

Let's say we start with an empty 2-3 tree and insert the following sequence of keys:
`10, 20, 5, 15, 30`

- **Insert 10:**
  Initially empty:

  ```css
  (Empty)
  ```

  After inserting 10:

  ```css
  [10]
  ```

  This is just a single node (a 2-node with one key).

- **Insert 20:**
  Insert 20 into the leaf that currently has key 10. Now that leaf will have two keys: 10 and 20, forming a 3-node.

  ```css
  [10 | 20]
  ```

  The tree is still just one node, but now it’s a 3-node because it has two keys.

- **Insert 5:**
  We try to insert 5 into the existing 3-node `[10 | 20]`. Inserting 5 will give the node three keys: 5, 10, and 20. A 2-3 node can’t hold three keys, so we must split.

  Temporary:

  ```css
  [5 | 10 | 20]
  ```

  Upon splitting, the middle key (10) moves up to become the root. The left key (5) becomes the left child, and the right key (20) becomes the right child:

  ```css
       [10]
      /    \
   [5]    [20]
  ```

  Now we have a root with one key (10) and two children: [5] and [20]. Each child is a leaf node with one key.

- **Insert 15:**
  The keys are currently arranged as:

  ```css
       [10]
      /    \
   [5]    [20]
  ```

  We need to insert 15. Compare 15 with 10: `15 > 10`, so we go to the right child `[20]`. Insert 15 there. The `[20]` leaf becomes `[15 | 20]`:

  ```css
       [10]
      /    \
   [5]    [15 | 20]
  ```

  This is valid. The structure remains balanced and no further splits are needed.

- **Insert 30:**
  Insert 30 into the tree:

  Compare with 10: `30 > 10`, go right. The right child is `[15 | 20]`. Insert 30 there:
  Now we have `[15 | 20 | 30]`, which is a temporary 3-key leaf. We must split it.

  The middle key (20) goes up to the parent. The parent currently is `[10]`, which has only one key. After adding the middle key (20), we get a parent with `[10 | 20]`. The left part `[15]` remains the left child of the new subtree, and the right part `[30]` becomes a new child:

  The tree restructures to:

  ```css
           [10 | 20]
         /     |     \
      [5]    [15]    [30]
  ```

  Now we have a well-balanced 2-3 tree. All leaves (5, 15, 30) are at the same level.

**In summary, the tree after inserting 10, 20, 5, 15, 30 is:**

```css
       [10 | 20]
      /    |     \
   [5]   [15]   [30]
```

Each leaf is at the same level, and internal nodes have either 2 or 3 children.

### 2-3 Tree Node Structure in C

To represent a 2-3 Tree in C, we need to define a node structure that can accommodate both 2-nodes and 3-nodes. Each node will contain:

- **Keys:** One or two keys.
- **Children:** Pointers to two or three children.
- **Parent Pointer:** (Optional) To facilitate upward traversal, especially useful during insertion and deletion.

#### **Defining the Node Structure**

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

// Maximum number of keys and children in a node
#define MAX_KEYS 2
#define MAX_CHILDREN 3

typedef struct Node {
    int keys[MAX_KEYS];              // Array to store keys
    struct Node* children[MAX_CHILDREN]; // Pointers to child nodes
    int numKeys;                     // Current number of keys
    bool isLeaf;                     // Flag to check if node is a leaf
} Node;

```

**Explanation:**

- `keys[MAX_KEYS]`: Stores one or two keys.
- `children[MAX_CHILDREN]`: Pointers to up to three children.
- `numKeys`: Tracks whether the node is a 2-node (`numKeys = 1`) or a 3-node (`numKeys = 2`).
- `isLeaf`: Indicates whether the node is a leaf node.

### Key Operations

Implementing a 2-3 Tree involves several core operations: searching, inserting, and deleting keys. 

#### Understanding Tree Terminology

Before diving into the specifics of leaves in a 2-3 Tree, it's essential to familiarize ourselves with some fundamental tree terminology:

- **Node**: An individual element of a tree containing data.
- **Root**: The topmost node of a tree, with no parent.
- **Internal Node**: A node that has one or more children.
- **Leaf (or External Node)**: A node with no children.
- **Subtree**: A tree entirely contained within another tree, rooted at a specific node.
- **Depth/Height**: The level of a node within the tree; the root has depth 0, its children depth 1, and so on.

#### 5.1. Searching

The search operation in a 2-3 Tree is straightforward due to its balanced and ordered structure.

**Algorithm:**

1. Start at the root node.
2. Compare the target key with the keys in the current node.
3. If the key matches, return success.
4. If the key is less than the first key, move to the left child.
5. If the key is greater than the first key but (in a 3-node) less than the second key, move to the middle child.
6. If the key is greater than all keys, move to the right child.
7. Repeat until the key is found or a leaf is reached.

#### C Implementation:

```C
// Function to search a key in the 2-3 Tree
bool search(Node* root, int key) {
    if (root == NULL)
        return false;

    int i = 0;

    // Find the first key greater than or equal to key
    while (i < root->numKeys && key > root->keys[i])
        i++;

    // If the found key is equal to key, return true
    if (i < root->numKeys && root->keys[i] == key)
        return true;

    // If the node is a leaf, the key is not present
    // In any tree data structure, a leaf (also known as an external node) is a node that does not have any children.
    if (root->isLeaf)
        return false;

    // Recurse on the appropriate child
    return search(root->children[i], key);
}

```



#### Insertion

Inserting a key into a 2-3 Tree involves ensuring that the tree remains balanced and maintains its properties after the insertion. The primary challenge is handling node splits when a node becomes overfull (i.e., exceeds the maximum number of keys).

**Algorithm:**

1. **Find the Appropriate Leaf:**
   - Navigate the tree to locate the correct leaf node where the new key should be inserted.
2. **Insert the Key:**
   - If the leaf node has space (`numKeys < 2`), insert the key in order.
   - If the leaf node is full (`numKeys == 2`), split the node:
     - Create two new nodes and distribute the keys.
     - Promote the middle key to the parent.
     - If the parent is also full, recursively split the parent.
3. **Handle Root Splits:**
   - If the root splits, create a new root with the promoted key and two children.

**C Implementation:**

Implementing insertion requires handling node splits and promoting keys.

```C
// Function to create a new node
Node* createNode(int key, bool isLeaf) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if(!newNode){
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->keys[0] = key;
    newNode->numKeys = 1;
    newNode->isLeaf = isLeaf;
    for (int i = 0; i < MAX_CHILDREN; i++)
        newNode->children[i] = NULL;
    return newNode;
}

// Function to insert a key into a non-full node
void insertNonFull(Node* node, int key) {
    int i = node->numKeys - 1;

    if (node->isLeaf) {
        // Shift keys to make room for the new key
        while (i >= 0 && key < node->keys[i]) {
            node->keys[i + 1] = node->keys[i];
            i--;
        }
        node->keys[i + 1] = key;
        node->numKeys += 1;
    } else {
        // Find the child which is going to have the new key
        while (i >= 0 && key < node->keys[i])
            i--;

        i++;

        // Check if the found child is full
        if (node->children[i]->numKeys == MAX_KEYS) {
            // Split the child
            // Assuming a function splitChild exists
            // splitChild(node, i, node->children[i]);

            // For simplicity, skipping the implementation details
            // After splitting, decide which of the two children to descend
            // if (key > node->keys[i])
            //     i++;
        }

        insertNonFull(node->children[i], key);
    }
}

// Function to split the child of a node
void splitChild(Node* parent, int index, Node* child) {
    // Create a new node to store (t-1) keys of child
    Node* newChild = createNode(child->keys[1], child->isLeaf);
    newChild->numKeys = 1;

    // If child is not a leaf, copy the last t children to newChild
    if (!child->isLeaf) {
        for (int j = 0; j < MAX_CHILDREN / 2; j++)
            newChild->children[j] = child->children[j + 2];
    }

    // Reduce the number of keys in child
    child->numKeys = 1;

    // Create space for the new child
    for (int j = parent->numKeys; j >= index + 1; j--)
        parent->children[j + 1] = parent->children[j];

    // Link the new child to the parent
    parent->children[index + 1] = newChild;

    // Move the middle key of child up to the parent
    for (int j = parent->numKeys - 1; j >= index; j--)
        parent->keys[j + 1] = parent->keys[j];

    parent->keys[index] = child->keys[1];
    parent->numKeys += 1;
}

// Function to insert a key into the 2-3 Tree
void insert(Node** rootRef, int key) {
    Node* root = *rootRef;

    // If the tree is empty
    if (root == NULL) {
        *rootRef = createNode(key, true);
        return;
    }

    // If root is full, tree grows in height
    if (root->numKeys == MAX_KEYS) {
        Node* newRoot = createNode(0, false); // Temporary key
        newRoot->children[0] = root;
        splitChild(newRoot, 0, root);

        // Now insert the key into the appropriate child
        int i = 0;
        if (newRoot->keys[0] < key)
            i++;
        // Assuming the splitChild function updated the keys correctly
        // insertNonFull(newRoot->children[i], key);

        // For simplicity, calling insertNonFull
        insertNonFull(newRoot->children[i], key);

        *rootRef = newRoot;
    } else {
        insertNonFull(root, key);
    }
}

```

**Notes:**

- The `splitChild` function handles splitting a full child node and promoting the middle key to the parent.
- The `insert` function checks if the root is full and splits it if necessary, ensuring that the tree's height increases only when required.
- The `insertNonFull` function inserts a key into a node that is guaranteed not to be full.

#### Deletion

Deletion in 2-3 Trees is more involved as it may require merging nodes or redistributing keys to maintain the tree's properties. Given its complexity, implementing deletion is beyond the scope of this introductory guide. However, understanding that deletion involves ensuring that nodes do not become underfull (i.e., have fewer keys than required) is essential.

For comprehensive deletion implementation, consider studying B-Trees or more advanced balanced trees like Red-Black Trees, which offer similar balanced properties with more straightforward deletion algorithms.

---

## Red-Black Tree

A **Red-Black Tree** is a type of **self-balancing binary search tree (BST)**. It ensures that the tree remains approximately balanced during insertions and deletions, guaranteeing efficient operations. 

- A red-black tree is a 2-3-4 tree (i.e all nodes have 2,3, or 4 children and 1, 2, or 3 internal keys) where each node is represented by a little binary tree with a black root and zero, one, or two red extender nodes as follows:
- The invariant for a red-black tree is that
  - No two red nodes are adjacent.
  - Every path contains the same number of black nodes.
- From the variant, it follows that every path has between k and 2k nodes, where k is the black-height, the common number of black nodes on each path.
- From this, we can prove that the height of the tree is `O(log n)`.
- Searching in a red-black tree is identical to searching in any other binary search tree.
  - We simply ignore the color bit on each node.
  - So search takes `O(log n)` time.
- For insertions, we use the standard binary search tree insertion algorithm, and insert the new node as a red node.
  - This may violate the first part of the invariant (it doesn't violate the second because it doesn't change the number of black nodes on any path).
  - In this case, we need to fix up the constraint by recoloring nodes and possibly performing a single or double rotation.
- Which operations we need to do depend on the color of the new node’s uncle.
- If the uncle is red, we can recolor the node’s parent, uncle, and grandparent and get rid of the double-red edge between the new node and its parent without
  changing the number of black nodes on any path. 
- In this case, the grandparent becomes red, which may create a new double-red edge which must be fixed
  recursively. 
- Thus up to O(log n) such recolorings may occur at a total cost of O(log n).
- If the uncle is black (which includes the case where the uncle is a null pointer), a rotation (possibly a double rotation) and recoloring is necessary. 
- In this case,  the new grandparent is always black, so there are no more double-red edges. 
- So at most two rotations occur after any insertion.
- Deletion is more complicated but can also be done in O(log n) recolorings and O(1) (in this case up to 3) rotations. 
- Because deletion is simpler in red-black
  trees than in AVL trees, and because operations on red-black trees tend to have slightly smaller constants than corresponding operation on AVL trees, red-black
  trees are more often used than AVL trees in practice.

![Screenshot from 2024-12-20 20-59-20](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-20 20-59-20.png)

![Screenshot from 2024-12-20 20-59-33](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-20 20-59-33.png)

### Red-Black Tree Properties

To maintain balance, Red-Black Trees adhere to the following properties:

1. **Node Color:**
   - Each node is either **red** or **black**.
2. **Root Property:**
   - The **root** of the tree is always **black**.
3. **Leaf Property:**
   - All **leaves** (NIL or NULL nodes) are considered **black**.
4. **Red Property:**
   - If a node is **red**, then both its **children** are **black**.
   - This ensures that **no two red nodes appear consecutively**.
5. **Black-Height Property:**
   - Every path from a given node to any of its descendant **NIL** nodes contains the **same number of black nodes**.
   - This uniformity ensures the tree remains balanced.

### How Red-Black Trees Work

Red-Black Trees use these properties to perform efficient insertions, deletions, and searches while maintaining balance. The key operations involve **rotations** and **recoloring** to fix any violations of the properties after modifications.

### Red-Black Tree Implementation in C

Implementing a Red-Black Tree in C is more involved than standard BSTs due to the need to maintain additional properties. Below is a simplified implementation focusing on **insertion** and **searching**. **Deletion** is more complex.

#### Program Code

`practice.h`

```C
typedef enum
{
    RED,
    BLACK
} Color;

typedef struct RBNode
{
    int data;
    Color color;
    struct RBNode *left, *right, *parent;
} RBNode;

RBNode *createNode(int data);
void leftRotate(RBNode **root, RBNode *x);
void rightRotate(RBNode **root, RBNode *y);

void fixViolation(RBNode **root, RBNode *z);

void insertRB(RBNode **root, int data);

RBNode *searchRB(RBNode *root, int data);

void inorderTraversal(RBNode *root);

void printInOrder(RBNode *root);

void freeTree(RBNode *root);
```

`functions.c`

```C
// Function to create a new Red-Black Tree node
RBNode* createNode(int data) {
    RBNode* newNode = (RBNode*)malloc(sizeof(RBNode));
    if (!newNode) {
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->color = RED; // New nodes are initially red
    newNode->left = newNode->right = newNode->parent = NULL;
    return newNode;
}

// Function to perform a left rotation
void leftRotate(RBNode** root, RBNode* x) {
    RBNode* y = x->right;
    x->right = y->left;
    if (y->left != NULL)
        y->left->parent = x;
    
    y->parent = x->parent;
    if (x->parent == NULL)
        *root = y;
    else if (x == x->parent->left)
        x->parent->left = y;
    else
        x->parent->right = y;
    
    y->left = x;
    x->parent = y;
}

// Function to perform a right rotation
void rightRotate(RBNode** root, RBNode* y) {
    RBNode* x = y->left;
    y->left = x->right;
    if (x->right != NULL)
        x->right->parent = y;
    
    x->parent = y->parent;
    if (y->parent == NULL)
        *root = x;
    else if (y == y->parent->left)
        y->parent->left = x;
    else
        y->parent->right = x;
    
    x->right = y;
    y->parent = x;
}

// Function to fix violations after insertion
void fixViolation(RBNode** root, RBNode* z) {
    while (z->parent != NULL && z->parent->color == RED) {
        RBNode* parent = z->parent;
        RBNode* grandparent = z->parent->parent;
        
        // Case 1: Parent is left child of grandparent
        if (parent == grandparent->left) {
            RBNode* uncle = grandparent->right;
            
            // Case 1a: Uncle is red
            if (uncle != NULL && uncle->color == RED) {
                grandparent->color = RED;
                parent->color = BLACK;
                uncle->color = BLACK;
                z = grandparent;
            }
            // Case 1b: Uncle is black or NULL
            else {
                if (z == parent->right) {
                    leftRotate(root, parent);
                    z = parent;
                    parent = z->parent;
                }
                rightRotate(root, grandparent);
                // Swap colors
                parent->color = BLACK;
                grandparent->color = RED;
            }
        }
        // Case 2: Parent is right child of grandparent
        else {
            RBNode* uncle = grandparent->left;
            
            // Case 2a: Uncle is red
            if (uncle != NULL && uncle->color == RED) {
                grandparent->color = RED;
                parent->color = BLACK;
                uncle->color = BLACK;
                z = grandparent;
            }
            // Case 2b: Uncle is black or NULL
            else {
                if (z == parent->left) {
                    rightRotate(root, parent);
                    z = parent;
                    parent = z->parent;
                }
                leftRotate(root, grandparent);
                // Swap colors
                parent->color = BLACK;
                grandparent->color = RED;
            }
        }
    }
    (*root)->color = BLACK; // Ensure root is always black
}

// Function to insert a node into the Red-Black Tree
void insertRB(RBNode** root, int data) {
    RBNode* z = createNode(data);
    RBNode* y = NULL;
    RBNode* x = *root;
    
    // Standard BST insertion
    while (x != NULL) {
        y = x;
        if (z->data < x->data)
            x = x->left;
        else if (z->data > x->data)
            x = x->right;
        else {
            printf("Duplicate value %d not inserted.\n", data);
            free(z);
            return;
        }
    }
    
    z->parent = y;
    if (y == NULL)
        *root = z; // Tree was empty
    else if (z->data < y->data)
        y->left = z;
    else
        y->right = z;
    
    // Fix Red-Black Tree properties
    fixViolation(root, z);
}

// Function to search for a value in the Red-Black Tree
RBNode* searchRB(RBNode* root, int data) {
    while (root != NULL) {
        if (data < root->data)
            root = root->left;
        else if (data > root->data)
            root = root->right;
        else
            return root; // Found
    }
    return NULL; // Not found
}

// Function for in-order traversal to display the tree
void inorderTraversal(RBNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d(%s) ", root->data, root->color == RED ? "R" : "B");
        inorderTraversal(root->right);
    }
}

// Function to print the Red-Black Tree in in-order
void printInOrder(RBNode* root) {
    printf("In-order Traversal: ");
    inorderTraversal(root);
    printf("\n");
}

void freeTree(RBNode *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

**Code Breakdown:**

1. **Enumerations and Structures:**
   - `Color`: Enumerated type to represent node colors (`RED` or `BLACK`).
   - `RBNode`: Structure representing a node in the Red-Black Tree, containing:
     - `data`: The integer value stored.
     - `color`: The color of the node.
     - `left`, `right`, `parent`: Pointers to child nodes and parent node.
2. **Node Creation:**
   - `createNode`: Allocates memory for a new node, initializes its data, sets color to `RED`, and initializes child and parent pointers to `NULL`.
3. **Rotations:**
   - Left Rotate (`leftRotate`):
     - Rotates the subtree to the left, adjusting pointers accordingly.
   - Right Rotate (`rightRotate`):
     - Rotates the subtree to the right, adjusting pointers accordingly.
4. **Fixing Violations (`fixViolation`):**
   - After insertion, the tree might violate Red-Black properties.
   - The function corrects these violations by performing rotations and recoloring.
   - It handles cases based on the color of the node's uncle and the relative positions of the nodes.
5. **Insertion (`insertRB`):**
   - Inserts a new node into the tree following standard BST insertion.
   - After insertion, calls `fixViolation` to maintain Red-Black properties.
   - Prevents insertion of duplicate values.
6. **Searching (`searchRB`):**
   - Standard BST search for a given value.
   - Returns the node if found; otherwise, returns `NULL`.
7. **Traversal (`inorderTraversal` & `printInOrder`):**
   - Performs an in-order traversal of the tree.
   - Displays each node's data along with its color (`R` for Red, `B` for Black).

`practice.c`

```C
int main()
{
    RBNode *root = NULL;

    int values[] = {10, 20, 30, 15, 25, 5, 1};
    int n = sizeof(values) / sizeof(values[0]);

    printf("Inserting values: ");
    for (int i = 0; i < n; i++)
    {
        printf("%d ", values[i]);
        insertRB(&root, values[i]);
    }
    printf("\n");
	
    
    // display in-order traversal
    printInOrder(root);

    // Search for a value
    int key = 15;
    RBNode *found = searchRB(root, key);
    if (found)
    {
        printf("Value %d found in the Red-Black Tree. Color: %s\n", key, found->color == RED ? "Red" : "Black");
    }
    else
    {
        printf("Value %d not found in the Red-Black Tree.\n", key);
    }

    // Search for a non-existing value
    key = 100;
    found = searchRB(root, key);

    if (found)
    {
        printf("Value %d found in the Red-Black Tree. Color: %s\n", key, found->color == RED ? "Red" : "Black");
    }
    else
    {
        printf("Value %d not found in the Red-Black Tree.\n", key);
    }

    freeTree(root);
    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==39468== Memcheck, a memory error detector
==39468== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==39468== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==39468== Command: ./practice
==39468== 
Inserting values: 10 20 30 15 25 5 1 
In-order Traversal: 1(R) 5(B) 10(R) 15(B) 20(B) 25(R) 30(B) 
Value 15 found in the Red-Black Tree. Color: Black
Value 100 not found in the Red-Black Tree.
==39468== 
==39468== HEAP SUMMARY:
==39468==     in use at exit: 0 bytes in 0 blocks
==39468==   total heap usage: 8 allocs, 8 frees, 1,248 bytes allocated
==39468== 
==39468== All heap blocks were freed -- no leaks are possible
==39468== 
==39468== For lists of detected and suppressed errors, rerun with: -s
==39468== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
chan@CMA:~/C_Programming/practice$ 
```



#### Visual Representation

After inserting the values `{10, 20, 30, 15, 25, 5, 1}`, the Red-Black Tree maintains its properties. Here's a possible representation:

```css
           15(B)
         /       \
      5(R)      25(R)
     /   \       /    \
   1(B) 10(B) 20(B) 30(B)
```

This structure satisfies all Red-Black Tree properties:

1. **Root is Black.**
2. **Red nodes have Black children.**
3. **All paths from root to leaves have the same number of Black nodes.**

### Key Takeaways

**Red-Black Trees:**

- A self-balancing binary search tree ensuring efficient operations.
- Maintains balance through node coloring and rotations.
- Guarantees **O(log n)** time complexity for insertion, deletion, and searching.
- Widely used in standard libraries and applications requiring ordered data structures.

---

## B-Trees
