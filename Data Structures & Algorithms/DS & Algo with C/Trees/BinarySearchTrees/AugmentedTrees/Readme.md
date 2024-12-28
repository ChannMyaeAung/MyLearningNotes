## Augmented trees

- Augmented trees are an extension of standard binary search trees (BSTs) that store additional information in their nodes to efficiently support a broader range of queries and operations. 
- By augmenting a BST with extra data, we can solve more complex problems without compromising the tree's fundamental properties, such as search, insertion, and deletion operations.

An **augmented tree** is a binary search tree that, in addition to the standard key and value, stores extra information (often called "augmented data") in each node that caches values that might otherwise be expensive to compute.

- This might include information like the size of a subtree (which can be useful for ranking values, where we want to determine how many elements of the trees are smaller), the height of a subtree, or other summary information like the sum of all the keys in a subtree.

- This additional data is designed to answer more complex queries efficiently. 
- The augmented data must be maintained correctly during tree modifications (insertions and deletions) to ensure the tree remains accurate.

**Key Characteristics:**

- **Binary Search Tree Properties:** Nodes are organized such that for any given node, all keys in the left subtree are less than the node's key, and all keys in the right subtree are greater.
- **Augmented Data:** Extra fields in each node that provide additional information, such as subtree sizes, aggregate values, or other domain-specific data.
- **Efficient Query Support:** The augmented data allows for efficient implementation of operations that would otherwise require traversing the entire tree.

### Common Use Cases

Augmented trees are used in various applications, including:

- **Order-Statistic Trees:** Support operations like finding the k-th smallest element or determining the rank of an element.
- **Interval Trees:** Efficiently manage intervals and support queries like finding all intervals that overlap with a given interval.
- **Range Trees:** Support multi-dimensional range queries.
- **Segment Trees:** Efficiently perform range queries and updates on arrays.
- Storing the height field can be useful for balancing, as in AVL trees.

### Implementing an Augmented Binary Search Tree in C

Let's walk through implementing an **Order-Statistic Tree** in C as an example of an augmented tree. An Order-Statistic Tree allows us to:

- Find the k-th smallest element in the tree.
- Determine the rank (position) of a given element.

To achieve this, each node will store the size of its subtree.

**Key Operations**:

1. **Insert:** Adds a new key to the tree while maintaining the BST property and updating the `size` attribute.
2. **kthSmallest:** Retrieves the k-th smallest element by leveraging the `size` attribute.
3. **getRank:** Determines the rank of a given key in the tree.

#### Program Code

`practice.h`

```C
typedef struct Node
{
    int key;
    int size;
    struct Node *left, *right;
} Node;

Node *createNode(int key);
int getSize(Node *node);
void updateSize(Node *node);
Node *insert(Node *root, int key);
Node *kthSmallest(Node *root, int k);
int getRank(Node *root, int key);
void inorderTraversal(Node *root);
void freeTree(Node *root);
```

`functions.c`

