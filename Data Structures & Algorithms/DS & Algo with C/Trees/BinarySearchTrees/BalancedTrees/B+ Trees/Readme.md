## Understanding B⁺ Trees

A B⁺ Tree is an extension of the B-Tree data structure. While both are balanced and multi-way trees, B⁺ Trees differ in how they store data and manage their nodes. Specifically:

- **All data records are stored at the leaf nodes.**
- **Internal nodes only store keys and child pointers.**
- **Leaf nodes are linked together in a linked list to facilitate range queries and ordered traversals.**

These characteristics make B⁺ Trees particularly suitable for scenarios requiring efficient range queries and sequential access.

------

## 2. Properties of B⁺ Trees

Before diving into the implementation, it's essential to understand the defining properties of B⁺ Trees:

1. **Balanced Structure:**
   - All leaf nodes are at the same depth.
   - Ensures consistent performance for all operations.
2. **Order of the Tree (`m`):**
   - Defines the maximum number of children a node can have.
   - A B⁺ Tree of order `m` can have a maximum of `m` children and `m-1` keys per internal node.
3. **Node Capacity:**
   - Internal Nodes:
     - Minimum number of keys: `⌈m/2⌉ - 1`
     - Maximum number of keys: `m - 1`
   - Leaf Nodes:
     - Similar to internal nodes regarding the number of keys.
     - Additionally, each leaf node contains pointers to the next (and optionally previous) leaf nodes.
4. **Data Storage:**
   - **Internal Nodes:** Store only keys and child pointers.
   - **Leaf Nodes:** Store keys and associated data pointers (or the data itself).
5. **Linked Leaf Nodes:**
   - Leaf nodes are linked together to allow efficient range queries and ordered traversals.
6. **Key Duplication:**
   - In B⁺ Trees, keys are duplicated in internal nodes to guide the search process, but actual data resides only in the leaves.

------

## 3. B⁺ Tree Structure in C

To implement a B⁺ Tree in C, we need to define the structure of internal and leaf nodes. Below is a basic structure using C structs.

### **Definitions:**

- **Order (`M`):** Maximum number of children per node.
- **Minimum Degree (`T`):** Minimum number of keys per node (typically `ceil(M/2) - 1`).

### **Structs:**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define M 4 // Example order (maximum children)

typedef struct BPTreeNode {
    bool is_leaf;
    int num_keys;
    int keys[M - 1]; // Maximum keys
    struct BPTreeNode* children[M]; // Pointers to child nodes
    // For leaf nodes
    struct BPTreeNode* next; // Pointer to next leaf
    // Data pointers can be added here if storing data
} BPTreeNode;

// Function to create a new node
BPTreeNode* create_node(bool is_leaf) {
    BPTreeNode* node = (BPTreeNode*)malloc(sizeof(BPTreeNode));
    node->is_leaf = is_leaf;
    node->num_keys = 0;
    for(int i = 0; i < M; i++) {
        node->children[i] = NULL;
    }
    node->next = NULL;
    return node;
}
```

**Notes:**

- **`is_leaf`:** Indicates whether the node is a leaf.
- **`num_keys`:** Current number of keys in the node.
- **`keys`:** Array to store keys. Size is `M - 1` because a node with `M` children has `M - 1` keys.
- **`children`:** Array of child pointers. For leaf nodes, this can store data pointers instead.
- **`next`:** Pointer to the next leaf node (used only in leaf nodes).

------

## 4. Key Operations in B⁺ Trees

Implementing a B⁺ Tree involves handling several operations, primarily search, insertion, and deletion. Here's an overview of each:

### **4.1 Search**

The search operation navigates from the root to the appropriate leaf node by traversing internal nodes based on key comparisons.

**Algorithm:**

1. Start at the root node.
2. For each internal node:
   - Perform a binary search or linear search on the keys to find the correct child pointer.
   - Move to the child node.
3. Once a leaf node is reached, search for the key within the leaf's keys.
4. If found, return the associated data; otherwise, indicate that the key does not exist.

**Implementation:**

```c
BPTreeNode* search(BPTreeNode* root, int key) {
    if (root == NULL) return NULL;
    int i = 0;
    while (i < root->num_keys && key > root->keys[i]) {
        i++;
    }
    if (root->is_leaf) {
        // Search within the leaf node
        for(int j = 0; j < root->num_keys; j++) {
            if(root->keys[j] == key) {
                return root; // Key found
            }
        }
        return NULL; // Key not found
    } else {
        return search(root->children[i], key);
    }
}
```

### **4.2 Insertion**

Insertion involves:

1. Finding the Correct Leaf Node:
   - Similar to the search operation.
2. Inserting the Key:
   - Insert the key in the correct sorted position within the leaf node.
3. Handling Node Overflow:
   - If the leaf node exceeds the maximum number of keys, split the node.
   - Promote the middle key to the parent node.
   - Recursively handle splits up the tree, potentially creating a new root.

**Implementation:**

Due to the complexity of the insertion operation, here's a simplified version:

```c
// Function to insert a key into a leaf node
void insert_into_leaf(BPTreeNode* leaf, int key) {
    int i = leaf->num_keys - 1;
    // Find the position to insert the new key
    while (i >= 0 && leaf->keys[i] > key) {
        leaf->keys[i + 1] = leaf->keys[i];
        i--;
    }
    leaf->keys[i + 1] = key;
    leaf->num_keys++;
}

