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

```css
  1
 /  \
4    3
```

  ```css
- **Largest is 4; swap 1 and 4:**
  ```

```css
  4
 /  \
1    3
```

  ```css
- **Array After Iteration 2:** `[4, 1, 3, 5, 10]`

**Iteration 3:**

- **Swap 4 and 3:**

  ```

~~~css
    3
   /  \
  1    4
```
~~~

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