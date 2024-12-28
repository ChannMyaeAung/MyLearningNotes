### B-Tree Implementation in C (Simple)

#### Code Intro with explanation and/or visualization

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

// Minimum degree (defines the range for number of keys)
#define T 3

// B-Tree node structure
typedef struct BTreeNode {
    int *keys;               // An array of keys
    int t;                   // Minimum degree
    struct BTreeNode **C;    // An array of child pointers
    int n;                   // Current number of keys
    bool leaf;               // Is true when node is leaf. Otherwise false
} BTreeNode;
```

##### Creating a New B-Tree Node

```c
BTreeNode* createNode(int t, bool leaf) {
    BTreeNode* node = (BTreeNode*) malloc(sizeof(BTreeNode));
    node->t = t;
    node->leaf = leaf;

    node->keys = (int*) malloc(sizeof(int) * (2 * t - 1));
    node->C = (BTreeNode**) malloc(sizeof(BTreeNode*) * (2 * t));

    for (int i = 0; i < 2 * t; i++)
    {
        node->C[i] = NULL;
    }
    node->n = 0;
    return node;
}
```

**Before Initialization:**

```css
+-----------+
|  BTreeNode|
+-----------+
| keys      | [ ... ]
| t         | 3
| C         | [ ? , ? , ? , ? , ? , ? ]
| n         | 0
| leaf      | true
+-----------+
```

*(Child pointers contain garbage values)*

**After Initialization:**

```css
+-----------+
|  BTreeNode|
+-----------+
| keys      | [ ... ]
| t         | 3
| C         | [NULL, NULL, NULL, NULL, NULL, NULL]
| n         | 0
| leaf      | true
+-----------+
```

*(All child pointers are safely set to NULL)*

##### B-Tree Structure

```c
typedef struct BTree {
    BTreeNode* root;
    int t;
} BTree;
```

##### Initializing a B-Tree

```c
BTree* initializeBTree(int t) {
    BTree* tree = (BTree*) malloc(sizeof(BTree));
    tree->t = t;
    tree->root = createNode(t, true);
    return tree;
}
```

##### Searching in a B-Tree

The `search` function recursively searches for a key starting from the root node.

```c
BTreeNode *search(BTreeNode *root, int key)
{
    int i = 0;

    // checks if the search key is > the current key at index i
    while (i < root->n && key > root->keys[i])
    {
        // increment i to continue checking the next key as long as the key to search is greater than the current key
        i++;
    }

    if (i < root->n && root->keys[i] == key)
    {
        return root;
    }

    if (root->leaf)
    {
        return NULL;
    }

    // once the loop finishes, the value of i will indicate the appropriate child node to search next.
    return search(root->C[i], key);
}
```

###### Visualization of searching in B-Tree

- Let's say we want to find the value 30 in the following node:

```css
 i  i = 1 i = 2 (found it!)
 ↓   ↓    ↓
