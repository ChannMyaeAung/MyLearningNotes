#### AVL Trees**

- **Definition:** Named after inventors Adelson-Velsky and Landis, AVL trees are height-balanced binary search trees.
- **Balancing Criteria:** For every node, the height difference between its left and right subtrees (balance factor) is at most **1**.
- **Operations:** Ensure balance through rotations during insertions and deletions.
- **Use Cases:** Situations requiring frequent lookups and insertions, such as in-memory databases.

### Implementing an AVL Tree in C

#### Program Code

`practice.h`

```C
typedef struct AVLNode
{
    int key;
    struct AVLNode *left;
    struct AVLNode *right;
    int height;
} AVLNode;

AVLNode *createNode(int key);
int height(AVLNode *node);

int getBalance(AVLNode *node);

int max(int a, int b);

AVLNode *rightRotate(AVLNode *y);

AVLNode *leftRotate(AVLNode *x);

AVLNode *leftRightRotate(AVLNode *node);

AVLNode *rightLeftRotate(AVLNode *node);

AVLNode *insert(AVLNode *node, int key);

AVLNode *minValueNode(AVLNode *node);

AVLNode *deleteNode(AVLNode *root, int key);

void inorderTraversal(AVLNode *root);

void freeTree(AVLNode *root);
```

`functions.c`

