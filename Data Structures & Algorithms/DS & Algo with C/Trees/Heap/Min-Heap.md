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