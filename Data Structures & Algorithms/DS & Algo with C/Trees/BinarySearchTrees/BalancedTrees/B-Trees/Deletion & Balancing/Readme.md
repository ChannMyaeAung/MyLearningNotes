### B-Tree Deletion & Balancing

Deleting a key from a B-Tree is more complex than insertion because it requires maintaining the B-Tree properties, especially ensuring that each node (except the root) has at least ⎡m/2⎤ - 1 keys after deletion. The deletion process involves several cases:

1. **Deleting from a Leaf Node:**
   - Simply remove the key from the leaf.
2. **Deleting from an Internal Node:**
   - **Case 1**: If the predecessor (the maximum key in the left subtree) has at least ⎡m/2⎤ keys, replace the key with its predecessor and recursively delete the predecessor.
   - **Case 2**: If the successor (the minimum key in the right subtree) has at least ⎡m/2⎤ keys, replace the key with its successor and recursively delete the successor.
   - **Case 3**: If both the predecessor and successor have only ⎡m/2⎤ - 1 keys, merge the key and the right subtree into the left subtree, and recursively delete the key from the merged subtree.
3. **Balancing the Tree:**
   - If a node falls below the minimum number of keys after deletion, perform operations like borrowing a key from a sibling or merging with a sibling to restore the B-Tree properties.