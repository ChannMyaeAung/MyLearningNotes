### What is a Binary Search Tree (BST)?

A **Binary Search Tree (BST)** is a type of binary tree where each node has at most two children, and it maintains a specific order:

- **Left Subtree**: All nodes have values **less than** the parent node.
- **Right Subtree**: All nodes have values **greater than** the parent node.

This property makes BSTs efficient for operations like searching, insertion, and deletion.

- Structurally, a complete binary tree consists of either a single node (a leaf) or a root node with a left and right **subtree**, each of which is itself either a leaf or a root node with two subtrees.

**Visual Representation**

```css
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

- **Example**: Searching for the number `7` involves comparing with `8` (go left), then `3` (go right), then `6` (go right), and finally finding `7`.



### Common Operations on BSTs

#### a. Insertion

**Goal**: Add a new node to the BST while maintaining its properties.

**Algorithm**:

1. Start at the **root**.
2. Compare the **value** to insert with the current node.
3. If the value is **less**, go to the **left child**; if **greater**, go to the **right child**.
4. Repeat until you find a `NULL` spot and insert the new node there.

```C
#include <stdio.h>
#include <stdlib.h>

// Define the structure for a tree node
struct Node {
    int data;
    struct Node* left;
    struct Node* right;
} TreeNode;

