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