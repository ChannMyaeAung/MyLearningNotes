## Trees

A **tree** is a way to organize data hierarchically. Think of it like a family tree:

- **Root**: The top person in the family.
- **Nodes**: Each person (including the root).
- **Edges**: The connections between people (like parent to child).

```css
      Root
     /    \
  Child1  Child2
   /  \
Grand1 Grand2

```

### Why Use Trees?

- **Organize Data**: Like folders in our computer.
- **Fast Searching**: Quickly find items.
- **Hierarchical Relationships**: Represent parent-child relationships.

### Understanding Tree Terminology

- **Node**: An individual element of a tree containing data.
- **Root**: The topmost node of a tree, with no parent.
- **Internal Node**: A node that has one or more children.
- **Leaf (or External Node)**: A node with no children.
- **Subtree**: A tree entirely contained within another tree, rooted at a specific node.
- **Depth/Height**: The level of a node within the tree; the root has depth 0, its children depth 1, and so on.

### Basic Tree Implementation in C

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
void inOrder(TreeNode *root);

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
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// in-order traversal: left, root, right
void inOrder(TreeNode *root)
{
    if (root != NULL)
    {
        inOrder(root->left); // visit left child
        printf("%d ", root->data); // visit node itself
        inOrder(root->right); // visit right child
    }
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

`practice.c`

```C
int main(int argc, char *argv[])
{

    TreeNode *root = createNode(1);
    root->left = createNode(2);
    root->right = createNode(3);
    root->left->left = createNode(4);
    root->left->right = createNode(5);
    
    // Now the tree looks like this:
    //       1
    //     /   \
    //    2     3
    //   / \
    //  4   5

    // traverse and print the tree
    inOrder(root); // output: 4 2 5 1 3
    printf("\n");
    freeTree(root);
    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==23562== Memcheck, a memory error detector
==23562== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==23562== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==23562== Command: ./practice
==23562== 
4 2 5 1 3 
==23562== 
==23562== HEAP SUMMARY:
==23562==     in use at exit: 0 bytes in 0 blocks
==23562==   total heap usage: 6 allocs, 6 frees, 1,144 bytes allocated
==23562== 
==23562== All heap blocks were freed -- no leaks are possible
==23562== 
==23562== For lists of detected and suppressed errors, rerun with: -s
==23562== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

### Key Points to Remember

- **Node Structure**: Each node holds data and pointers to its children.
- **Root**: The starting point of the tree.
- **Pointers**: Used to connect nodes (like links).
- **Traversal**: Ways to visit all nodes (in-order, pre-order, post-order).
  - **in-order:** left, root, right
  - **pre-order:** root, left, right
  - **post-order:** left, right, root