[10, 20, 30]
```

- If we want to find the value 14 in the following node:

  ```css
             i          i = 1     i = 2 
   			 ↓          ↓         ↓
  			[10,      20,         30]
             /      |             |           \
       [5,6,7,8] [11,12,13,14] [21,22,23,24]  [32,33,34,35]
  ```

- Since 12 is between 10 and 20, we don't move forward

- Consider searching for key `15` in the following B-tree:

  ```css
              [10 | 20 | 30]
          /     |          |     \
     [5 | 8] [12 |15|18] [25|28] [35|40]
  ```

- **Search Steps:**

  1. **Start at Root Node `[10 | 20 | 30]`**
     - Compare `15` with `10` → `15 > 10` → `i = 1`
     - Compare `15` with `20` → `15 < 20` → Stop; `i = 1`
  2. **Key `15` ≠ `20`, and Root is Not a Leaf**
     - Recurse to Child at Index `1` → Node `[12 |15|18]`
  3. **At Node `[12 |15|18]`**
     - Compare `15` with `12` → `15 > 12` → `i = 1`
     - Compare `15` with `15` → `15 == 15` → Key Found

  **Result:** The key `15` is found in the node `[12 |15|18]`.

##### Splitting a Child

Before inserting a new key, if the child node is full, it needs to be split.

The `splitChild` function splits a full child node into two nodes and moves the median key up to the parent node.

```c
// i = index in the parent's child array where the new child will be inserted
void splitChild(BTreeNode *parent, int i, BTreeNode *fullChild)
{
    // Minimum degree
    int t = parent->t;

    // A new node to hold the latter half of fullChild's keys
    BTreeNode *newChild = createNode(t, fullChild->leaf);

    // # of keys in newChild after splitting.
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

    // Update the # of keys in fullChild
    fullChild->n = t - 1;

    // Create space for newChild in parent
    // shifts the child pointers in the parent node to make room for the new child
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

    // Move the middle key up to the parent
    parent->keys[i] = fullChild->keys[t - 1];
    parent->n += 1;
}
```

###### Visualization of Splitting in B-Trees

```css
                [40, 80]
               /    |     \
[10, 20, 30, 35, 40]   [60, 70]   [90, 100]

