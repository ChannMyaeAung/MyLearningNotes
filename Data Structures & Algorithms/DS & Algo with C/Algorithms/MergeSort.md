### Merge Sort

**Merge Sort** is a **divide-and-conquer** algorithm that efficiently sorts large datasets. 

- It divides the unsorted list into smaller sublists until each sublist contains a single element. 
- Then, it repeatedly merges these sublists to produce new sorted sublists until there is only one sorted list remaining—the sorted array.

#### Merge Sort Algorithm Steps

1. **Divide:** Split the array into two halves.
2. **Conquer:** Recursively apply Merge Sort to both halves.
3. **Combine:** Merge the two sorted halves into a single sorted array.

#### Visualization

```css
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