```C
AVLNode *createNode(int key)
{
    AVLNode *node = malloc(sizeof(AVLNode));
    if (!node)
    {
        printf("Memory allocation failed\n");
        exit(1);
    }

    node->key = key;
    node->left = node->right = NULL;
    node->height = 1;
    return node;
}
int height(AVLNode *node)
{
    return node ? node->height : 0;
}

// Function to get balance factor of node
int getBalance(AVLNode *node)
{
    return node ? height(node->left) - height(node->right) : 0;
}

int max(int a, int b)
{
    return a > b ? a : b;
}

AVLNode *rightRotate(AVLNode *y)
{
    AVLNode *x = y->left;
    AVLNode *T2 = x->right;

    // Perform rotation
    x->right = y;
    y->left = T2;

    // Update heights
    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;

    // Return new root
    return x;
}

AVLNode *leftRotate(AVLNode *x)
{
    AVLNode *y = x->right;
    AVLNode *T2 = y->left;

    // Perform rotation
    y->left = x;
    x->right = T2;

    // Update heights
    x->height = max(height(x->left), height(y->right)) + 1;
    y->height = max(height(y->left), height(y->right)) + 1;

    // Return new root
    return y;
}

AVLNode *leftRightRotate(AVLNode *node)
{
    node->left = leftRotate(node->left);
    return rightRotate(node);
}

AVLNode *rightLeftRotate(AVLNode *node)
{
    node->right = rightRotate(node->right);
    return leftRotate(node);
}

AVLNode *insert(AVLNode *node, int key)
{
    // 1. Perform the normal BST insertion
    if (node == NULL)
    {
        return createNode(key);
    }

    if (key < node->key)
    {
        node->left = insert(node->left, key);
    }
    else if (key > node->key)
    {
        node->right = insert(node->right, key);
    }
    else
    {
        // equal keys not allowed in BST
        return node;
    }

    // 2. Update height of this ancestor node
    node->height = 1 + max(height(node->left), height(node->right));

    // 3. Get the balance factor to check whether this node became unbalanced
    int balance = getBalance(node);

    // 4. If unbalanced, then there are 4 cases

    // Positive Balance (>1): Left-heavy.
	// Negative Balance (<-1): Right-heavy.
    // Left Left Case
    if (balance > 1 && key < node->left->key)
    {
        return rightRotate(node);
    }

    // Right Right Case
    if (balance < -1 && key > node->right->key)
    {
        return leftRotate(node);
    }

    // left right case
    if (balance > 1 && key > node->left->key)
    {
        return leftRightRotate(node);
    }
	
    
    // Right left case
    if (balance < -1 && key < node->right->key)
    {
        return rightLeftRotate(node);
    }

    // 5. Return the (unchanged) node pointer
    return node;
}

AVLNode *minValueNode(AVLNode *node)
{
    AVLNode *current = node;

    // Loop down to find the leftmost leaf
    while (current->left != NULL)
    {
        current = current->left;
    }
    return current;
}

AVLNode *deleteNode(AVLNode *root, int key)
{
    // 1. Perform standard BST delete
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
        if ((root->left == NULL) || (root->right == NULL))
        {
            AVLNode *temp = root->left ? root->left : root->right;

            // No child case
            if (temp == NULL)
            {
                temp = root;
                root = NULL;
            }
            else // One child case
            {
                *root = *temp; // Copy the contents of the non-empty child
            }
            free(temp);
        }
        else
        {
            // Node with two children: Get the inorder successor (smallest in the right subtree)
            AVLNode *temp = minValueNode(root->right);
            // Copy the inorder successor's data to this node
            root->key = temp->key;
            
            // Delete the inorder successor
            root->right = deleteNode(root->right, temp->key);
        }
    }

    // If the tree had only one node then return
    if (root == NULL)
    {
        return root;
    }

    // 2. Update height of the current node
    root->height = 1 + max(height(root->left), height(root->right));

    // 3. Get the balance factor
    int balance = getBalance(root);

    // 4. If the node is unbalanced, then there are 4 cases

    // Left Left Case
    if (balance > 1 && getBalance(root->left) >= 0)
    {
        return rightRotate(root);
    }
    
    // Left Right Case
    if (balance > 1 && getBalance(root->left) < 0)
    {
        return leftRightRotate(root);
    }

    // Right Right Case
    if (balance < -1 && getBalance(root->right) <= 0)
    {
        return leftRotate(root);
    }

    // Right Left Case
    if (balance < -1 && getBalance(root->right) > 0)
    {
        return rightLeftRotate(root);
    }

    return root;
}
void inorderTraversal(AVLNode *root)
{
    if (root != NULL)
    {
        inorderTraversal(root->left);
        printf("%d ", root->key);
        inorderTraversal(root->right);
    }
}

void freeTree(AVLNode *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

`insert()`:

- **Left Left (LL) Case:**
  - **Condition:** `balance > 1` and `key < node->left->key`
  - **Explanation:** The imbalance is caused by an insertion in the **left subtree's left** side.
  - **Action:** Perform a **Right Rotation** to balance.
- **Right Right (RR) Case:**
  - **Condition:** `balance < -1` and `key > node->right->key`
  - **Explanation:** The imbalance is caused by an insertion in the **right subtree's right** side.
  - **Action:** Perform a **Left Rotation** to balance.
- **Left Right (LR) Case:**
  - **Condition:** `balance > 1` and `key > node->left->key`
  - **Explanation:** The imbalance is caused by an insertion in the **left subtree's right** side.
  - **Action:** Perform a **Left Rotation** on the left child, followed by a **Right Rotation** on the current node.
- **Right Left (RL) Case:**
  - **Condition:** `balance < -1` and `key < node->right->key`
  - **Explanation:** The imbalance is caused by an insertion in the **right subtree's left** side.
  - **Action:** Perform a **Right Rotation** on the right child, followed by a **Left Rotation** on the current node.

**`deleteNode()`**

- **Left Left (LL) Case:**
  - **Condition:** `balance > 1` and `getBalance(root->left) >= 0`
  - **Explanation:** The imbalance is due to deletion in the **left subtree's left** side.
  - **Action:** Perform a **Right Rotation**.
- **Left Right (LR) Case:**
  - **Condition:** `balance > 1` and `getBalance(root->left) < 0`
  - **Explanation:** The imbalance is due to deletion in the **left subtree's right** side.
  - **Action:** Perform a **Left Rotation** on the left child, followed by a **Right Rotation** on the current node.
- **Right Right (RR) Case:**
  - **Condition:** `balance < -1` and `getBalance(root->right) <= 0`
  - **Explanation:** The imbalance is due to deletion in the **right subtree's right** side.
  - **Action:** Perform a **Left Rotation**.
- **Right Left (RL) Case:**
  - **Condition:** `balance < -1` and `getBalance(root->right) > 0`
  - **Explanation:** The imbalance is due to deletion in the **right subtree's left** side.
  - **Action:** Perform a **Right Rotation** on the right child, followed by a **Left Rotation** on the current node.

`practice.c`

```C
int main(int argc, char *argv[])
{
    AVLNode *root = NULL;

    root = insert(root, 10);
    root = insert(root, 20);
    root = insert(root, 30);
    root = insert(root, 40);
    root = insert(root, 50);
    root = insert(root, 25);

    printf("Inorder traversal of the constructed AVL tree is\n");
    inorderTraversal(root);
    printf("\n");

    root = deleteNode(root, 40);
    printf("Inorder traversal of the AVL tree after deletion of 40\n");
    inorderTraversal(root);
    printf("\n");

    freeTree(root);
    root = NULL;
    printf("Tree has been freed\n");
    return 0;
}

