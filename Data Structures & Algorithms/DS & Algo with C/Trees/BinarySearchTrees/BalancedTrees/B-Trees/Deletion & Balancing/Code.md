### Complete B-Tree Implementation with Deletion in C

Below is the complete C implementation of a B-Tree with both insertion and deletion operations. This includes all necessary functions to maintain the B-Tree properties during deletions.

#### Newly Added functions for Deletion in B-Trees in C

1. `findKey()`:
   - **Purpose**: Finds the first index in the node  where `keys[idx]` is greater than or equal to `key`.
   - **Usage**: Determines the position of the key or the child pointer to follow for deletion.
2. `removeFromNode()`:
   - **Purpose**: Removes a key from the subtree rooted at node `node`.
   - **Process**:
     - Finds the key in the current node.
     - If the key is present:
       - If it's a leaf, remove it directly.
       - If it's an internal node, handle it based on whether the predecessor or successor has enough keys.
     - If the key is not present:
       - Ensure the appropriate child has enough keys by filling it if necessary.
       - Recursively proceed to the correct child.
3. `removeFromLeaf()`:
   - **Purpose**: Removes a key from a leaf node.
   - **Process**: Shifts all keys after the target index to fill the gap.
4. `removeFromNonLeaf()`:
   - **Purpose**: Removes a key from an internal node.
   - **Process**:
     - If the predecessor child has enough keys, replace the key with its predecessor and delete the predecessor.
     - Else, if the successor child has enough keys, replace the key with its successor and delete the successor.
     - Else, merge the key and the successor child into the predecessor child and delete the key from the merged child.
5. `getPredecessor()` and `getSuccessor()`:
   - **Purpose**: Retrieve the predecessor (maximum key in the left subtree) or successor (minimum key in the right subtree) of a key in an internal node.
   - **Usage**: Used to replace the key to be deleted with its predecessor or successor.
6. `fill()`:
   - **Purpose**: Ensures that the child node at index `idx` has at least `t` keys.
   - **Process**:
     - Tries to borrow a key from the left sibling.
     - If not possible, tries to borrow from the right sibling.
     - If neither is possible, merges the child with a sibling.
7. `borrowFromPrev()` and `borrowFromNext()`:
   - **Purpose**: Borrow a key from the left (`borrowFromPrev`) or right (`borrowFromNext`) sibling to maintain the minimum number of keys in the child node.
   - **Process**:
     - Adjust keys and child pointers between the parent and sibling to transfer a key.
8. `merge()`:
   - **Purpose**: Merges two child nodes with their parent key to ensure that the child has enough keys.
   - **Process**:
     - Moves the separator key from the parent down into the child.
     - Copies all keys and child pointers from the sibling into the child.
     - Removes the key and child pointer from the parent.
     - Frees the memory allocated to the sibling node.

#### Program Code

`practice.h`

```C
#define T 3

typedef struct BTreeNode
{
    int *keys;
    int t;
    struct BTreeNode **C;
    int n;
    bool leaf;
} BTreeNode;

typedef struct BTree
{
    BTreeNode *root;
    int t;
} BTree;

BTreeNode *createNode(int t, bool leaf);

BTree *initializeBTree(int t);
BTreeNode *search(BTreeNode *root, int key);
void splitChild(BTreeNode *parent, int i, BTreeNode *fullChild);

void insertNonFull(BTreeNode *node, int key);
void insert(BTree *tree, int key);

void traverse(BTreeNode *root);

void removeKey(BTree *tree, int key);

void freeTree(BTreeNode *root);
```

`functions.c`