// Function to create a new node
TreeNode* createNode(int value) {
    struct Node* newNode = malloc(sizeof(struct Node));
    if (!newNode) {
        printf("Memory error\n");
        return NULL;
    }
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into BST
TreeNode* insert(TreeNode* root, int value) {
    if (root == NULL) {
        // Tree is empty, return new node
        return createNode(value);
    }

    if (value < root->data) {
        // Insert in the left subtree
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        // Insert in the right subtree
        root->right = insert(root->right, value);
    }
    // If value == root->data, do not insert duplicates
    return root;
}
```

#### b. Searching

**Goal**: Find whether a value exists in the BST.

**Algorithm**:

1. Start at the **root**.
2. Compare the **target value** with the current node.
3. If equal, **found**.
4. If the target is **less**, search the **left subtree**; if **greater**, search the **right subtree**.
5. Repeat until found or reach `NULL`.

```C
// Function to search a value in BST
TreeNode* search(TreeNode* root, int key) {
    if (root == NULL || root->data == key)
        return root;

    if (key < root->data)
        return search(root->left, key);
    else
        return search(root->right, key);
}

```

#### c. Traversal

Traversing a tree means visiting all its nodes in a specific order. The three common depth-first traversals are:

1. **In-Order Traversal**: Left → Root → Right
2. **Pre-Order Traversal**: Root → Left → Right
3. **Post-Order Traversal**: Left → Right → Root

```C
// In-Order Traversal
void inOrder(struct Node* root) {
    if (root != NULL) {
        inOrder(root->left);
        printf("%d ", root->data);
        inOrder(root->right);
    }
}

// Pre-Order Traversal
void preOrder(struct Node* root) {
    if (root != NULL) {
        printf("%d ", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}

// Post-Order Traversal
void postOrder(struct Node* root) {
    if (root != NULL) {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d ", root->data);
    }
}
```

**Example Usage**:

```C
int main() {
    TreeNode* root = NULL;
    root = insert(root, 8);
    insert(root, 3);
    insert(root, 10);
    insert(root, 1);
    insert(root, 6);
    insert(root, 14);
    insert(root, 4);
    insert(root, 7);
    insert(root, 13);
    
    //       8
    //		/ \
    //     3  10
    //   /  \  \
    //  1   6   14
    //	   / \   /
    //    4   7  13 

    printf("In-Order Traversal: ");
    inOrder(root); // Output: 1 3 4 6 7 8 10 13 14
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root); // Output: 8 3 1 6 4 7 10 14 13
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root); // Output: 1 4 7 6 3 13 14 10 8
    printf("\n");

    // Searching for a value
    int key = 7;
    TreeNode* found = search(root, key);
    if (found)
        printf("Value %d found in the BST.\n", key);
    else
        printf("Value %d not found in the BST.\n", key);

    return 0;
}
```

#### d. Deletion

**Goal**: Remove a node from the BST while maintaining its properties.

**Cases to Handle**:

1. **Node to be deleted is a leaf** (no children): Simply remove it.
2. **Node has one child**: Replace the node with its child.
3. **Node has two children**: Find the **in-order successor** (smallest value in the right subtree), copy its value to the node, and delete the successor.

```C
// Function to find the minimum value node in a BST
TreeNode* findMin(TreeNode* root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}

// Function to delete a node from BST
TreeNode* deleteNode(struct Node* root, int key) {
    if (root == NULL) return root;

    if (key < root->data) {
        // Key is in the left subtree
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->data) {
        // Key is in the right subtree
        root->right = deleteNode(root->right, key);
    }
    else {
        // Node found

        // Case 1: No child
        if (root->left == NULL && root->right == NULL) {
            free(root);
            root = NULL;
        }
        // Case 2: One child (right)
        else if (root->left == NULL) {
            struct Node* temp = root;
            root = root->right;
            free(temp);
        }
        // Case 2: One child (left)
        else if (root->right == NULL) {
            struct Node* temp = root;
            root = root->left;
            free(temp);
        }
        // Case 3: Two children
        else {
            // find the min value in the right tree of the node let's say the root is 7 and the child min node we find is 9
            TreeNode* temp = findMin(root->right);
            
            // change the root value (7 in this case) to the min value we find which is 9. 
            // so now we have two 9
            //    10
            //   /  \
            //  9
            //   \
            //    9 (we will call our delete func recursive on this 9)
            root->data = temp->data;
            
            // recursive on the right node
            root->right = deleteNode(root->right, temp->data);
        }
    }
    return root;
}

```



### Example Code: Comprehensive BST Implementation

#### Program Code

`practice.h`

```C
typedef struct Node
{
    int data;
    struct Node *left;
    struct Node *right;
} TreeNode;

TreeNode *createNode(int value);

TreeNode *insert(TreeNode *root, int value);
TreeNode *search(TreeNode *root, int key);
void inOrder(TreeNode *root);
void preOrder(TreeNode *root);
void postOrder(TreeNode *root);

TreeNode *findMin(TreeNode *root);
TreeNode *deleteNode(TreeNode *root, int key);
void freeTree(TreeNode *root);
```

`functions.c`

```C
TreeNode *createNode(int value)
{
    TreeNode *newNode = malloc(sizeof(TreeNode));
    if (!newNode)
    {
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}

TreeNode *insert(TreeNode *root, int value)
{
    if (root == NULL)
    {
        return createNode(value);
    }

    if (value < root->data)
    {
        root->left = insert(root->left, value);
    }
    else if (value > root->data)
    {
        root->right = insert(root->right, value);
    }
	
    // duplicate values are not inserted
    return root;
}

TreeNode *search(TreeNode *root, int key)
{
    if (root == NULL || root->data == key)
    {
        return root;
    }

    if (key < root->data)
    {
        return search(root->left, key);
    }
    else
    {
        return search(root->right, key);
    }
}

void inOrder(TreeNode *root)
{
    if (root != NULL)
    {
        inOrder(root->left);
        printf("%d ", root->data);
        inOrder(root->right);
    }
}

void preOrder(TreeNode *root)
{
    if (root != NULL)
    {
        printf("%d ", root->data);
        preOrder(root->left);
        preOrder(root->right);
    }
}

void postOrder(TreeNode *root)
{
    if (root != NULL)
    {
        postOrder(root->left);
        postOrder(root->right);
        printf("%d ", root->data);
    }
}

TreeNode *findMin(TreeNode *root)
{
    while (root->left != NULL)
    {
        root = root->left;
    }
    return root;
}
TreeNode *deleteNode(TreeNode *root, int key)
{
    if (root == NULL)
    {
        return root;
    }

    if (key < root->data)
    {
        // key is in the left subtree
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->data)
    {
        // key is in the right subtree
        root->right = deleteNode(root->right, key);
    }
    else
    {
        // Node found
        
        
        // Case 1: No child
        if (root->left == NULL && root->right == NULL)
        {
            free(root);
            root = NULL;
        }
        // Case 1: One child (right)
        else if (root->left == NULL)
        {
            TreeNode *temp = root;
            // replace the node with its child
            root = root->right;
            free(temp);
        }
        // case 2: One child (left)
        else if(root->right == NULL){
            TreeNode *temp = root;
            // replace the node with its child
            root = root->left;
            free(temp);
        }
        // case 3: Two children
        // get the inorder successor (smallest in the right subtree)
        else{
            TreeNode *temp = findMin(root->right);
            root->data = temp->data;
            
            // recursion to make sure we go thru the node to be deleted have no children
            root->right = deleteNode(root->right, temp->data);
        }
    }
    return root;
}

void freeTree(TreeNode *root)
{
    if (root == NULL)
    {
        return;
    }
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}
```

**Example Scenario of Delete Opeartions**:

Consider deleting a node with value `50` from the following BST:

```css
        50
       /  \
     30    70
     / \   / \
   20  40 60  80
```

- **Step 1**: Locate `50` (root).
- **Step 2**: `50` has two children.
- **Step 3**: Find inorder successor (`60`).
- **Step 4**: Replace `50`'s data with `60`.
- **Step 5**: Delete node `60`, which has no children.

```css
        60
       /  \
     30    70
     / \     \
   20 40     80
```

- The `deleteNode` function efficiently removes a specified node from a BST while maintaining the tree's structural properties. 
- By handling different cases based on the node's children and utilizing the inorder successor for nodes with two children, the function ensures that the BST remains valid after deletion.

`practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(1);
    root = insert(root, 50);
    insert(root, 30);
    insert(root, 20);
    insert(root, 40);
    insert(root, 70);
    insert(root, 60);
    insert(root, 80);

    printf("In-Order Traversal: ");
    inOrder(root); // 20 30 40 50 60 70 80
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root);
    printf("\n");

    // Search for a value
    int key = 40;
    TreeNode *found = search(root, key);
    if (found)
    {
        printf("Value %d found in the BST.\n", key);
    }
    else
    {
        printf("Value %d not found in the BST.\n", key);
    }

    // Delete a node
    root = deleteNode(root, 20);
    root = deleteNode(root, 30);
    root = deleteNode(root, 50);

    printf("In-Order Traversal after deletion:");
    inOrder(root);
    printf("\n");

    printf("Pre-Order Traversal after deletion:");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal after deletion:");
    postOrder(root);
    printf("\n");

    freeTree(root);

    return 0;
}
```

**Visual Representation of BST Operations**:

**Building the BST**

**Insertion Order:** 1, 50, 30, 20, 40, 70, 60, 80

#### **Step-by-Step Insertion**

1. **Insert 1**

   ```css
   1
   ```

   

2. **Insert 50**

   ```css
     1
      \
      50
   ```

3. **Insert 30**

   ```css
     1
      \
      50
     /
    30
   ```

4. **Insert 20**

   ```css
      1
       \
       50
      /
     30
    /
   20
   ```

   

5. **Insert 40**

   ```css
      1
       \
       50
      /
     30
    /  \
   20  40
   ```

   

6. **Insert 70**

   ```css
      1
       \
       50
      /  \
     30   70
    /  \
   20  40
   ```

   

7. **Insert 60**

   ```css
      1
       \
       50
      /  \
     30   70
    /  \  /
   20  40 60
   ```

   

8. **Insert 80**

   ```css
        1
         \
         50
        /  \
       30   70
      /  \  / \
     20  40 60 80
   ```

**Traversals Before Deletion**:

1. In-Order Traversal (Left, Root, Right)

   ```css
   1 20 30 40 50 60 70 80
   ```

**Visualization**:

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**In-Order Traversal Steps:**

1. Visit left subtree of 1 (None)
2. Visit 1
3. Visit left subtree of 50:
   - Visit left subtree of 30:
     - Visit left subtree of 20 (None)
     - Visit 20
     - Visit right subtree of 20 (None)
   - Visit 30
   - Visit right subtree of 30:
     - Visit left subtree of 40 (None)
     - Visit 40
     - Visit right subtree of 40 (None)
4. Visit 50
5. Visit right subtree of 50:
   - Visit left subtree of 70:
     - Visit left subtree of 60 (None)
     - Visit 60
     - Visit right subtree of 60 (None)
   - Visit 70
   - Visit right subtree of 70:
     - Visit left subtree of 80 (None)
     - Visit 80
     - Visit right subtree of 80 (None)

**Pre-Order Traversal (Root, Left, Right)**:

```css
1 50 30 20 40 70 60 80
```

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**Pre-Order Traversal Steps:**

1. Visit 1
2. Visit 50
3. Visit 30
4. Visit 20
5. Visit 40
6. Visit 70
7. Visit 60
8. Visit 80



**Post-Order Traversal (Left, Right, Root)**

```css
20 40 30 60 80 70 50 1
```

```css
      1
       \
       50
      /  \
     30   70
    /  \  / \
   20  40 60 80
