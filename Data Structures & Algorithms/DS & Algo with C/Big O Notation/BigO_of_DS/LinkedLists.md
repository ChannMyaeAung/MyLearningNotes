### **Linked List**

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