```

##### **Detailed Breakdown**

1. **Initial Insertions:**

   - **Insert 10:**

     ```css
     10 (size=1)
     ```

   - **Insert 20:**

     ```css
       10 (1)
         \
         20 (1)
     ```

   - **Insert 30:**
     After balancing (Left Rotation on node 10):

     ```css
         20 (2)
        /    \
     10 (1)  30 (1)
     ```

   - **Insert 40:**

     ```css
         20 (3)
        /    \
     10 (1)  30 (2)
               \
               40 (1)
     ```

     

   - **Insert 50:**
     After balancing (Left Rotation on node 20):

      ```css
           30 (4)
          /     \
       20 (3)   40 (2)
      /          \
     10 (1)        50 (1)
      ```

     

   - **Insert 25:**

     ```css
           30 (5)
          /     \
       20 (4)   40 (2)
      /    \      \
     10 (1) 25 (1) 50 (1)
     ```

     

2. **Deletion Operation:**

   - **Delete Key `40`:**

     - **Node `40`** has one child: `50`.

     - **Action:** Replace node `40` with its child `50`.

     - **Resulting Tree:**

       ```css
           30 (4)
          /     \
       20 (3)   50 (1)
        /    \
       10(1) 25(1)
       ```

   - Size Updates:

     - **Node `30`:**
       - Left Subtree Size: `3` (nodes `20`, `10`, `25`)
       - Right Subtree Size: `1` (node `50`)
       - **Total Size:** `1 (itself)` + `3` + `1` = `5`
     - **Node `20`:**
       - Left Subtree Size: `1` (node `10`)
       - Right Subtree Size: `1` (node `25`)
       - **Total Size:** `1 (itself)` + `1` + `1` = `3`
     - **Nodes `10`, `25`, `50`:**
       - **Size:** `1` each

------

**Final In-Order Traversal**

```css
10, 20, 25, 30, 50
```

------

**Visualization Summary**

- **After Inserting Keys `10`, `20`, `30`, `40`, `50`, `25`:**

  The AVL tree balances itself after each insertion to maintain optimal height, ensuring that the tree remains balanced for efficient operations.

- **After Deleting Key `40`:**

  - **Replacement Strategy:**
    Since node `40` has only one child (`50`), it is directly replaced by its child.
  - **Resulting Structure:**
    The tree remains balanced with node `30` as the new root, maintaining the AVL property.

------

**Final Tree Diagram**

```css
        30 (size=5)
       /        \
  20 (size=3)    50 (size=1)
  /      \
10 (1)   25 (1)
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==20719== Memcheck, a memory error detector
==20719== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20719== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20719== Command: ./practice
==20719== 
Inorder traversal of the constructed AVL tree is
10 20 25 30 40 50 
Inorder traversal of the AVL tree after deletion of 40
10 20 25 30 50 
Tree has been freed
==20719== 
==20719== HEAP SUMMARY:
==20719==     in use at exit: 0 bytes in 0 blocks
==20719==   total heap usage: 7 allocs, 7 frees, 1,216 bytes allocated
==20719== 
==20719== All heap blocks were freed -- no leaks are possible
==20719== 
==20719== For lists of detected and suppressed errors, rerun with: -s
==20719== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

