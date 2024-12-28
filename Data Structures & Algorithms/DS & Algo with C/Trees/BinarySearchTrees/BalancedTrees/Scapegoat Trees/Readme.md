## Scapegoat Trees

A **scapegoat tree** is a **self-balancing binary search tree** that maintains balance through **partial rebuilding** when imbalances exceed a certain threshold.

- The idea of a scapegoat tree is that if we ever find ourselves doing an insert at the end of a path that is too long, we can find some subtree rooted at a node along this path that is particularly imbalanced and rebalance it all at once at a cost of O(k) where k is the size of the subtree.
- Unlike splay trees, scapegoat trees do not require modifying the tree during a search, and unlike AVL trees, scapegoat trees do not require tracking any information in the nodes (although they do require tracking the total size of the tree and, to allow for rebalancing after many deletes, the maximum size of the tree since the last time the entire tree was rebalanced).
- Unfortunately, scapegoat trees are not very fast, so one is probably better off with an AVL tree.

**Modern Usage:**

- Dynamic Sets with Strict Space Requirements:
  - Scapegoat trees don’t require **extra memory** (like parent pointers), making them suitable for **memory-constrained environments**.
- Real-time Systems:
  - They guarantee **logarithmic height** without requiring complex rotations, making them useful for predictable performance.
- Database Indexing:
  - Sometimes used in databases where insertions and deletions occur frequently, but performance needs to remain stable.

**Limitations:**

- The **rebuilding process** during rebalancing can cause **performance spikes**, which may not be ideal for systems requiring **consistent operation times**.
- Red-Black trees and AVL trees are **simpler to implement** and have **stable performance**, so they are preferred in most scenarios.