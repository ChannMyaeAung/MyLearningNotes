### Red-Black Tree Implementation in C

Implementing a Red-Black Tree in C is more involved than standard BSTs due to the need to maintain additional properties. Below is a simplified implementation focusing on **insertion** and **searching**. **Deletion** is more complex.

#### Program Code

`practice.h`

```C
typedef enum
{
    RED,
    BLACK
} Color;

typedef struct RBNode
{
    int data;
    Color color;
    struct RBNode *left, *right, *parent;
} RBNode;

RBNode *createNode(int data);
void leftRotate(RBNode **root, RBNode *x);
void rightRotate(RBNode **root, RBNode *y);

void fixViolation(RBNode **root, RBNode *z);

void insertRB(RBNode **root, int data);

RBNode *searchRB(RBNode *root, int data);

void inorderTraversal(RBNode *root);

void printInOrder(RBNode *root);

void freeTree(RBNode *root);
```

`functions.c`

```C
// Function to create a new Red-Black Tree node
RBNode* createNode(int data) {
    RBNode* newNode = (RBNode*)malloc(sizeof(RBNode));
    if (!newNode) {
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->color = RED; // New nodes are initially red
    newNode->left = newNode->right = newNode->parent = NULL;
    return newNode;
}

// Function to perform a left rotation
void leftRotate(RBNode** root, RBNode* x) {
    RBNode* y = x->right;
    x->right = y->left;
    if (y->left != NULL)
        y->left->parent = x;
    
    y->parent = x->parent;
    if (x->parent == NULL)
        *root = y;
    else if (x == x->parent->left)
        x->parent->left = y;
    else
        x->parent->right = y;
    
    y->left = x;
    x->parent = y;
}

// Function to perform a right rotation
void rightRotate(RBNode** root, RBNode* y) {
    RBNode* x = y->left;
    y->left = x->right;
    if (x->right != NULL)
        x->right->parent = y;
    
    x->parent = y->parent;
    if (y->parent == NULL)
        *root = x;
    else if (y == y->parent->left)
        y->parent->left = x;
    else
        y->parent->right = x;
    
    x->right = y;
    y->parent = x;
}

// Function to fix violations after insertion
void fixViolation(RBNode** root, RBNode* z) {
    while (z->parent != NULL && z->parent->color == RED) {
        RBNode* parent = z->parent;
        RBNode* grandparent = z->parent->parent;
        
        // Case 1: Parent is left child of grandparent
        if (parent == grandparent->left) {
            RBNode* uncle = grandparent->right;
            
            // Case 1a: Uncle is red
            if (uncle != NULL && uncle->color == RED) {
                grandparent->color = RED;
                parent->color = BLACK;
                uncle->color = BLACK;
                z = grandparent;
            }
            // Case 1b: Uncle is black or NULL
            else {
                if (z == parent->right) {
                    leftRotate(root, parent);
                    z = parent;
                    parent = z->parent;
                }
                rightRotate(root, grandparent);
                // Swap colors
                parent->color = BLACK;
                grandparent->color = RED;
            }
        }
        // Case 2: Parent is right child of grandparent
        else {
            RBNode* uncle = grandparent->left;
            
            // Case 2a: Uncle is red
            if (uncle != NULL && uncle->color == RED) {
                grandparent->color = RED;
                parent->color = BLACK;
                uncle->color = BLACK;
                z = grandparent;
            }
            // Case 2b: Uncle is black or NULL
            else {
                if (z == parent->left) {
                    rightRotate(root, parent);
                    z = parent;
                    parent = z->parent;
                }
                leftRotate(root, grandparent);
                // Swap colors
                parent->color = BLACK;
                grandparent->color = RED;
            }
        }
    }
    (*root)->color = BLACK; // Ensure root is always black
}

// Function to insert a node into the Red-Black Tree
void insertRB(RBNode** root, int data) {
    RBNode* z = createNode(data);
    RBNode* y = NULL;
    RBNode* x = *root;
    
    // Standard BST insertion
    while (x != NULL) {
        y = x;
        if (z->data < x->data)
            x = x->left;
        else if (z->data > x->data)
            x = x->right;
        else {
            printf("Duplicate value %d not inserted.\n", data);
            free(z);
            return;
        }
    }
    
    z->parent = y;
    if (y == NULL)
        *root = z; // Tree was empty
    else if (z->data < y->data)
        y->left = z;
    else
        y->right = z;
    
    // Fix Red-Black Tree properties
    fixViolation(root, z);
}

// Function to search for a value in the Red-Black Tree
RBNode* searchRB(RBNode* root, int data) {
    while (root != NULL) {
        if (data < root->data)
            root = root->left;
        else if (data > root->data)
            root = root->right;
        else
            return root; // Found
    }
    return NULL; // Not found
}

// Function for in-order traversal to display the tree
void inorderTraversal(RBNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d(%s) ", root->data, root->color == RED ? "R" : "B");
        inorderTraversal(root->right);
    }
}

// Function to print the Red-Black Tree in in-order
void printInOrder(RBNode* root) {
    printf("In-order Traversal: ");
    inorderTraversal(root);
    printf("\n");
}

void freeTree(RBNode *root)
{
    if (root != NULL)
    {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}
```

