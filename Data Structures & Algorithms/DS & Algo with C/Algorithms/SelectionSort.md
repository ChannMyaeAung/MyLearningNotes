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