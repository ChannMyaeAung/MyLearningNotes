## B-Trees

A **B-Tree** is a self-balancing tree data structure that maintains sorted data and allows searches, sequential access, insertions, and deletions in logarithmic time. Unlike binary search trees, B-Trees can have multiple children per node, which helps in reducing the tree's height and optimizing disk reads.

Useful Animation on B-Trees: https://youtu.be/K1a2Bk8NrYQ?si=6e0rIKTN7JCAYfiU

### Key Properties of B-Trees

1. **Balanced**: All leaf nodes are at the same depth.

2. **Order**: A B-Tree of order `m` can have a maximum of `m` children.

3. **Nodes:**

   - Each node contains a certain number of keys.
   - Keys within a node are stored in a sorted manner.
   - The number of keys in a node is between ⎡m/2⎤ - 1 and m - 1.
     - Let's say m is 4, so the minimum key a node must have is [4/2] - 1 = 1 and the maximum key a node can have is 4 - 1 = 3. Hence, the number of keys a node can have is between 1 and 3.

4. **Children:**

   - A non-leaf node with `k` keys has `k + 1` children.

   - Children pointers are arranged such that all keys in the `i`-th child are between the `i-1`-th and `i`-th key in the node.

     - Let's say we have a node with 3 keys which will have 4 children (leaves).

     ```css
     			[10, 20, 30]
     
     		/    |        |         \
       [x < 10] [10<x<20] [20<x<30]  [x > 30]
     ```

     - The left most child will have value less than the left most value of its parent.
     - The middle left child will have value between the left most value and the middle value of its parent.
     - The middle right child will have value between the middle value and the right most value of its parent.
     - The right most child will have value greater than the right most value of its parent.

- A B-tree is a generalization of a 2–3 tree where each node has between `M/2`and `M − 1` children, where `M` is some large constant chosen so that a node (including up to M − 1 pointers and up to M − 2 keys) will just fit inside a single block.

- When a node would otherwise end up with `M` children, it splits into two nodes with M/2 children each, and **moves its middle key up into its parent**. 

  - ```css
    [10, 20, 30, 40, 50]
    ```

  - Let's say the maximum key a node can have is 4.

  - ```css
    	[30]
     /       \
    [10. 20]  [40, 50]
    ```

  - The middle key will becomes the root. If there is a root already there, the value will be merge to that root.

- As in 2–3 trees this may **eventually require the root to split and a new root to be created**; in practice, M is often large enough that a small fixed height is enough to span as much data as the storage system is capable of holding.

- Searches in B-trees require looking through log<sub>M</sub> n nodes, at a cost of O(M) time per node. If M is a constant the total time is asymptotically O(log n). 

- But the reason for using B-trees is that the O(M) cost of reading a block is trivial compare to the much larger constant time to find the block on the disk; and so it is better to minimize the number of disk accesses (by making M large) than reduce the CPU time.

### Advantages of B-Trees

- **Minimized Disk Access**: Designed to work well on systems that read and write large blocks of data due to **wide nodes** with multiple keys.
- **Efficient Search, Insert, Delete**: Operations are performed in O(log n) time.
- **Dynamic Structure**: Grows and shrinks as needed, maintaining balance.
- **Scalable and Balanced:**
  - **B-trees** are **balanced** by design, ensuring a **logarithmic height** even as data grows significantly.
  - They handle **large amounts of data** efficiently, making them ideal for **databases** and **file systems**.
- **Space Efficiency**: Stores more data per node, reducing overhead.
- **Sequential Access**: Supports **range queries** and ordered traversal more efficiently than hash tables.

### Applications of B-Trees

- **Databases**: Indexing large datasets to allow quick retrieval.

  - Most **Relational Databases** (e.g., **MySQL**, **PostgreSQL**) and **NoSQL databases** (e.g., **MongoDB**) use **B-trees** or **B+ trees** for indexing and query optimization.

- **File Systems**: Managing directories and files efficiently.

  - **NTFS (Windows)** and **HFS+ (MacOS)** use **B-trees** for organizing directories and files.

    Modern file systems like **ZFS** also rely on **B-trees** for metadata indexing.

- **Operating Systems**: Managing memory and storage.

  - Operating systems use **B-trees** in virtual memory and paging systems to organize **page tables** efficiently.

- **Search Engines**:

  - **B-trees** and **B+ trees** are used to implement **inverted indexes** for fast retrieval of documents matching search queries.

- **Key-Value Stores**:

  - Distributed systems like **LevelDB** and **RocksDB** utilize **B-trees** or variants to store data persistently.