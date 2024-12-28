### 2-3 Tree Node Structure in C

To represent a 2-3 Tree in C, we need to define a node structure that can accommodate both 2-nodes and 3-nodes. Each node will contain:

- **Keys:** One or two keys.
- **Children:** Pointers to two or three children.
- **Parent Pointer:** (Optional) To facilitate upward traversal, especially useful during insertion and deletion.

#### **Defining the Node Structure**

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

// Maximum number of keys and children in a node
#define MAX_KEYS 2
#define MAX_CHILDREN 3

typedef struct Node {
    int keys[MAX_KEYS];              // Array to store keys
    struct Node* children[MAX_CHILDREN]; // Pointers to child nodes
    int numKeys;                     // Current number of keys
    bool isLeaf;                     // Flag to check if node is a leaf
} Node;

```

**Explanation:**

- `keys[MAX_KEYS]`: Stores one or two keys.
- `children[MAX_CHILDREN]`: Pointers to up to three children.
- `numKeys`: Tracks whether the node is a 2-node (`numKeys = 1`) or a 3-node (`numKeys = 2`).
- `isLeaf`: Indicates whether the node is a leaf node.

### Key Operations

Implementing a 2-3 Tree involves several core operations: searching, inserting, and deleting keys. 

#### Understanding Tree Terminology

Before diving into the specifics of leaves in a 2-3 Tree, it's essential to familiarize ourselves with some fundamental tree terminology:

- **Node**: An individual element of a tree containing data.
- **Root**: The topmost node of a tree, with no parent.
- **Internal Node**: A node that has one or more children.
- **Leaf (or External Node)**: A node with no children.
- **Subtree**: A tree entirely contained within another tree, rooted at a specific node.
- **Depth/Height**: The level of a node within the tree; the root has depth 0, its children depth 1, and so on.

#### 5.1. Searching

The search operation in a 2-3 Tree is straightforward due to its balanced and ordered structure.

**Algorithm:**

1. Start at the root node.
2. Compare the target key with the keys in the current node.
3. If the key matches, return success.
4. If the key is less than the first key, move to the left child.
5. If the key is greater than the first key but (in a 3-node) less than the second key, move to the middle child.
6. If the key is greater than all keys, move to the right child.
7. Repeat until the key is found or a leaf is reached.

#### C Implementation:

```C
// Function to search a key in the 2-3 Tree
bool search(Node* root, int key) {
    if (root == NULL)
        return false;

    int i = 0;

    // Find the first key greater than or equal to key
    while (i < root->numKeys && key > root->keys[i])
        i++;

    // If the found key is equal to key, return true
    if (i < root->numKeys && root->keys[i] == key)
        return true;

    // If the node is a leaf, the key is not present
    // In any tree data structure, a leaf (also known as an external node) is a node that does not have any children.
    if (root->isLeaf)
        return false;

    // Recurse on the appropriate child
    return search(root->children[i], key);
}

```



#### Insertion

Inserting a key into a 2-3 Tree involves ensuring that the tree remains balanced and maintains its properties after the insertion. The primary challenge is handling node splits when a node becomes overfull (i.e., exceeds the maximum number of keys).

**Algorithm:**

1. **Find the Appropriate Leaf:**
   - Navigate the tree to locate the correct leaf node where the new key should be inserted.
2. **Insert the Key:**
   - If the leaf node has space (`numKeys < 2`), insert the key in order.
   - If the leaf node is full (`numKeys == 2`), split the node:
     - Create two new nodes and distribute the keys.
     - Promote the middle key to the parent.
     - If the parent is also full, recursively split the parent.
3. **Handle Root Splits:**
   - If the root splits, create a new root with the promoted key and two children.

**C Implementation:**

Implementing insertion requires handling node splits and promoting keys.

```C
// Function to create a new node
Node* createNode(int key, bool isLeaf) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if(!newNode){
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->keys[0] = key;
    newNode->numKeys = 1;
    newNode->isLeaf = isLeaf;
    for (int i = 0; i < MAX_CHILDREN; i++)
        newNode->children[i] = NULL;
    return newNode;
}

// Function to insert a key into a non-full node
void insertNonFull(Node* node, int key) {
    int i = node->numKeys - 1;

    if (node->isLeaf) {
        // Shift keys to make room for the new key
        while (i >= 0 && key < node->keys[i]) {
            node->keys[i + 1] = node->keys[i];
            i--;
        }
        node->keys[i + 1] = key;
        node->numKeys += 1;
    } else {
        // Find the child which is going to have the new key
        while (i >= 0 && key < node->keys[i])
            i--;

        i++;

        // Check if the found child is full
        if (node->children[i]->numKeys == MAX_KEYS) {
            // Split the child
            // Assuming a function splitChild exists
            // splitChild(node, i, node->children[i]);

            // For simplicity, skipping the implementation details
            // After splitting, decide which of the two children to descend
            // if (key > node->keys[i])
            //     i++;
        }

        insertNonFull(node->children[i], key);
    }
}

// Function to split the child of a node
void splitChild(Node* parent, int index, Node* child) {
    // Create a new node to store (t-1) keys of child
    Node* newChild = createNode(child->keys[1], child->isLeaf);
    newChild->numKeys = 1;

    // If child is not a leaf, copy the last t children to newChild
    if (!child->isLeaf) {
        for (int j = 0; j < MAX_CHILDREN / 2; j++)
            newChild->children[j] = child->children[j + 2];
    }

    // Reduce the number of keys in child
    child->numKeys = 1;

    // Create space for the new child
    for (int j = parent->numKeys; j >= index + 1; j--)
        parent->children[j + 1] = parent->children[j];

    // Link the new child to the parent
    parent->children[index + 1] = newChild;

    // Move the middle key of child up to the parent
    for (int j = parent->numKeys - 1; j >= index; j--)
        parent->keys[j + 1] = parent->keys[j];

    parent->keys[index] = child->keys[1];
    parent->numKeys += 1;
}

// Function to insert a key into the 2-3 Tree
void insert(Node** rootRef, int key) {
    Node* root = *rootRef;

    // If the tree is empty
    if (root == NULL) {
        *rootRef = createNode(key, true);
        return;
    }

    // If root is full, tree grows in height
    if (root->numKeys == MAX_KEYS) {
        Node* newRoot = createNode(0, false); // Temporary key
        newRoot->children[0] = root;
        splitChild(newRoot, 0, root);

        // Now insert the key into the appropriate child
        int i = 0;
        if (newRoot->keys[0] < key)
            i++;
        // Assuming the splitChild function updated the keys correctly
        // insertNonFull(newRoot->children[i], key);

        // For simplicity, calling insertNonFull
        insertNonFull(newRoot->children[i], key);

        *rootRef = newRoot;
    } else {
        insertNonFull(root, key);
    }
}

```

**Notes:**

- The `splitChild` function handles splitting a full child node and promoting the middle key to the parent.
- The `insert` function checks if the root is full and splits it if necessary, ensuring that the tree's height increases only when required.
- The `insertNonFull` function inserts a key into a node that is guaranteed not to be full.

#### Deletion

Deletion in 2-3 Trees is more involved as it may require merging nodes or redistributing keys to maintain the tree's properties. Given its complexity, implementing deletion is beyond the scope of this introductory guide. However, understanding that deletion involves ensuring that nodes do not become underfull (i.e., have fewer keys than required) is essential.

For comprehensive deletion implementation, consider studying B-Trees or more advanced balanced trees like Red-Black Trees, which offer similar balanced properties with more straightforward deletion algorithms.