```

**Post-Order Traversal Steps:**

1. Visit left subtree of 1 (None)
2. Visit right subtree of 1:
   - Visit left subtree of 50:
     - Visit left subtree of 30:
       - Visit left subtree of 20 (None)
       - Visit right subtree of 20 (None)
       - Visit 20
     - Visit right subtree of 30:
       - Visit left subtree of 40 (None)
       - Visit right subtree of 40 (None)
       - Visit 40
     - Visit 30
   - Visit right subtree of 50:
     - Visit left subtree of 70:
       - Visit left subtree of 60 (None)
       - Visit right subtree of 60 (None)
       - Visit 60
     - Visit right subtree of 70:
       - Visit left subtree of 80 (None)
       - Visit right subtree of 80 (None)
       - Visit 80
     - Visit 70
   - Visit 50
3. Visit 1

**Searching for a Value**:

**Search Key:** 40

**Search Path:**

1. Start at root (1)
2. 40 > 1 → Move to right child (50)
3. 40 < 50 → Move to left child (30)
4. 40 > 30 → Move to right child (40)
5. **Found 40**

**Deletions**:

**Nodes to Delete:** 20, 30, 50

**Delete 20**

**Original Tree:**

```css
        1
         \
         50
        /  \
       30   70
      /  \  / \
     20  40 60 80
