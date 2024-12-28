## 2-3 Trees

A **2-3 Tree** is a balanced search tree where every internal node has either one key and two children (**2-node**) or two keys and three children (**3-node**). All leaves are at the same depth, ensuring the tree remains balanced. This balance guarantees that operations like search, insertion, and deletion can be performed in logarithmic time relative to the number of elements in the tree.

2-3 Trees are a subset of **B-Trees** of order 3 and are **foundational** in understanding more complex tree structures like Red-Black Trees.



### Properties of 2-3 Trees

Understanding the fundamental properties of 2-3 Trees is crucial for their implementation:

1. **Node Types:**
   - **2-Node:** Contains one key and two children.
   - **3-Node:** Contains two keys and three children.
2. **Balanced Tree:**
   - All leaf nodes are at the same depth, ensuring the tree is height-balanced.
3. **Ordering:**
   - For a **2-node** with key `k`, all keys in the left subtree are less than `k`, and all keys in the right subtree are greater than `k`.
   - For a  3-node with keys  `k1` and  `k2` ( `k1 < k2`):
     - All keys in the leftmost subtree are less than `k1`.
     - Keys in the middle subtree are between `k1` and `k2`. `k1` < values < `k2`
     - Keys in the rightmost subtree are greater than `k2`.
4. **No Duplicate Keys:**
   - Each key in the tree is unique.

### Advantages of 2-3 Trees

2-3 Trees offer several benefits:

- **Balanced Structure:** Guarantees `O(log n)` time complexity for search, insertion, and deletion.
- **Simpler Implementation Compared to B-Trees:** Easier to understand and implement for educational purposes.
- **Consistent Performance:** Avoids the worst-case scenarios of unbalanced trees, such as linked lists.

### **Visual Representation:**

Let's say we start with an empty 2-3 tree and insert the following sequence of keys:
`10, 20, 5, 15, 30`

- **Insert 10:**
  Initially empty:

  ```css
  (Empty)
  ```

  After inserting 10:

  ```css
  [10]
  ```

  This is just a single node (a 2-node with one key).

- **Insert 20:**
  Insert 20 into the leaf that currently has key 10. Now that leaf will have two keys: 10 and 20, forming a 3-node.

  ```css
  [10 | 20]
  ```

  The tree is still just one node, but now it’s a 3-node because it has two keys.

- **Insert 5:**
  We try to insert 5 into the existing 3-node `[10 | 20]`. Inserting 5 will give the node three keys: 5, 10, and 20. A 2-3 node can’t hold three keys, so we must split.

  Temporary:

  ```css
  [5 | 10 | 20]
  ```

  Upon splitting, the middle key (10) moves up to become the root. The left key (5) becomes the left child, and the right key (20) becomes the right child:

  ```css
       [10]
      /    \
   [5]    [20]
  ```

  Now we have a root with one key (10) and two children: [5] and [20]. Each child is a leaf node with one key.

- **Insert 15:**
  The keys are currently arranged as:

  ```css
       [10]
      /    \
   [5]    [20]
  ```

  We need to insert 15. Compare 15 with 10: `15 > 10`, so we go to the right child `[20]`. Insert 15 there. The `[20]` leaf becomes `[15 | 20]`:

  ```css
       [10]
      /    \
   [5]    [15 | 20]
  ```

  This is valid. The structure remains balanced and no further splits are needed.

- **Insert 30:**
  Insert 30 into the tree:

  Compare with 10: `30 > 10`, go right. The right child is `[15 | 20]`. Insert 30 there:
  Now we have `[15 | 20 | 30]`, which is a temporary 3-key leaf. We must split it.

  The middle key (20) goes up to the parent. The parent currently is `[10]`, which has only one key. After adding the middle key (20), we get a parent with `[10 | 20]`. The left part `[15]` remains the left child of the new subtree, and the right part `[30]` becomes a new child:

  The tree restructures to:

  ```css
           [10 | 20]
         /     |     \
      [5]    [15]    [30]
  ```

  Now we have a well-balanced 2-3 tree. All leaves (5, 15, 30) are at the same level.

**In summary, the tree after inserting 10, 20, 5, 15, 30 is:**

```css
       [10 | 20]
      /    |     \
   [5]   [15]   [30]
```

Each leaf is at the same level, and internal nodes have either 2 or 3 children.