```C
Node *createNode(int key)
{
    Node *newNode = malloc(sizeof(Node));
    if (!newNode)
    {
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }

    newNode->key = key;
    newNode->size = 1;
    newNode->left = newNode->right = NULL;
    return newNode;
}
int getSize(Node *node)
{
    return node ? node->size : 0;
}
void updateSize(Node *node)
{
    if (node)
    {
        node->size = 1 + getSize(node->left) + getSize(node->right);
    }
}

//Navigate the tree as in a standard BST insertion.
// After inserting the new node, update the size of each node on the path back to the root.
// This ensures that every node correctly reflects the total number of nodes in its subtree.
Node *insert(Node *root, int key)
{
    if (root == NULL)
    {
        return createNode(key);
    }

    if (key < root->key)
    {
        root->left = insert(root->left, key);
    }
    else if (key > root->key)
    {
        root->right = insert(root->right, key);
    }
    else
    {
        printf("Duplicate key %d not inserted.\n", key);
        return root;
    }

    updateSize(root);
    return root;
}

Node *kthSmallest(Node *root, int k)
{
    if (root == NULL)
    {
        return NULL;
    }

    // At each node, determine the size of its left subtree (leftSize).
    int leftSize = getSize(root->left);

    // If k equals leftSize + 1, the current node is the k-th smallest.
    if (k == leftSize + 1)
    {
        return root;
    }
    // if k is less than or equal to leftSize, recurse into the left subtree.
    else if (k <= leftSize)
    {
        return kthSmallest(root->left, k);
    }
    else
    {
        // adjusting k to exclude the left subtree and the current node
        return kthSmallest(root->right, k - leftSize - 1);
    }
}

int getRank(Node *root, int key)
{
    if (root == NULL)
    {
        return 0; // key not found
    }

    // Compare key with root->key:
    // Less: The rank resides in the left subtree.
   // Greater: Count all nodes in the left subtree and the current node, then search in the right subtree.
  // Equal: The rank is the number of nodes in the left subtree plus one (for the current node).
    if (key < root->key)
    {
        return getRank(root->left, key);
    }
    else if (key > root->key)
    {
        return 1 + getSize(root->left) + getRank(root->right, key);
    }
    else
    {
        return getSize(root->left) + 1;
    }
}
void inorderTraversal(Node *root)
{
    if (root != NULL)
    {
        inorderTraversal(root->left);
        printf("%d ", root->key);
        inorderTraversal(root->right);
    }
}

void freeTree(Node *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

- Visualization of `kthSmallest()`:

  1. **Base Case:** If the current `root` is `NULL`, return `NULL` (k is out of bounds).

  2. **Compute Left Subtree Size:** Determine the number of nodes in the left subtree (`leftSize`).

  3. **Compare `k` with `leftSize + 1`:**

     - **Equal:** The current node (`root`) is the k-th smallest.

     - **Less:** The k-th smallest lies in the left subtree.

     - **Greater:** The k-th smallest lies in the right subtree, adjusting `k` to exclude the left subtree and the current node.

Consider the following BST with the `size` attribute for each node indicating the number of nodes in its subtree (including itself):

- The `size` attribute in each node of the Binary Search Tree (BST) represents the **total number of nodes** in the subtree rooted at that particular node, **including the node itself**.

```css
             20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/     \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```

**Step-by-Step Execution:**

1. **Start at Root (20):**
   - `leftSize = size of left subtree of 20 (15 (size=3)) = 3` (nodes: 15, 10, 18)
   - `k = 4`
   - Compare:
     - `4 == 3 + 1` → `if(k == leftSize + 1) return root` →**True**
   - **Result:** The current node **20** is the 4th smallest element.

**Another Example:** Find the 5th smallest element (`k = 5`).

1. **Start at Root (20):**
   - `leftSize = 3`
   - `k = 5`
   - Compare:
     - `5 > 3 + 1` → **True**
   - Move to **right subtree** with `k = 5 - 3 - 1 = 1`
2. **Move to Node (25):**
   - `leftSize = 1` (node: 22)
   - `k = 1`
   - Compare:
     - `1 == 1 + 1` → **False**
     - `1 <= 1` → **True**
   - Move to **left subtree** with `k = 1`
3. **Move to Node (22):**
   - `leftSize = 0`
   - `k = 1`
   - Compare:
     - `1 == 0 + 1` → **True**
   - **Result:** The current node **22** is the 5th smallest element.

- **Visualization of `getRank()`**

```css
             20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/      \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```

**Objective:** Find the rank of key `22`.

**Step-by-Step Execution:**

1. Start at Root (20):
   - `key = 22`
   - `22 > 20`
   - Calculate: `1 (current node) + size of left subtree (3) = 4`
   - Move to **right subtree** with `key = 22`
2. Move to Node (25):
   - `key = 22`
   - `22 < 25`
   - Move to **left subtree**
3. Move to Node (22):
   - `key = 22 == 22`
   - Calculate: `size of left subtree (0) + 1 = 1`
   - **Result:** Rank = `4 (from previous step) + 1 = 5`

**Interpretation:** There are **5** elements in the BST that are less than or equal to `22`.

**Another Example:** Find the rank of key `15`.

1. Start at Root (20):
   - `15 < 20`
   - Move to **left subtree**
2. Move to Node (15):
   - `15 == 15`
   - Calculate: `size of left subtree of 15 (which is 10(size = 1)) (1) + 1 = 2`
   - **Result:** Rank = `2`

**Interpretation:** There are **2** elements in the BST that are less than or equal to `15`.

**Summary of Both Functions**

| Function      | Purpose                           | Time Complexity  |
| ------------- | --------------------------------- | ---------------- |
| `kthSmallest` | Find the k-th smallest element    | O(log n) average |
| `getRank`     | Find the number of elements ≤ key | O(log n) average |

`practice.c`

```C
int main(int argc, char *argv[])
{
    Node *root = NULL;
    int keys[] = {20, 15, 25, 10, 18, 22, 30};
    int n = sizeof(keys) / sizeof(keys[0]);

    // insert keys into the tree
    for (int i = 0; i < n; i++)
    {
        root = insert(root, keys[i]);
    }

    printf("In-order traversal of the tree:\n");
    inorderTraversal(root);

    // Find the 3rd smallest element
    int k = 3;
    Node *kth = kthSmallest(root, k);
    if (kth != NULL)
    {
        printf("\n%d-th smallest element is %d\n", k, kth->key);
    }
    else
    {
        printf("\nThere are fewer than %d elements in the tree.\n", k);
    }

    // Get the rank of a specific key
    int keyToFind = 18;
    int rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    freeTree(root);
    root = NULL;// avoid dangling pointer
    printf("The augmented tree has been successfully freed.\n");
    return 0;
}
```

**Visualization Diagrams**

**A. Finding the k-th Smallest Element (`k = 3`)**

```css
Step 1: Start at Root (20)
- leftSize = 3
- k = 3
- Compare: 3 == 3 + 1? No
- 3 <= 3? Yes
- Move to Left Subtree (15) with k = 3