```C
BTreeNode *createNode(int t, bool leaf)
{
    BTreeNode *node = malloc(sizeof(BTreeNode));
    node->t = t;
    node->leaf = leaf;

    node->keys = malloc(sizeof(int) * (2 * t - 1));
    node->C = malloc(sizeof(BTreeNode *) * (2 * t));

    for (int i = 0; i < 2 * t; i++)
    {
        node->C[i] = NULL;
    }
    node->n = 0;
    return node;
}

BTree *initializeBTree(int t)
{
    BTree *tree = malloc(sizeof(BTree));
    tree->root = createNode(t, true);
    tree->t = t;
    return tree;
}
BTreeNode *search(BTreeNode *root, int key)
{
    // starts from the left key
    int i = 0;

    // check if the key to be searched is within the range of n and greater than the current key in the node
    while (i < root->n && key > root->keys[i])
    {

        // increment the index to check the next key
        i++;
    }

    if (i < root->n && key == root->keys[i])
    {
        return root;
    }

    if (root->leaf)
    {
        return NULL;
    }

    return search(root->C[i], key);
}
void splitChild(BTreeNode *parent, int i, BTreeNode *fullChild)
{
    // Minimum degree
    int t = parent->t;

    // A new node to hold the latter half of fullChild's keys
    BTreeNode *newChild = createNode(t, fullChild->leaf);

    // # of keys in newChild after splitting
    newChild->n = t - 1;

    // copy the last (t-1) keys of fullChild to newChild
    for (int j = 0; j < t - 1; j++)
    {
        newChild->keys[j] = fullChild->keys[j + t];
    }

    // Copy the last t children of fullChild to newChild
    if (!fullChild->leaf)
    {
        for (int j = 0; j < t; j++)
        {
            newChild->C[j] = fullChild->C[j + t];
        }
    }

    fullChild->n = t - 1;

    // Create space for newChild in parent
    //  shifts the child pointers in the parent node to make room for the new child
    for (int j = parent->n; j >= i + 1; j--)
    {
        parent->C[j + 1] = parent->C[j];
    }

    // inserts newChild as a child of parent at position i + 1
    parent->C[i + 1] = newChild;

    // Move keys in parent to make space for the middle key
    for (int j = parent->n - 1; j >= i; j--)
    {
        parent->keys[j + 1] = parent->keys[j];
    }

    // Copy the middle key of fullChild to parent
    parent->keys[i] = fullChild->keys[t - 1];
    parent->n += 1;
}

void insertNonFull(BTreeNode *node, int key)
{
    // starts from the rightmost key in the node and move left to find the correct position for the new key
    int i = node->n - 1;

    if (node->leaf)
    {
        // Find the location where new key must be inserted
        while (i >= 0 && node->keys[i] > key)
        {
            node->keys[i + 1] = node->keys[i];
            i--;
        }
        // for e.g [3, 4, 5, 7]
        // we want to insert 6, so i starts at 7, since 7 > 6, decrement i and now i will points to 5, i = 2. 5 < 6, so we insert 6 next to 5 which is i + 1 = 2 + 1 = 3 in the 3rd index.
        // [3,4,5,6,7]
        // Insert the new key at found location
        node->keys[i + 1] = key;
        node->n += 1;
    }
    else
    {
        // find the child which is going to have the new key
        while (i >= 0 && node->keys[i] > key)
        {
            i--;
        }

        // check if the found child is full
        if (node->C[i + 1]->n == 2 * node->t - 1)
        {
            // if the child is full, split it
            splitChild(node, i + 1, node->C[i + 1]);

            // After split, the middle key of C[i+1] goes up and
            // C[i+1] is split into two. Decide which of the two
            // will have the new key
            if (node->keys[i + 1] < key)
            {
                i++;
            }
        }
        // recursively call insertNonFull on the chosen child
        insertNonFull(node->C[i + 1], key);
    }
}
void insert(BTree *tree, int key)
{
    BTreeNode *root = tree->root;

    // If the root is full, then tree grows in height
    if (root->n == 2 * tree->t - 1)
    {
        BTreeNode *s = createNode(tree->t, false);
        s->C[0] = root;

        // use splitChild() to divide the full root into two nodes
        // move the middle key of the old root up to the new root
        splitChild(s, 0, root);

        // new root has two children. Decide which of the two children is going to have the new key
        int i = 0;

        if (s->keys[0] < key)
        {
            i++;
        }

        // call insertNonFull() on the appropriate child node to insert the new key
        insertNonFull(s->C[i], key);

        tree->root = s;
    }
    else
    {
        insertNonFull(root, key);
    }
}

void traverse(BTreeNode *root)
{
    if (root != NULL)
    {
        for (int i = 0; i < root->n; i++)
        {
            if (!root->leaf)
            {
                traverse(root->C[i]);
            }
            printf("%d ", root->keys[i]);
        }
        if (!root->leaf)
        {
            traverse(root->C[root->n]);
        }
    }
}

static int findKey(BTreeNode *node, int key)
{
    int idx = 0;
    while (idx < node->n && node->keys[idx] < key)
    {
        ++idx;
    }
    return idx;
}

static int getPredecessor(BTreeNode *node, int idx)
{
    BTreeNode *current = node->C[idx];
    while (!current->leaf)
    {
        current = current->C[current->n];
    }
    return current->keys[current->n - 1];
}

static int getSuccessor(BTreeNode *node, int idx)
{
    BTreeNode *current = node->C[idx + 1];
    while (!current->leaf)
    {
        current = current->C[0];
    }
    return current->keys[0];
}

// Function to borrow a key from C[idx - 1] and insert it into C[idx]
static void borrowFromPrev(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx - 1];

    // Move all keys in child one step ahead
    for (int i = child->n - 1; i >= 0; --i)
    {
        child->keys[i + 1] = child->keys[i];
    }

    // if child is not a leaf, move its child pointers
    if (!child->leaf)
    {
        for (int i = child->n; i >= 0; --i)
        {
            child->C[i + 1] = child->C[i];
        }
    }

    // set child's first key equal to keys[idx - 1] from node
    child->keys[0] = node->keys[idx - 1];

    // move sibling's last child as C[idx]'s first child
    if (!child->leaf)
    {
        child->C[0] = sibling->C[sibling->n];
    }

    // Move the last key from sibling to node
    node->keys[idx - 1] = sibling->keys[sibling->n - 1];

    child->n += 1;
    sibling->n -= 1;
}

// Function to borrow a key from C[idx + 1] and insert it into C[idx]
static void borrowFromNext(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx + 1];

    // node's key[idx] is inserted as the last key in child
    child->keys[child->n] = node->keys[idx];

    // sibling's first child is inserted as the last child into child
    if (!child->leaf)
    {
        child->C[child->n + 1] = sibling->C[0];
    }

    // The first key from sibling is inserted into node
    node->keys[idx] = sibling->keys[0];

    // Move all keys in sibling one step to the left
    for (int i = 1; i < sibling->n; i++)
    {
        sibling->keys[i - 1] = sibling->keys[i];
    }

    // Move the child pointers one step to the left
    if (!sibling->leaf)
    {
        for (int i = 1; i <= sibling->n; i++)
        {
            sibling->C[i - 1] = sibling->C[i];
        }
    }

    child->n += 1;
    sibling->n -= 1;
}

// Function to merge C[idx] with C[idx + 1]
static void merge(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx + 1];
    int t = node->t;

    // Pull down the key from node and insert it into (t-1) position of the child
    child->keys[t - 1] = node->keys[idx];

    // Copy the keys from sibling to child
    for (int i = 0; i < sibling->n; i++)
    {
        child->keys[i + t] = sibling->keys[i];
    }

    // Copy the child pointers from sibling to child
    if (!child->leaf)
    {
        for (int i = 0; i <= sibling->n; i++)
        {
            child->C[i + 1] = sibling->C[i];
        }
    }

    // Move all keys after idx in the current node one step to the left
    for (int i = idx + 1; i < node->n; i++)
    {
        node->keys[i - 1] = node->keys[i];
    }

    // Move the child pointers after idx + 1 in the current node one step to the left
    for (int i = idx + 2; i <= node->n; i++)
    {
        node->C[i - 1] = node->C[i];
    }

    child->n += sibling->n + 1;
    node->n--;
}

// Function to fill the child node at idx which has less than t-1 keys
static void fill(BTreeNode *node, int idx)
{
    if (idx != 0 && node->C[idx - 1]->n >= node->t)
    {
        // borrow from the previous child
        borrowFromPrev(node, idx);
    }
    else if (idx != node->n && node->C[idx + 1]->n >= node->t)
    {
        borrowFromNext(node, idx);
    }
    else
    {
        if (idx != node->n)
        {
            // Merge C[idx] with C[idx + 1]
            merge(node, idx);
        }
        else
        {
            // merge C[idx - 1] with C[idx]
            merge(node, idx - 1);
        }
    }
}

static void removeFromNode(BTreeNode *node, int key);

static void removeFromLeaf(BTreeNode *node, int idx)
{
    // shift all keys after idx to the left
    for (int i = idx + 1; i < node->n; i++)
    {
        node->keys[i - 1] = node->keys[i];
    }
    node->n--;
}

static void removeFromNonLeaf(BTreeNode *node, int idx)
{
    int key = node->keys[idx];

    // if the child that precedes key has at least t keys, find the predecessor 'pred' of key in the subtree rooted at C[idx]. Replace key by pred. Recursively delete pred in C[idx]
    if (node->C[idx]->n >= node->t)
    {
        int pred = getPredecessor(node, idx);
        node->keys[idx] = pred;
        removeFromNode(node->C[idx], pred);
    }
    // if the child that succeeds key has at least t keys
    else if (node->C[idx + 1]->n >= node->t)
    {
        int succ = getSuccessor(node, idx);
        node->keys[idx] = succ;
        removeFromNode(node->C[idx + 1], succ);
    }
    // if both C[idx] and C[idx + 1] have t - 1 keys, merge them
    else
    {
        merge(node, idx);
        removeFromNode(node->C[idx], key);
    }
}

// Function to remove the key from the subtree rooted with the node
static void removeFromNode(BTreeNode *node, int key)
{
    int idx = findKey(node, key);

    // the key to be removed is present in this node
    if (idx < node->n && node->keys[idx] == key)
    {
        if (node->leaf)
        {
            // case 1: the node is a leaf
            removeFromLeaf(node, idx);
        }
        else
        {
            // case 2: the node is an internal node
            removeFromNonLeaf(node, idx);
        }
    }
    else
    {
        // the key is not present in this node
        if (node->leaf)
        {
            printf("The key %d does not exist in the tree.\n", key);
            return;
        }

        // Flag indicates whether the key is present in the subtree rooted with the last child
        bool flag = ((idx == node->n) ? true : false);

        // if the child where the key is supposed to exist has less than t keys, fill it
        if (node->C[idx]->n < node->t)
        {
            fill(node, idx);
        }

        // If the last child has been merged, it must have been merged with the previous child
        if (flag && idx > node->n)
        {
            removeFromNode(node->C[idx - 1], key);
        }
        else
        {
            removeFromNode(node->C[idx], key);
        }
    }
}

void removeKey(BTree *tree, int key)
{
    if (!tree->root)
    {
        printf("The tree is empty.\n");
        return;
    }

    removeFromNode(tree->root, key);

    // if the root node has 0 keys, make its first child the new root
    if (tree->root->n == 0)
    {
        BTreeNode *tmp = tree->root;
        if (tree->root->leaf)
        {
            tree->root = NULL;
        }
        else
        {
            tree->root = tree->root->C[0];
        }
        free(tmp->keys);
        free(tmp->C);
        free(tmp);
    }
}

void freeTree(BTreeNode *root)
{
    if (root == NULL)
    {
        return;
    }
    if (!root->leaf)
    {
        for (int i = 0; i < root->n; i++)
        {
            freeTree(root->C[i]);
        }
        freeTree(root->C[root->n]);
    }

    free(root->keys);
    free(root->C);
    free(root);
}
```