// Function to split a leaf node
BPTreeNode* split_leaf(BPTreeNode* leaf, int* new_key) {
    BPTreeNode* new_leaf = create_node(true);
    int mid = (M + 1) / 2;
    new_leaf->num_keys = leaf->num_keys - mid;
    // Move half the keys to the new leaf
    for(int i = 0; i < new_leaf->num_keys; i++) {
        new_leaf->keys[i] = leaf->keys[mid + i];
    }
    leaf->num_keys = mid;
    // Update leaf links
    new_leaf->next = leaf->next;
    leaf->next = new_leaf;
    // The first key of the new leaf is promoted
    *new_key = new_leaf->keys[0];
    return new_leaf;
}

// Function to insert a key into the B+ Tree
BPTreeNode* insert(BPTreeNode* root, int key) {
    if (root == NULL) {
        root = create_node(true);
        root->keys[0] = key;
        root->num_keys = 1;
        return root;
    }
    
    // Find the leaf node
    BPTreeNode* leaf = root;
    while (!leaf->is_leaf) {
        int i = 0;
        while (i < leaf->num_keys && key > leaf->keys[i]) {
            i++;
        }
        leaf = leaf->children[i];
    }
    
    // Insert the key into the leaf node
    insert_into_leaf(leaf, key);
    
    // Check for overflow
    if (leaf->num_keys == M) {
        int new_key;
        BPTreeNode* new_leaf = split_leaf(leaf, &new_key);
        // Insert the new key into the parent
        // This requires implementing a function to handle parent insertion
        // For simplicity, assume we have a function called insert_into_parent
        // root = insert_into_parent(root, leaf, new_key, new_leaf);
        // Implementing insert_into_parent is complex and beyond this scope
    }
    
    return root;
}
```

**Notes:**

- The `insert` function currently handles only inserting into leaf nodes and splitting them. To fully handle insertion, you need to implement `insert_into_parent`, which manages inserting the promoted key into the parent node and handling potential splits recursively.

### **4.3 Deletion**

Deletion involves:

1. **Finding the Key:**
   - Similar to the search operation.
2. **Removing the Key:**
   - Delete the key from the leaf node.
3. **Handling Node Underflow:**
   - If the node has fewer keys than the minimum required, attempt to redistribute keys from sibling nodes.
   - If redistribution is not possible, merge the node with a sibling.
   - Recursively handle underflows up the tree, potentially reducing the tree's height.

**Implementation:**

Deletion in B⁺ Trees is intricate. Here's a high-level overview without code:

1. **Locate the Leaf Node:**
   - Traverse the tree to find the leaf node containing the key.
2. **Remove the Key:**
   - Delete the key from the leaf node and shift remaining keys.
3. **Check for Underflow:**
   - If the leaf has fewer than `ceil(M/2) - 1` keys, attempt to borrow a key from a sibling.
4. **Merge if Necessary:**
   - If borrowing is not possible, merge the node with a sibling and adjust the parent node accordingly.
5. **Adjust the Tree:**
   - Update internal nodes to reflect any changes in keys due to merging or borrowing.

**Note:** Implementing deletion requires careful handling of various cases and maintaining the tree's balance. Due to its complexity, a full implementation is beyond this guide's scope.