Step 2: At Node (15)
- leftSize = 1
- k = 3
- Compare: 3 == 1 + 1? No
- 3 > 1 + 1? Yes
- Move to Right Subtree (18) with k = 3 - 1 - 1 = 1

Step 3: At Node (18)
- leftSize = 0
- k = 1
- Compare: 1 == 0 + 1? Yes
- **Result:** 18
```

Visualization:

```css
            20
           /  
         15   
           \   
            18  
```

**Finding the Rank of Key `22`**

```css
  			 20(size=7)
           /          	\
      15(size=3)     	25(size=3)
      /      \       		/      \
 10(size=1) 18(size=1) 22(size=1) 30(size=1)
```



```css
Step 1: Start at Root (20)
- key = 22 > 20
- Add 1 (current node) + leftSize (3) = 4
- Move to Right Subtree (25) with key = 22

Step 2: At Node (25)
- key = 22 < 25
- Move to Left Subtree (22)

Step 3: At Node (22)
- key = 22 == 22
- Add 0 (leftSize) + 1 = 1
- **Total Rank:** 4 + 1 = 5
```

**visualization**

```css
            20
              \
               25
              /
            22
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==14232== Memcheck, a memory error detector
==14232== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==14232== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==14232== Command: ./practice
==14232== 
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 18 is 3
The augmented tree has been successfully freed.
==14232== 
==14232== HEAP SUMMARY:
==14232==     in use at exit: 0 bytes in 0 blocks
==14232==   total heap usage: 8 allocs, 8 frees, 1,192 bytes allocated
==14232== 
==14232== All heap blocks were freed -- no leaks are possible
==14232== 
==14232== For lists of detected and suppressed errors, rerun with: -s
==14232== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

chan@CMA:~/C_Programming/practice$ ./practice
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 22 is 5
The augmented tree has been successfully freed.

```

#### Adding a delete function in our Augmented Trees

```C
Node *findMin(Node *root)
{
    while (root->left != NULL)
    {
        root = root->left;
    }
    return root;
}