**Code Breakdown:**

1. **Enumerations and Structures:**
   - `Color`: Enumerated type to represent node colors (`RED` or `BLACK`).
   - `RBNode`: Structure representing a node in the Red-Black Tree, containing:
     - `data`: The integer value stored.
     - `color`: The color of the node.
     - `left`, `right`, `parent`: Pointers to child nodes and parent node.
2. **Node Creation:**
   - `createNode`: Allocates memory for a new node, initializes its data, sets color to `RED`, and initializes child and parent pointers to `NULL`.
3. **Rotations:**
   - Left Rotate (`leftRotate`):
     - Rotates the subtree to the left, adjusting pointers accordingly.
   - Right Rotate (`rightRotate`):
     - Rotates the subtree to the right, adjusting pointers accordingly.
4. **Fixing Violations (`fixViolation`):**
   - After insertion, the tree might violate Red-Black properties.
   - The function corrects these violations by performing rotations and recoloring.
   - It handles cases based on the color of the node's uncle and the relative positions of the nodes.
5. **Insertion (`insertRB`):**
   - Inserts a new node into the tree following standard BST insertion.
   - After insertion, calls `fixViolation` to maintain Red-Black properties.
   - Prevents insertion of duplicate values.
6. **Searching (`searchRB`):**
   - Standard BST search for a given value.
   - Returns the node if found; otherwise, returns `NULL`.
7. **Traversal (`inorderTraversal` & `printInOrder`):**
   - Performs an in-order traversal of the tree.
   - Displays each node's data along with its color (`R` for Red, `B` for Black).

`practice.c`

```C
int main()
{
    RBNode *root = NULL;

    int values[] = {10, 20, 30, 15, 25, 5, 1};
    int n = sizeof(values) / sizeof(values[0]);

    printf("Inserting values: ");
    for (int i = 0; i < n; i++)
    {
        printf("%d ", values[i]);
        insertRB(&root, values[i]);
    }
    printf("\n");
	
    
    // display in-order traversal
    printInOrder(root);

    // Search for a value
    int key = 15;
    RBNode *found = searchRB(root, key);
    if (found)
    {
        printf("Value %d found in the Red-Black Tree. Color: %s\n", key, found->color == RED ? "Red" : "Black");
    }
    else
    {
        printf("Value %d not found in the Red-Black Tree.\n", key);
    }

    // Search for a non-existing value
    key = 100;
    found = searchRB(root, key);

    if (found)
    {
        printf("Value %d found in the Red-Black Tree. Color: %s\n", key, found->color == RED ? "Red" : "Black");
    }
    else
    {
        printf("Value %d not found in the Red-Black Tree.\n", key);
    }

    freeTree(root);
    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==39468== Memcheck, a memory error detector
==39468== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==39468== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==39468== Command: ./practice
==39468== 
Inserting values: 10 20 30 15 25 5 1 
In-order Traversal: 1(R) 5(B) 10(R) 15(B) 20(B) 25(R) 30(B) 
Value 15 found in the Red-Black Tree. Color: Black
Value 100 not found in the Red-Black Tree.
==39468== 
==39468== HEAP SUMMARY:
==39468==     in use at exit: 0 bytes in 0 blocks
==39468==   total heap usage: 8 allocs, 8 frees, 1,248 bytes allocated
==39468== 
==39468== All heap blocks were freed -- no leaks are possible
==39468== 
==39468== For lists of detected and suppressed errors, rerun with: -s
==39468== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
chan@CMA:~/C_Programming/practice$ 
```