```



T = 3 as we have defined, we can have # of keys between 1 and 3.

**Parent Node:** `[40, 80]` with **2 keys** (valid, 2≤keys≤5)

**Children:**

- **Child 0:** `[10, 20, 30, 35, 40]` with **5 keys** (**overfull** if inserting another key, but assuming we need to split now)
- **Child 1:** `[60, 70]` with **2 keys** (valid)
- **Child 2:** `[90, 100]` with **2 keys** (valid)

###### **Step-by-Step Splitting Process**

**Step 1: Identify the Overfull Child**

- **Overfull Child:** `[10, 20, 30, 35, 40]` (5 keys, which is the maximum allowed but needs splitting if inserting another key)

**Step 2: Apply `splitChild` Function**

1. **Parameters:**
   - **Parent Node:** `[40, 80]`
   - **Index `i`:** `0` (Child 0 is overfull)
   - **Full Child:** `[10, 20, 30, 35, 40]`
   - **Minimum Degree t=3.**
2. **Determine Split Parameters:**
   - **Median Key (to promote):** `30` (3rd key, index t−1=2)
   - **Keys to be in the left node after split:** First t−1=2 keys: `[10, 20]`
   - **Keys to be in the right node after split:** Last t−1=2 keys: `[35, 40]`
   - **Promoted Key:** `30`
3. **Create `newChild` and Assign Keys:**
   - **`newChild->keys`:** `[35, 40]`
   - **`newChild->n`:** `2` (correct as t−1=2)
4. **Update `fullChild`:**
   - **`fullChild->keys`:** `[10, 20]` (first two keys)
   - **`fullChild->n`:** `2`
5. **Promote the Median Key to Parent:**
   - **Parent's Keys Before Promotion:** `[40, 80]`
   - **Insert `30` into Parent:** After insertion, Parent's keys become `[30, 40, 80]`
   - **Parent's Key Count:** `3` (valid, 2≤3≤5)
6. **Update Parent's Children Pointers:**
   - Parent's Children After Split:
     - **Child 0:** `[10, 20]`
     - **Child 1:** `[35, 40]` (`newChild`)
     - **Child 2:** `[60, 70]`
     - **Child 3:** `[90, 100]`

#### **Visual Representation After Correct Split**

```css
                 [30, 40, 80]
                 /      |      |      \
       [10, 20]    [35, 40]   [60, 70]   [90, 10
```

###### **Detailed Steps Mapped to `splitChild` Function**

Let’s align the corrected example with your `splitChild` function:

1. **Create New Child:**

   ```c
   BTreeNode *newChild = createNode(t, fullChild->leaf);
   ```

   - **`newChild`:** Created as a new node, same leaf status as `fullChild`.

2. **Assign Keys to `newChild`:**

   ```c
   newChild->n = t - 1; // newChild->n = 2
   for (int j = 0; j < t - 1; j++)
   {
       newChild->keys[j] = fullChild->keys[j + t];
   }
   ```

   - **Keys Moved:** `[35, 40]` (assuming `fullChild->keys` are `[10, 20, 30, 35, 40]`)
   - **Explanation:** For  t=3 , j+t iterates as follows:
     - j=0: `newChild->keys[0] = fullChild->keys[3] = 35`
     - j=1: `newChild->keys[1] = fullChild->keys[4] = 40`

3. **Assign Children to `newChild` (if not a leaf):**

   ```c
   if (!fullChild->leaf)
   {
       for (int j = 0; j < t; j++)
       {
           newChild->C[j] = fullChild->C[j + t];
       }
   }
   ```

   - **In this example:** Assuming the `fullChild` is a leaf, so no children to assign.

4. **Update `fullChild` Key Count:**

   ```c
   fullChild->n = t - 1; // fullChild->n = 2
   ```

   - **`fullChild` After Split:** `[10, 20]` with `n = 2`

5. **Shift Parent's Children to Make Room for `newChild`:**

   ```c
   for (int j = parent->n; j >= i + 1; j--)
   {
       parent->C[j + 1] = parent->C[j];
   }
   ```

   - **Shifting Children:** Existing children are shifted to the right to accommodate the new child.

6. **Insert `newChild` into Parent's Children:**

   ```c
   parent->C[i + 1] = newChild;
   ```

   - Parent's Children After Insertion:
     - **Child 0:** `[10, 20]`
     - **Child 1:** `[35, 40]` (`newChild`)
     - **Child 2:** `[60, 70]`
     - **Child 3:** `[90, 100]`

7. **Shift Parent's Keys to Make Room for Promoted Key:**

   ```c
   for (int j = parent->n - 1; j >= i; j--)
   {
       parent->keys[j + 1] = parent->keys[j];
   }
   ```

   - **Shifting Keys:** Existing keys are shifted to the right to insert the promoted key.

8. **Promote the Median Key to Parent:**

   ```c
   parent->keys[i] = fullChild->keys[t - 1]; // parent->keys[0] = 30
   parent->n += 1; // parent->n = 3
   ```

   - **Promoted Key:** `30`
   - **Parent's Keys After Promotion:** `[30, 40, 80]`

**Final Balanced B-Tree Structure**

```css
                  [30, 40, 80]
                 /      |      |      \
       [10, 20]    [35, 40]   [60, 70]   [90, 100
```

##### Inserting a Key

Insertion involves finding the correct position for the new key and ensuring that nodes are split if necessary to maintain B-Tree properties.

**insertNonFull**: Inserts a key into a node that is not full. If the node is a leaf, it inserts the key directly. If it's an internal node, it finds the appropriate child to recurse into and splits it if necessary.

**insert**: Handles the case where the root node is full. It creates a new root and splits the old root before proceeding with insertion.

```c
void insertNonFull(BTreeNode* node, int key) {
    
    // starts from the last key in the node and move left to find the correct position for the new key
    int i = node->n - 1;

    if (node->leaf) {
        // Find the location where new key must be inserted
        while (i >= 0 && node->keys[i] > key) {
            node->keys[i + 1] = node->keys[i];
            i--;
        }
		// for e.g [3, 4, 5, 7]
        // we want to insert 6, so i starts at 7, since 7 > 6, decrement i and now i will points to 5, i = 2. 5 < 6, so we insert 6 next to 5 which is i + 1 = 2 + 1 = 3 in the 3rd index. 
        // [3,4,5,6,7]
        // Insert the new key at found location
        node->keys[i + 1] = key;
        node->n += 1;
    } else {
        // Find the child which is going to have the new key
        while (i >= 0 && node->keys[i] > key)
            i--;

        // Check if the found child is full
        if (node->C[i + 1]->n == 2 * node->t - 1) {
            // If the child is full, split it
            splitChild(node, i + 1, node->C[i + 1]);

            // After split, the middle key of C[i+1] goes up and
            // C[i+1] is split into two. Decide which of the two
            // will have the new key
            if (node->keys[i + 1] < key)
                i++;
        }
        
        // recursively call insertNonFull on the chosen child
        insertNonFull(node->C[i + 1], key);
    }
}

void insert(BTree* tree, int key) {
    BTreeNode* root = tree->root;

    // If root is full, then tree grows in height
    if (root->n == 2 * tree->t - 1) {
        BTreeNode* s = createNode(tree->t, false);
        s->C[0] = root;
        
        // use splitChild() to divide the full root into two nodes.
        // move the middle key of the old root up to the new root.
        splitChild(s, 0, root);

        // New root has two children now. Decide which child to insert the key
        int i = 0;
        
        // this is based on comparing the key to be inserted and the new promoted middle key
        if (s->keys[0] < key)
            i++;
        
        // call insertNonFull function on the appropriate child node to insert the new key
        insertNonFull(s->C[i], key);

        tree->root = s;
    } else {
        insertNonFull(root, key);
    }
}
```

###### Visulization and Explanation of Insert functions in B-Trees

Step-by-Step Example

**Scenario:**
Let's insert the following keys into an initially empty B-Tree with a minimum degree `t = 3` (each node can have at most `2t - 1 = 5` keys).

**Keys to Insert:**
`[10, 20, 5, 6, 12, 30, 7, 17]`

**B-Tree Properties:**

- **Minimum Degree (`t`):** 3
- **Maximum Keys per Node:** `2t - 1 = 5`
- **Maximum Children per Node:** `2t = 6`

**Insertion Process:**

1. **Insert 10**:

   - Tree is empty. Create a root node and insert `10`.

   - Tree Structure:

     [10]

2. **Insert 20**:

   - Root is not full. Insert `20` into root.

   - Tree Structure:

     [10, 20]

3. **Insert 5**:

   - Root is not full. Insert `5` into root.

   - Tree Structure:

     [5, 10, 20]

4. **Insert 6**:

   - Root is not full. Insert `6` into root.

   - Tree Structure:

     [5, 6, 10, 20]

5. **Insert 12**:

   - Root is not full. Insert `12` into root.

   - Tree Structure:

     [5, 6, 10, 12, 20]

6. **Insert 30**:

   - Root is **full** (`n = 5`).

   - Action:

     - Create a new root node `s`.
     - Split the old root.
     - Move the middle key (`10`) to the new root.
     - The old root now has `[5, 6]` and a new child has `[12, 20]`.

   - Tree Structure:

     ```css
         [10]
        /     \
     [5, 6]  [12, 20, 30]
     ```

     

   - Insert 30:

     - Compare `30` with `10`. Since `30 > 10`, insert into the right child `[12, 20]`.
     - Right child is not full. Insert `30`.

   - Final Structure after Insertion:

     ```css
         [10]
        /     \
     [5, 6]  [12, 20, 30]
     ```

     

7. **Insert 7**:

   - Root is not full.

   - Compare `7` with `10`. Since `7 < 10`, insert into the left child `[5, 6]`.

   - Left child is not full. Insert `7`.

   - Tree Structure:

     ```css
         [10]
        /     \
     [5, 6, 7]  [12, 20, 30]
     ```

8. **Insert 17**:

   - Root is not full.

   - Compare `17` with `10`. Since `17 > 10`, insert into the right child `[12, 20, 30]`.

   - Right child is not full. Insert `17`.

   - Final B-Tree Structure:

     ```css
         [10]
        /     \
     [5, 6, 7]  [12, 17, 20, 30]
     ```

**Visualizing the Insertion Process**

Let's visualize the insertion of key `30` in detail to understand how `insert` and `insertNonFull` work together.

**Before Inserting 30:**

  [5, 6, 10, 12, 20]

- Root Node:
  - Keys: `[5, 6, 10, 12, 20]`
  - Node is full (`n = 5`).

**Inserting 30:**

1. **Call `insert` Function:**

   - **Check Root:**
     Root is full (`n = 5`).
   - Action:
     - Create a new root `s`.
     - Set `s->C[0]` to old root.
     - Call `splitChild(s, 0, old root)`.

2. **Call `splitChild` Function:**

   - Parameters:
     - `parent = s`
     - `i = 0`
     - `fullChild = old root [5, 6, 10, 12, 20]`
   - Action:
     - Create `newChild`.
     - Move last `t - 1 = 2` keys (`10`, `12`) to `newChild`.
     - Update `fullChild` to have first `t - 1 = 2` keys (`5, 6`).
     - Insert `10` into `parent`.
     - Result:
       - **New Root `s`:** `[10]`
       - Children:
         - `s->C[0]`: `[5, 6]`
         - `s->C[1]`: `[12, 20]`

3. **Continue `insert`:**

   - **Determine Child for 30:**
     Compare `30` with `10`. Since `30 > 10`, set `i = 1`.
   - Call `insertNonFull` on `s->C[1]` (`[12, 20]`):
     - Node is Not Full:
       - Insert `30` into `[12, 20]` resulting in `[12, 20, 30]`.

4. **Final Structure After Insertion:**

   ```css
       [10]
      /     \
   [5,6]  [12,20,30]
   ```

##### Traversing the B-Tree

A simple in-order traversal to print the keys.

The `traverse` function performs an in-order traversal of the B-Tree, printing keys in sorted order.

```c
void traverse(BTreeNode* root) {
    if (root != NULL) {
        int i;
        for (i = 0; i < root->n; i++) {
            if (!root->leaf)
                traverse(root->C[i]);
            printf("%d ", root->keys[i]);
        }
        if (!root->leaf)
            traverse(root->C[i]);
    }
}
```

#### Full Program Code

`practice.h`

```C
// Minimum degree (defines the range for number of keys)
#define T 3

// B-Tree node structure
typedef struct BTreeNode {
    int *keys;               // An array of keys
    int t;                   // Minimum degree
    struct BTreeNode **C;    // An array of child pointers
    int n;                   // Current number of keys
    bool leaf;               // Is true when node is leaf. Otherwise false
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
    tree->t = t;
    tree->root = createNode(t, true);
    return tree;
}
BTreeNode *search(BTreeNode *root, int key)
{
    int i = 0;

    // checks if the search key is > the current key at index i
    while (i < root->n && key > root->keys[i])
    {
        // increment i to continue checking the next key
        i++;
    }

    if (i < root->n && root->keys[i] == key)
    {
        return root;
    }

    if (root->leaf)
    {
        return NULL;
    }

    // once the loop finishes, the value of i will indicate the appropriate child node to search next.
    return search(root->C[i], key);
}

// i = index in the parent's child array where the new child will be inserted
void splitChild(BTreeNode *parent, int i, BTreeNode *fullChild)
{
    // Minimum degree
    int t = parent->t;

    // A new node to hold the latter half of fullChild's keys
    BTreeNode *newChild = createNode(t, fullChild->leaf);

    // # of keys in newChild after splitting.
    newChild->n = t - 1;

    // copy the last (t-1) keys of fullChild to newChild
    for(int j = 0; j < t - 1; j++){
        newChild->keys[j] = fullChild->keys[j + t];
    }

    // Copy the last t children of fullChild to newChild
    if(!fullChild->leaf){
        for(int j = 0; j < t; j++){
            newChild->C[j] = fullChild->C[j + t];
        }
    }

    // Update the # of keys in fullChild
    fullChild->n = t - 1;

    // Create space for newChild in parent
    // shifts the child pointers in the parent node to make room for the new child
    for(int j = parent->n; j >= i + 1; j--){
        parent->C[j + 1] = parent->C[j];
    }

    // inserts newChild as a child of parent at position i + 1
    parent->C[i + 1] = newChild;

    // Move keys in parent to make space for the middle key
    for(int j = parent->n - 1; j >= i; j--){
        parent->keys[j + 1] = parent->keys[j];
    }

    // Move the middle key up to the parent
    parent->keys[i] = fullChild->keys[t - 1];
    parent->n += 1;
}

void insertNonFull(BTreeNode *node, int key)
{
    // starts from the last key in the node
    int i = node->n - 1;
    if (node->leaf)
    {
        // Find the location of where new key must be inserted
        while (i >= 0 && node->keys[i] > key)
        {
            node->keys[i + 1] = node->keys[i];
            i--;
        }

        // insert the new key at found location
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

        // Check if the found child is full
        if (node->C[i + 1]->n == 2 * node->t - 1)
        {
            // If the child is full, split it
            splitChild(node, i + 1, node->C[i + 1]);

            // After split, the middle key of C[i + 1] goes up and
            // C[i + 1] is split into two. Decide which of the two
            // will have the new key
            if (node->keys[i + 1] < key)
            {
                i++;
            }
        }
        insertNonFull(node->C[i + 1], key);
    }
}
void insert(BTree *tree, int key)
{
    BTreeNode *root = tree->root;

    // If root is full, then tree grows in height
    if (root->n == 2 * tree->t - 1)
    {
        BTreeNode *s = createNode(tree->t, false);
        s->C[0] = root;
        splitChild(s, 0, root);

        // New Root has two children now. Decide which child to insert the key
        int i = 0;
        if (s->keys[0] < key)
        {
            i++;
        }
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
        int i;
        for (i = 0; i < root->n; i++)
        {
            if (!root->leaf)
            {
                traverse(root->C[i]);
            }
            printf("%d ", root->keys[i]);
        }
        if (!root->leaf)
        {
            traverse(root->C[i]);
        }
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

    freeTree(tree->root);
    free(tree);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==15895== Memcheck, a memory error detector
==15895== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15895== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15895== Command: ./final
==15895== 
Traversal of the constructed B-tree is: 
5 6 7 10 12 17 20 30 
Key 6 found in the B-Tree.
==15895== 
==15895== HEAP SUMMARY:
==15895==     in use at exit: 0 bytes in 0 blocks
==15895==   total heap usage: 11 allocs, 11 frees, 1,340 bytes allocated
==15895== 
==15895== All heap blocks were freed -- no leaks are possible
==15895== 
==15895== For lists of detected and suppressed errors, rerun with: -s
==15895== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**B-Tree Diagram**

```css
          [10]
         /    \
   [5, 6, 7]  [12, 17, 20, 30]
```

- **Insertion Sequence:**
  The keys were inserted in the following order: `10, 20, 5, 6, 12, 30, 7, 17`.
- **Root Splitting:**
  - Initially, keys are inserted into the root node until it becomes full.
  - When inserting the 6th key, the root node splits to maintain the B-tree properties (minimum degree `t = 3`).
  - The middle key (`10`) moves up to become the new root, and the remaining keys are distributed among two child nodes.
- **Final Structure:**
  - **Root (`[10]`):** Contains the middle key after splitting.
  - **Left Child (`[5, 6, 7]`):** Contains keys less than `10`.
  - **Right Child (`[12, 17, 20, 30]`):** Contains keys greater than `10`.
- This structure ensures that the B-tree properties are maintained:
  - **Balanced Tree:** All leaf nodes are at the same depth.
  - **Sorted Keys:** In-order traversal yields sorted keys.
  - **Efficient Operations:** Search, insertion, and deletion operations remain efficient due to the balanced nature of the tree.