```

**Deletion Process:**

- **Node 20** is a **leaf node** (no children).
- Remove node 20 directly.

**Tree After Deleting 20:**

```css
    1
     \
     50
    /  \
   30   70
     \  / \
     40 60 80
```

**Delete 30**

**Original Tree:**

```css
        1
         \
         50
        /  \
       30   70
         \  / \
         40 60 80
```

**Deletion Process:**

- **Node 30** has **one child** (40).
- Replace node 30 with its child (40).

**Tree After Deleting 30:**

```css
        1
         \
         50
        /  \
       40   70
           / \
          60 80
```

**Delete 50**

**Original Tree:**

```css
        1
         \
         50
        /  \
       40   70
           / \
          60 80
```

**Deletion Process:**

- **Node 50** has **two children** (40 and 70).
- Find **Inorder Successor** (smallest node in the right subtree): **60**.
- Replace node 50's data with 60.
- Delete node 60 (which is now a leaf node).

**Tree After Deleting 50:**

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Traversals After Deletions**:

**In-Order Traversal**

```css
1 40 60 70 80
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Pre-Order Traversal**

```css
1 60 40 70 80
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Post-Order Traversal**

```css
40 80 70 60 1
```

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

**Final Tree Structure**

```css
        1
         \
         60
        /  \
       40   70
             \
             80
```

#### Explanation of `FreeTree()`

Consider the following BST:

```css
        50
       /  \
     30    70
    / \    / \
  20  40  60 80
```

- **Node Values:** 50 (root), 30, 70, 20, 40, 60, 80

Let's walk through how `freeTree` deallocates each node in the BST using **Post-Order Traversal**.

**a. Initial Call**

- **Function Call:** `freeTree(root)` where `root` points to node `50`.

**b. Processing the Left Subtree of 50**

1. **Call:** `freeTree(30)`
2. Call: `freeTree(20)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(20)` → Node `20` is deallocated.
3. Call: `freeTree(40)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(40)` → Node `40` is deallocated.
4. **Action:** `free(30)` → Node `30` is deallocated.

**State of the Tree**:

```css
        50
          \
           70
          / \
        60 80
```

**Processing the Right Subtree of 50**

1. **Call:** `freeTree(70)`
2. Call: `freeTree(60)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(60)` → Node `60` is deallocated.
3. Call: `freeTree(80)`
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Call:** `freeTree(NULL)` → Returns immediately (no action).
   - **Action:** `free(80)` → Node `80` is deallocated.
4. **Action:** `free(70)` → Node `70` is deallocated.

**State of the Tree:**

```css
        50
```

**Final Action on Root**

- **Action:** `free(50)` → Node `50` is deallocated.

**State of the Tree:**

```css
    (Empty Tree)
