## Splay Trees

A **splay tree** is a **self-adjusting binary search tree** that moves frequently accessed elements closer to the root through **splaying** operations. The basic idea of a splay operation is that we move some particular node to the root of the tree, using a sequence of **rotations** that tends to fix the balance of the tree if the node starts out very deep.

- Splaying a node to the root involves performing rotations two layers at a time.
- There are two main cases, depending on whether the node's parent and grandparent are in the same direction (zig-zig) or in opposite directions (zig-zag), plus a third case when the node is only one step away from the root.
- At each step, we pick one of these cases and apply it, until the target node reaches the root of the tree.

**Modern Usage:**

- Caching and Memory Management:
  - Frequently accessed elements are moved to the top, making them faster to retrieve.
  - Useful for applications with **temporal locality** (recently accessed elements are likely to be accessed again).
- String Processing Algorithms:
  - Used in data compression algorithms like **LZ77** due to their ability to prioritize frequently accessed patterns.
- Amortized Data Structures:
  - Ideal for scenarios where **average-case performance** is prioritized over **worst-case performance** (e.g., network routing tables).

### Key Operations

- **Searching**: If the key is found, splay the node containing the key. If the key is not found, splay the parent of the leaf position where we tried to find the key
- **Insertion**: When inserting a new node, that node gets splayed to the root.
- **Deletion**: When deleting a node, that node's parent node is splayed to the root.

**Limitations:**

- Their **worst-case time complexity** is **O(n)**, which can be problematic for applications needing strict **logarithmic bounds**.
- Red-Black trees and AVL trees offer **better guarantees** for balanced structures, making them more suitable for general-purpose use.