#### Visual Representation of All Rotations in RB Tree

![Screenshot 2024-12-22 193804](C:\Users\User\Pictures\Screenshots\Screenshot 2024-12-22 193804.png)

![Screenshot 2024-12-22 193755](C:\Users\User\Pictures\Screenshots\Screenshot 2024-12-22 193755.png)

![Screenshot 2024-12-22 193743](C:\Users\User\Pictures\Screenshots\Screenshot 2024-12-22 193743.png)

![Screenshot 2024-12-22 193735](C:\Users\User\Pictures\Screenshots\Screenshot 2024-12-22 193735.png)

#### Visual Representation

After inserting the values `{10, 20, 30, 15, 25, 5, 1}`, the Red-Black Tree maintains its properties. Here's a possible representation:

```css
           15(B)
         /       \
      5(R)      25(R)
     /   \       /    \
   1(B) 10(B) 20(B) 30(B)
```

This structure satisfies all Red-Black Tree properties:

1. **Root is Black.**
2. **Red nodes have Black children.**
3. **All paths from root to leaves have the same number of Black nodes.**

#### Implementing Deletion in Red-Black Trees

`functions.c`

```C
// Function to replace subtree u with subtree v
static void transplant(RBNode **root, RBNode *u, RBNode *v)
{
    if (u->parent == NULL)
    {
        // If u is root, update root to v
        *root = v;
    }
    else if (u == u->parent->left)
    {
        // If u is left child, set parent's left child to v
        u->parent->left = v;
    }
    else
    {
        // If u is right child, set parent's right child to v
        u->parent->right = v;
    }
    if (v != NULL)
    {
        // Update parent pointer of v
        v->parent = u->parent;
    }
}

// Function to find the node with the minimum value in a subtree
static RBNode *minimum(RBNode *node)
{
    while (node->left != NULL)
    {
        node = node->left;
    }
    return node;
}

// Function to fix Red-Black Tree properties after deletion
static void deleteFixup(RBNode **root, RBNode *x)
{
    while (x != *root && (x == NULL || x->color == BLACK))
    {
        if (x == x->parent->left)
        {
            RBNode *w = x->parent->right; // Sibling of x
            if (w && w->color == RED)
            {
                // Case 1: Sibling w is red
                w->color = BLACK;
                x->parent->color = RED;
                leftRotate(root, x->parent);
                w = x->parent->right;
            }
            if ((w->left == NULL || w->left->color == BLACK) &&
                (w->right == NULL || w->right->color == BLACK))
            {
                // Case 2: Both of w's children are black
                if (w)
                    w->color = RED;
                x = x->parent;
            }
            else
            {
                if (w->right == NULL || w->right->color == BLACK)
                {
                    // Case 3: w's right child is black, left child is red
                    if (w->left)
                        w->left->color = BLACK;
                    w->color = RED;
                    rightRotate(root, w);
                    w = x->parent->right;
                }
                // Case 4: w's right child is red
                if (w)
                {
                    w->color = x->parent->color;
                    x->parent->color = BLACK;
                    if (w->right)
                        w->right->color = BLACK;
                    leftRotate(root, x->parent);
                }
                x = *root;
            }
        }
        else
        {
            RBNode *w = x->parent->left; // Sibling of x
            if (w && w->color == RED)
            {
                // Case 1: Sibling w is red
                w->color = BLACK;
                x->parent->color = RED;
                rightRotate(root, x->parent);
                w = x->parent->left;
            }
            if ((w->right == NULL || w->right->color == BLACK) &&
                (w->left == NULL || w->left->color == BLACK))
            {
                // Case 2: Both of w's children are black
                if (w)
                    w->color = RED;
                x = x->parent;
            }
            else
            {
                if (w->left == NULL || w->left->color == BLACK)
                {
                    // Case 3: w's left child is black, right child is red
                    if (w->right)
                        w->right->color = BLACK;
                    w->color = RED;
                    leftRotate(root, w);
                    w = x->parent->left;
                }
                // Case 4: w's left child is red
                if (w)
                {
                    w->color = x->parent->color;
                    x->parent->color = BLACK;
                    if (w->left)
                        w->left->color = BLACK;
                    rightRotate(root, x->parent);
                }
                x = *root;
            }
        }
    }
    if (x != NULL)
        x->color = BLACK;
}

// Function to delete a node with given data from the Red-Black Tree
void deleteRB(RBNode **root, int data)
{
    // Find the node to be deleted
    RBNode *z = searchRB(*root, data);
    if (z == NULL)
    {
        // Node not found
        printf("Value %d not found in the Red-Black Tree.\n", data);
        return;
    }

    RBNode *y = z;                 // Node to be removed or moved
    int yOriginalColor = y->color; // Store original color of y
    RBNode *x = NULL;              // Child of y that will be moved

    if (z->left == NULL)
    {
        // Case 1: z has no left child
        x = z->right;
        transplant(root, z, z->right);
    }
    else if (z->right == NULL)
    {
        // Case 2: z has no right child
        x = z->left;
        transplant(root, z, z->left);
    }
    else
    {
        // Case 3: z has two children
        y = minimum(z->right);     // Find z's in-order successor
        yOriginalColor = y->color; // Store original color of y
        x = y->right;              // x is y's right child

        if (y->parent == z)
        {
            // If y is z's direct child
            if (x != NULL)
                x->parent = y;
        }
        else
        {
            // Replace y with its right child
            transplant(root, y, y->right);
            y->right = z->right;
            if (y->right != NULL)
                y->right->parent = y;
        }
        // Replace z with y
        transplant(root, z, y);
        y->left = z->left;
        if (y->left != NULL)
            y->left->parent = y;
        y->color = z->color; // Preserve y's color
    }

    // Free the memory of the deleted node
    free(z);

    if (yOriginalColor == BLACK)
    {
        // If the removed node was black, fix the tree
        deleteFixup(root, x);
    }
}
```

