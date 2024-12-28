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