```

**Visual Representation of the Deallocation Process**

```css
freeTree(50)
├─ freeTree(30)
│  ├─ freeTree(20)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ freeTree(NULL)
│  ├─ Action: free(20)
│  ├─ freeTree(40)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(40)
│  └─ Action: free(30)
├─ freeTree(70)
│  ├─ freeTree(60)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(60)
│  ├─ freeTree(80)
│  │  ├─ freeTree(NULL)
│  │  └─ freeTree(NULL)
│  │     └─ Returns
│  ├─ Action: free(80)
│  └─ Action: free(70)
└─ Action: free(50)
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==32744== Memcheck, a memory error detector
==32744== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==32744== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==32744== Command: ./practice
==32744== 
In-Order Traversal: 1 20 30 40 50 60 70 80 
Pre-Order Traversal: 1 50 30 20 40 70 60 80 
Post-Order Traversal: 20 40 30 60 80 70 50 1 
Value 40 found in the BST.
In-Order Traversal after deletion:1 40 60 70 80 
Pre-Order Traversal after deletion:1 60 40 70 80 
Post-Order Traversal after deletion:40 80 70 60 1 
==32744== 
==32744== HEAP SUMMARY:
==32744==     in use at exit: 0 bytes in 0 blocks
==32744==   total heap usage: 9 allocs, 9 frees, 1,216 bytes allocated
==32744== 
==32744== All heap blocks were freed -- no leaks are possible
==32744== 
==32744== For lists of detected and suppressed errors, rerun with: -s
==32744== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



#### Adding Height to our program

```C
int treeHeight(TreeNode *root){
    if(root == NULL){
        return -1; // height of empty tree is -1
    }
    
    int leftHeight = treeHieght(root->left);
    int rightHeight = treeHeight(root->right);
    return (leftHeight > rightHeight ? leftHeight : rightHeight) + 1;
}
```

In `practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(50);
    insert(root, 30);
    insert(root, 20);
    insert(root, 40);
    insert(root, 70);
    insert(root, 60);
    insert(root, 80);
    root = insert(root, 1);

    printf("In-Order Traversal: ");
    inOrder(root); // 20 30 40 50 60 70 80
    printf("\n");

    printf("Pre-Order Traversal: ");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal: ");
    postOrder(root);
    printf("\n");

    // Search for a value
    int key = 40;
    TreeNode *found = search(root, key);
    if (found)
    {
        printf("Value %d found in the BST.\n", key);
    }
    else
    {
        printf("Value %d not found in the BST.\n", key);
    }

    int height = treeHeight(root);
    printf("Height of the tree: %d\n", height);

    // Delete a node
    root = deleteNode(root, 20);
    root = deleteNode(root, 30);
    root = deleteNode(root, 50);

    printf("In-Order Traversal after deletion:");
    inOrder(root);
    printf("\n");

    printf("Pre-Order Traversal after deletion:");
    preOrder(root);
    printf("\n");

    printf("Post-Order Traversal after deletion:");
    postOrder(root);
    printf("\n");

    height = treeHeight(root);
    printf("Height of the tree after deletion: %d\n", height);

    freeTree(root);

    return 0;
}
```

`Output`

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==12706== Memcheck, a memory error detector
==12706== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==12706== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==12706== Command: ./practice
==12706== 
In-Order Traversal: 1 20 30 40 50 60 70 80 
Pre-Order Traversal: 50 30 20 1 40 70 60 80 
Post-Order Traversal: 1 20 40 30 60 80 70 50 
Value 40 found in the BST.
Height of the tree: 3
In-Order Traversal after deletion:1 40 60 70 80 
Pre-Order Traversal after deletion:60 40 1 70 80 
Post-Order Traversal after deletion:1 40 80 70 60 
Height of the tree after deletion: 2
==12706== 
==12706== HEAP SUMMARY:
==12706==     in use at exit: 0 bytes in 0 blocks
==12706==   total heap usage: 9 allocs, 9 frees, 1,216 bytes allocated
==12706== 
==12706== All heap blocks were freed -- no leaks are possible
==12706== 
==12706== For lists of detected and suppressed errors, rerun with: -s
==12706== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



#### Additional Concepts

- The **size** of a tree is the number of nodes: a leaf by itself has size 1.

##### Height of the Tree

- The height of a tree is the number of edges on the longest path from the root to a leaf.
- 0 for a leaf, at least one in any larger tree.
- The **depth** of a node is the length of the path from the root to that node.
- The **height** of a node is the height of the subtree of which it is the root, i.e, the length of the longest path from that node to some leaf below it.
- A node `u` is an **ancestor** of a node `v` if `v` is contained in the subtree rooted at `u`.
  - We may write equivalently that `v` is a descendant of `u`.
- Every node is both an ancestor and a descendent of itself.

##### Key Takeways of BST:

- **BST Properties**: Left child < parent < right child.
- **Efficiency**: BST operations (search, insert, delete) have an average time complexity of **O(log n)**, but can degrade to **O(n)** if the tree becomes unbalanced.
- **Traversal Methods**: Essential for accessing and processing all nodes in the tree.
- **Recursive Thinking**: Many tree operations are naturally implemented using recursion.