`practice.c`

```C
int main()
{
    BTree *tree = initializeBTree(T);

    int keys[] = {10, 20, 5, 6, 12, 30, 7, 17};
    int n = sizeof(keys) / sizeof(keys[0]);

    for (int i = 0; i < n; i++)
    {
        insert(tree, keys[i]);
    }

    printf("Traversal of the constructed B-tree is: \n");
    traverse(tree->root);
    printf("\n");

    // Search for a key
    int key = 6;
    BTreeNode *res = search(tree->root, key);
    if (res != NULL)
    {
        printf("Key %d found in the B-Tree.\n", key);
    }
    else
    {
        printf("Key %d not found in the B-Tree.\n", key);
    }

    // Delete a key
    printf("\nDeleting key %d...\n", key);
    removeKey(tree, key);

    printf("Traversal after deletion is: ");
    traverse(tree->root);
    printf("\n");

    // Search for the deleted key
    res = search(tree->root, key);
    if (res != NULL)
    {
        printf("Key %d found in the B-Tree.\n", key);
    }
    else
    {
        printf("Key %d not found in the B-Tree.\n", key);
    }

    // Additional deletions to demonstrate balancing
    int del_keys[] = {6, 13, 7, 4, 2, 16};
    int del_n = sizeof(del_keys) / sizeof(del_keys[0]);

    for (int i = 0; i < del_n; i++)
    {
        printf("\nDeleting key %d...\n", del_keys[i]);
        removeKey(tree, del_keys[i]);
        printf("Traversal after deletion is: ");
        printf("Traversal after deletion is: ");
        traverse(tree->root);
        printf("\n");
    }

    freeTree(tree->root);
    free(tree);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==20539== Memcheck, a memory error detector
==20539== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20539== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20539== Command: ./final
==20539== 
Traversal of the constructed B-tree is: 
5 6 7 10 12 17 20 30 
Key 6 found in the B-Tree.

Deleting key 6...
Traversal after deletion is: 5 7 10 12 17 20 30 
Key 6 not found in the B-Tree.

Deleting key 6...
The key 6 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 5 7 10 12 17 20 30 

Deleting key 13...
The key 13 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 5 7 10 12 17 20 30 

Deleting key 7...
Traversal after deletion is: Traversal after deletion is: 5 10 12 17 20 30 

Deleting key 4...
The key 4 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 5 10 12 17 20 30 

Deleting key 2...
The key 2 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 5 10 12 17 20 30 

Deleting key 16...
The key 16 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 5 10 12 17 20 30 
==20539== 
==20539== HEAP SUMMARY:
==20539==     in use at exit: 0 bytes in 0 blocks
==20539==   total heap usage: 11 allocs, 11 frees, 1,340 bytes allocated
==20539== 
==20539== All heap blocks were freed -- no leaks are possible
==20539== 
==20539== For lists of detected and suppressed errors, rerun with: -s
==20539== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

#### Step-by-Step B-Tree Construction and Deletion of this program

**Insert 5**

- **Action:** Insert into root.

- Tree Structure:

  ```css
  [5]
  ```

**2. Insert 6**

- **Action:** Insert into root (not full).

- Tree Structure:

  ```css
  [5, 6]
  ```

**3. Insert 7**

- **Action:** Insert into root (not full).

- Tree Structure:

  ```css
  [5, 6, 7]
  ```

**4. Insert 10**

- **Action:** Insert into root (not full).

- Tree Structure:

  ```css
  [5, 6, 7, 10]
  ```

**5. Insert 12**

- **Action:** Insert into root (now has 4 keys, still within capacity).

- Tree Structure:

  ```css
  [5, 6, 7, 10, 12]
  ```

  - **Note:** Root now has **5** keys, reaching the maximum capacity for t=3.

**6. Insert 17**

- **Action:** Root is full; **split the root** before insertion.

  **Splitting Process:**

  1. **Identify Median Key:** `7` (3rd key in `[5, 6, 7, 10, 12]`).
  2. Create Two Children:
     - **Left Child:** `[5, 6]`
     - **Right Child:** `[10, 12, 17]` (after inserting `17`).
  3. Promote Median Key to New Root:
     - **New Root:** `[7]`

  **Tree Structure After Split and Insertion:**

  ```css
          [7]
         /   \
    [5, 6]  [10, 12, 17]
  ```

**7. Insert 20**

- **Action:** Traverse to the right child `[10, 12, 17]` (since `20 > 7`).

- **Insert `20` into `[10, 12, 17]` (not full).**

- **Updated Right Child:** `[10, 12, 17, 20]`

  **Tree Structure:**

  ```css
          [7]
         /   \
    [5, 6]  [10, 12, 17, 20]
  ```

**8. Insert 30**

- **Action:** Traverse to the right child `[10, 12, 17, 20]` (since `30 > 7`).

- **Insert `30` into `[10, 12, 17, 20]` (now has 5 keys, reaching maximum capacity).**

- **Updated Right Child:** `[10, 12, 17, 20, 30]`

  **Tree Structure:**

  ```css
          [7]
         /   \
    [5, 6]  [10, 12, 17, 20, 30]
  ```

  - **Note:** The right child now has **5** keys, which is the maximum for t=3t = 3t=3. Any further insertions into this node would require another split.

**B. Deleting Keys from the B-Tree**

We'll perform the deletions in the following order: {6, 6, 13, 7, 4, 2, 16}

**1. Delete Key 6**

- **Action:** Remove key `6` from the left child `[5, 6]`.

- **Resulting Left Child:** `[5]`

  **Tree Structure Before Fixing Underflow:**

  ```css
          [7]
         /   \
    [5]    [10, 12, 17, 20, 30]
  ```

- **Issue:** Left child `[5]` now has **1** key, which is **less than the minimum requirement** (t−1=2).

- **Solution:** **Borrow** from the sibling or **merge** nodes.

  - **Check Siblings:** Only the right sibling `[10, 12, 17, 20, 30]` exists and has **5** keys (maximum capacity).
  - **Borrow from Sibling:**
    1. **Borrow the first key (`10`) from the right sibling.**
    2. **Move the parent key (`7`) down to the left child.**
  - **After Borrowing:**
    - **New Root Keys:** `[10]`
    - **Left Child:** `[5, 7]`
    - **Right Child:** `[12, 17, 20, 30]`

  **Tree Structure After Deletion:**

  ```css
          [10]
         /    \
    [5, 7]  [12, 17, 20, 30]
  ```

**2. Delete Key 6 Again**

- **Action:** Attempt to delete key `6` which no longer exists in the tree.
- Result:
  - **Message:** "The key 6 does not exist in the tree."
  - **Traversal remains unchanged:** `5 7 10 12 17 20 30`

**3. Delete Key 13**

- **Action:** Attempt to delete key `13` which does not exist in the tree.
- Result:
  - **Message:** "The key 13 does not exist in the tree."
  - **Traversal remains unchanged:** `5 7 10 12 17 20 30`

**4. Delete Key 7**

- **Action:** Remove key `7` from the left child `[5, 7]`.

- **Resulting Left Child:** `[5]`

  **Tree Structure Before Fixing Underflow:**

  ```css
          [10]
         /    \
    [5]    [12, 17, 20, 30]
  ```

- **Issue:** Left child `[5]` now has **1** key, which is **less than the minimum requirement** (t−1=2).

- **Solution:** **Borrow** from the sibling or **merge** nodes.

  - **Check Siblings:** Only the right sibling `[12, 17, 20, 30]` exists and has **4** keys (t=3, so minimum required is 2).
  - **Borrow from Sibling:**
    1. **Borrow the first key (`12`) from the right sibling.**
    2. **Move the parent key (`10`) down to the left child.**
  - **After Borrowing:**
    - **New Root Keys:** `[12]`
    - **Left Child:** `[5, 10]`
    - **Right Child:** `[17, 20, 30]`

  **Tree Structure After Deletion:**

  ```css
          [12]
         /    \
    [5, 10] [17, 20, 30]
  ```

**5. Delete Key 4**

- **Action:** Attempt to delete key `4` which does not exist in the tree.
- Result:
  - **Message:** "The key 4 does not exist in the tree."
  - **Traversal remains unchanged:** `5 10 12 17 20 30`

**6. Delete Key 2**

- **Action:** Attempt to delete key `2` which does not exist in the tree.
- Result:
  - **Message:** "The key 2 does not exist in the tree."
  - **Traversal remains unchanged:** `5 10 12 17 20 30`

**7. Delete Key 16**

- **Action:** Attempt to delete key `16` which does not exist in the tree.
- Result:
  - **Message:** "The key 16 does not exist in the tree."
  - **Traversal remains unchanged:** `5 10 12 17 20 30`

------

**4. Final B-Tree Structure with t=3**

After performing all the insertions and deletions, the final B-Tree structure is as follows:

```css
          [12]
         /    \
    [5, 10] [17, 20, 30]
