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

  ````C
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
  ````
  
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