#### Final Program Output with Delete function

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==30495== Memcheck, a memory error detector
==30495== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==30495== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==30495== Command: ./practice
==30495== 
Inserting values: 10 20 30 15 25 5 1 
In-order Traversal: 1(R) 5(B) 10(R) 15(B) 20(B) 25(R) 30(B) 
Value 15 found in the Red-Black Tree. Color: Black
Value 100 not found in the Red-Black Tree.
Deleting value 20
In-order Traversal: 1(R) 5(B) 10(R) 15(B) 25(B) 30(B) 
==30495== 
==30495== HEAP SUMMARY:
==30495==     in use at exit: 0 bytes in 0 blocks
==30495==   total heap usage: 8 allocs, 8 frees, 1,248 bytes allocated
==30495== 
==30495== All heap blocks were freed -- no leaks are possible
==30495== 
==30495== For lists of detected and suppressed errors, rerun with: -s
==30495== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

#### Red-Black Tree Diagram

Before Deletion of 20

```css
        20(B)
       /     \
    10(R)    25(R)
    /   \       \
  5(B) 15(B)  30(B)
 /
1(R)
```

After Deletion of 20

```css
        25(B)
       /     \
    10(R)    30(B)
    /   \
  5(B) 15(B)
 /
1(R)
```



### Key Takeaways

**Red-Black Trees:**

- A self-balancing binary search tree ensuring efficient operations.
- Maintains balance through node coloring and rotations.
- Guarantees **O(log n)** time complexity for insertion, deletion, and searching.
- Widely used in standard libraries and applications requiring ordered data structures.