Node *deleteNode(Node *root, int key)
{
    if (root == NULL)
    {
        return root;
    }

    if (key < root->key)
    {
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->key)
    {
        root->right = deleteNode(root->right, key);
    }
    else
    {
        // Node with only one child or no child
        if (root->left == NULL)
        {
            Node *temp = root->right;
            free(root);
            return temp;
        }
        else if (root->right == NULL)
        {
            Node *temp = root->left;
            free(root);
            return temp;
        }

        // Node with two children: Get the inorder successor (smallest in the right subtree)
        Node *temp = findMin(root->right);

        // copy the inorder successor's content to this node
        root->key = temp->key;

        // delete the inorder successor
        root->right = deleteNode(root->right, temp->key);
    }
    updateSize(root);
    return root;
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    Node *root = NULL;
    int keys[] = {20, 15, 25, 10, 18, 22, 30};
    int n = sizeof(keys) / sizeof(keys[0]);

    // insert keys into the tree
    for (int i = 0; i < n; i++)
    {
        root = insert(root, keys[i]);
    }

    printf("In-order traversal of the tree:\n");
    inorderTraversal(root);

    // Find the 3rd smallest element
    int k = 3;
    Node *kth = kthSmallest(root, k);
    if (kth != NULL)
    {
        printf("\n%d-th smallest element is %d\n", k, kth->key);
    }
    else
    {
        printf("\nThere are fewer than %d elements in the tree.\n", k);
    }

    // Get the rank of a specific key
    int keyToFind = 22;
    int rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    // Delete a node from the tree
    int keyToDelete = 15;
    printf("\nDeleting key %d from the tree.\n", keyToDelete);
    root = deleteNode(root, keyToDelete);

    printf("In-order traversal of the tree after deletion:\n");
    inorderTraversal(root);
    printf("\n");

    // verify the rank after deletion
    rank = getRank(root, keyToFind);
    if (rank > 0)
    {
        printf("Rank of %d after deletion is %d\n", keyToFind, rank);
    }
    else
    {
        printf("Key %d not found in the tree.\n", keyToFind);
    }

    freeTree(root);
    root = NULL; // avoid dangling pointer
    printf("The augmented tree has been successfully freed.\n");
    return 0;
}
```

Initial BST Before Deletion:

```css
        20(size=7)
       /          \
  15(size=3)      25(size=3)
  /      \        /      \
10(size=1)18(size=1)22(size=1)30(size=1)
```

**Delete Key `15`:**

- **Node `15`** has two children: `10` (left) and `18` (right).
- **Inorder Successor:** The smallest node in the right subtree of `15`, which is `18`.
- **Action:** Replace `15` with `18` and adjust pointers accordingly.

BST After Deletion of Key `15`

```css
        20(size=6)
       /          \
  18(size=2)      25(size=3)
  /              /      \
10(size=1)     22(size=1) 30(size=1)
```



#### Output after adding a delete function

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==20431== Memcheck, a memory error detector
==20431== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20431== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20431== Command: ./practice
==20431== 
In-order traversal of the tree:
10 15 18 20 22 25 30 
3-th smallest element is 18
Rank of 22 is 5

Deleting key 15 from the tree.
In-order traversal of the tree after deletion:
10 18 20 22 25 30 
Rank of 22 after deletion is 4
The augmented tree has been successfully freed.
==20431== 
==20431== HEAP SUMMARY:
==20431==     in use at exit: 0 bytes in 0 blocks
==20431==   total heap usage: 8 allocs, 8 frees, 1,192 bytes allocated
==20431== 
==20431== All heap blocks were freed -- no leaks are possible
==20431== 
==20431== For lists of detected and suppressed errors, rerun with: -s
==20431== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

#### Final BST Structure After Deletion of Key 15

```css
            20(size=6)
           /          \
      18(size=2)      25(size=3)
      /              /      \
    10(size=1)     22(size=1) 30(size=1)
```



### Maintaining Augmented Data During Operations

When augmenting a tree with additional data, it's crucial to ensure that this data remains accurate after every modification (insertion, deletion, rotation in balanced trees, etc.). Here are general guidelines:

1. **Insertion:**
   - After inserting a new node, backtrack and update the augmented data (`size` in our example) for each ancestor node.
2. **Deletion:**
   - After removing a node, backtrack and update the augmented data for each ancestor node.
3. **Rotations (in Balanced Trees):**
   - When performing rotations (e.g., in AVL or Red-Black Trees), update the augmented data for the nodes involved in the rotation since their subtrees have changed.
4. **Balancing:**
   - If we're using a balanced tree variant (like AVL or Red-Black Trees), ensure that the balancing operations also maintain the augmented data.

**Example:** In our Order-Statistic Tree, after any insertion or deletion, we called `updateSize(node)` to recalculate the `size` based on the sizes of its left and right subtrees.



### Conclusion

Augmented trees are powerful data structures that extend the capabilities of standard binary search trees by storing additional information within each node. This augmentation enables efficient execution of complex queries and operations that would otherwise be inefficient or cumbersome.

In C, implementing an augmented tree involves:

1. **Designing the Node Structure:** Incorporate fields for both the standard BST properties and the augmented data.
2. **Implementing Core Operations:** Modify insertion, deletion, and other tree operations to maintain the augmented data correctly.
3. **Leveraging Augmented Data:** Utilize the extra information to perform advanced queries efficiently.