```

**Diagrammatic Representation:**

```css
          [12]
         /    \
    [5, 10] [17, 20, 30]
```

**Traversal Confirmation:**

Performing an **in-order traversal** of the final tree yields:

```css
5, 10, 12, 17, 20, 30 
```

------

**5. Verification of B-Tree Properties**

Let's ensure that the **final B-Tree** adheres to all B-Tree properties with **minimum degree t=3**:

1. **Node Key Constraints:**

   - **Root Node:**

     ```css
     [12]
     ```

     - **Number of Keys:** 1
     - **Validity:** As the **root**, it can have fewer than t−1=2 keys. It is valid.

   - **Child 0:**

     ```css
     [5, 10]
     ```

     - **Number of Keys:** 2
     - **Validity:** Meets the minimum requirement (t−1=2).

   - Child 1:

     ```css
     [17, 20, 30]
     ```

     - **Number of Keys:** 3
     - **Validity:** Within the range (2≤keys≤5).

2. **Child Pointers:**

   - Root Node: Has 2 children, which is within the allowed range (

     t≤children≤2t → 3≤ children ≤6).

     - **Note:** For t=3, a node should have at least 3 children if it has more than 1 key. However, since the root has only 1 key, it is allowed to have fewer children.

   - **Child Nodes:** Both are **leaf nodes**, so they do not have child pointers.

3. **Leaf Nodes:**

   - All leaf nodes are at the **same depth**, ensuring the tree is **balanced**.

4. **Ordering:**

   - **In-Node Ordering:** All keys within each node are **sorted in ascending order**.
   - **Subtree Ordering:** All keys in the left subtree of a key are **less than the key**, and all keys in the right subtree are **greater**.

5. **Height:**

   - **Balanced Height:** The tree has a height of **1** (root) + **1** (children) = **2**, ensuring minimal height for efficient operations.

#### Program Output #2

Tested with another array of values:  `int keys[] = {10, 20, 30, 35, 40, 45, 50, 80, 70, 60, 90, 100};`

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==9860== Memcheck, a memory error detector
==9860== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==9860== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==9860== Command: ./final
==9860== 
Traversal of the constructed B-tree is: 
10 20 30 35 40 45 50 60 70 80 90 100 
Key 40 found in the B-Tree.

Deleting key 40...
Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 
Key 40 not found in the B-Tree.

Deleting key 6...
The key 6 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 

Deleting key 13...
The key 13 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 

Deleting key 7...
The key 7 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 

Deleting key 4...
The key 4 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 

Deleting key 2...
The key 2 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 

Deleting key 16...
The key 16 does not exist in the tree.
Traversal after deletion is: Traversal after deletion is: 10 20 30 35 45 50 60 70 80 90 100 
==9860== 
==9860== HEAP SUMMARY:
==9860==     in use at exit: 0 bytes in 0 blocks
==9860==   total heap usage: 17 allocs, 17 frees, 1,540 bytes allocated
==9860== 
==9860== All heap blocks were freed -- no leaks are possible
==9860== 
==9860== For lists of detected and suppressed errors, rerun with: -s
==9860== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**B-Tree Diagram**

**Step 1: Insert 10**

```css
[10]
```

------

**Step 2: Insert 20**

```css
[10, 20]
```

------

**Step 3: Insert 30**

```css
[10, 20, 30]
```

------

**Step 4: Insert 35**

```css
[10, 20, 30, 35]
```

------

**Step 5: Insert 40**

```css
[10, 20, 30, 35, 40]
```

------

**Step 6: Insert 45**

```css
        [30]
       /    \
[10, 20]  [35, 40, 45]
```

------

**Step 7: Insert 50**

```css
        [30]
       /    \
[10, 20]  [35, 40, 45, 50]
```

------

**Step 8: Insert 60**

```css
        [30]
       /    \
[10, 20]  [35, 40, 45, 50, 60]
```

------

**Step 9: Insert 70**

```css
          [30, 60]
         /    |    \
[10, 20] [35, 40, 45, 50] [70]
```

------

**Step 10: Insert 80**

```css
          [30, 60]
         /    |    \
[10, 20] [35, 40, 45, 50, 60, 70, 80]
```

------

**Step 11: Insert 90**

```css
          [30, 60]
         /    |      \
[10, 20] [35, 40, 45, 50] [70, 80, 90]
```

------

**Step 12: Insert 100**

```css
          [30, 60]
         /    |      \
[10, 20] [35, 40, 45, 50] [70, 80, 90, 100]
```

------

#### **Delete 40**

```css
          [30, 60]
         /    |      \
[10, 20]  [35, 45, 50] [70, 80, 90, 100]
```

**Final B-Tree Structure**

```css
           [30, 60]
         /    |      \
[10, 20] [35, 45, 50, 60] [70, 80, 90, 100]
```

