### Extending the B-Tree implementation to handle duplicates and allow a dynamic degree

#### Program Code

`practice.h`

```C
#include <stdbool.h>
#ifndef HELLO_H
#define HELLO_H

typedef struct Key
{
    int value; // The key value
    int count; // Number of occurrences of the key
} Key;

typedef struct BTreeNode
{
    Key *keys;            // An array of keys with counts
    int t;                // Minimum degree (dynamic)
    struct BTreeNode **C; // an array of child pointers
    int n;                // Current number of keys
    bool leaf;            // is true when node is leaf, otherwise false
} BTreeNode;

typedef struct BTree
{
    BTreeNode *root;
    int t; // min degree
} BTree;

BTreeNode *createNode(int t, bool leaf);

BTree *initializeBTree(int t);
BTreeNode *search(BTreeNode *root, int key);
void splitChild(BTreeNode *parent, int i, BTreeNode *fullChild);

void insertNonFull(BTreeNode *node, int key);
void insert(BTree *tree, int key);

void traverse(BTreeNode *root);

int findKey(BTreeNode *node, int key);

void removeKey(BTree *tree, int key);

void freeTree(BTreeNode *root);
#endif // HELLO_H
```

`functions.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <strings.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <dirent.h>
#include <unistd.h>
#include <limits.h>
#include <glob.h>
#include <assert.h>
#include "hello.h"

BTreeNode *createNode(int t, bool leaf)
{
    BTreeNode *node = malloc(sizeof(BTreeNode));
    node->t = t;
    node->leaf = leaf;

    node->keys = malloc(sizeof(Key) * (2 * t - 1));
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
    while (i < root->n && key > root->keys[i].value)
    {

        // increment the index to check the next key
        i++;
    }

    if (i < root->n && key == root->keys[i].value)
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
        newChild->keys[j].value = fullChild->keys[j + t].value;
        newChild->keys[j].count = fullChild->keys[j + t].count;
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
    parent->keys[i].value = fullChild->keys[t - 1].value;
    parent->keys[i].count = fullChild->keys[t - 1].count;
    parent->n += 1;
}

void insertNonFull(BTreeNode *node, int key)
{
    // starts from the rightmost key in the node and move left to find the correct position for the new key
    int i = node->n - 1;

    if (node->leaf)
    {
        // Find the location where new key must be inserted
        while (i >= 0 && node->keys[i].value > key)
        {
            node->keys[i + 1] = node->keys[i];
            i--;
        }

        // check if the key already exists and increment count if it does
        if (i >= 0 && node->keys[i].value == key)
        {
            node->keys[i].count++;
            return;
        }
        // for e.g [3, 4, 5, 7]
        // we want to insert 6, so i starts at 7, since 7 > 6, decrement i and now i will points to 5, i = 2. 5 < 6, so we insert 6 next to 5 which is i + 1 = 2 + 1 = 3 in the 3rd index.
        // [3,4,5,6,7]
        // Insert the new key at found location
        node->keys[i + 1].value = key;
        node->keys[i + 1].count = 1;
        node->n += 1;
    }
    else
    {
        // find the child which is going to have the new key
        while (i >= 0 && node->keys[i].value > key)
        {
            i--;
        }

        // if the key already exists in the internal node, increment its count
        if (i >= 0 && node->keys[i].value == key)
        {
            node->keys[i].count++;
            return;
        }

        // check if the found child is full
        if (node->C[i + 1]->n == 2 * node->t - 1)
        {
            // if the child is full, split it
            splitChild(node, i + 1, node->C[i + 1]);

            // After split, the middle key of C[i+1] goes up and
            // C[i+1] is split into two. Decide which of the two
            // will have the new key
            if (node->keys[i + 1].value < key)
            {
                i++;
            }
            else if (key == node->keys[i + 1].value)
            {
                // if the key equals the middle key, increment count
                node->keys[i + 1].count++;
                return;
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

        if (s->keys[0].value < key)
        {
            i++;
        }
        else if (s->keys[0].value == key)
        {
            // if the key equals the middle key after split, increment count
            s->keys[0].count++;
            return;
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
            printf("%d(%d) ", root->keys[i].value, root->keys[i].count);
        }
        if (!root->leaf)
        {
            traverse(root->C[root->n]);
        }
    }
}

int findKey(BTreeNode *node, int key)
{
    int idx = 0;
    while (idx < node->n && node->keys[idx].value < key)
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
        // if the node is not a leaf, the loop goes to the rightmost node in the left subtree to find the largest key
        current = current->C[current->n];
    }

    // return the last key in the node (the rightmost key)
    return current->keys[current->n - 1].value;
}

static int getSuccessor(BTreeNode *node, int idx)
{
    BTreeNode *current = node->C[idx + 1];
    while (!current->leaf)
    {
        // if the node is not a leaf, the loop goes to the leftmost node in the right subtree to find the smallest key
        current = current->C[0];
    }

    // return the first key in the node (the leftmost key)
    return current->keys[0].value;
}

// Function to borrow a key from C[idx - 1] and insert it into C[idx]
static void borrowFromPrev(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx - 1];

    // Moving all keys in child one step ahead
    for (int i = child->n - 1; i >= 0; --i)
        child->keys[i + 1] = child->keys[i];

    // If child is not leaf, move its child pointers
    if (!child->leaf)
    {
        for (int i = child->n; i >= 0; --i)
        {
            child->C[i + 1] = child->C[i];
        }
    }

    // Moving the key from parent to child
    child->keys[0].value = node->keys[idx - 1].value;
    child->keys[0].count = node->keys[idx - 1].count;

    // Moving the last child from sibling to child
    if (!child->leaf)
        child->C[0] = sibling->C[sibling->n];

    // Moving the last key from sibling to parent
    node->keys[idx - 1].value = sibling->keys[sibling->n - 1].value;
    node->keys[idx - 1].count = sibling->keys[sibling->n - 1].count;

    child->n += 1;
    sibling->n -= 1;
}
// Function to borrow a key from C[idx + 1] and insert it into C[idx]
static void borrowFromNext(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx + 1];

    // node's key[idx] is inserted as the last key in child
    child->keys[child->n].value = node->keys[idx].value;
    child->keys[child->n].count = node->keys[idx].count;
    child->n += 1;

    // if child is not a leaf, sibling's first child is inserted as the last child into child
    if (!child->leaf)
    {
        child->C[child->n + 1] = sibling->C[0];
    }

    //] Move the first key from sibling up to the parent node
    node->keys[idx].value = sibling->keys[0].value;
    node->keys[idx].count = sibling->keys[0].count;

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

    sibling->n -= 1;
}

// Function to merge C[idx] with C[idx + 1]
static void merge(BTreeNode *node, int idx)
{
    BTreeNode *child = node->C[idx];
    BTreeNode *sibling = node->C[idx + 1];
    int t = node->t;

    // Pulling a key from the current node and inserting it into (t-1)th position of child
    child->keys[t - 1].value = node->keys[idx].value;
    child->keys[t - 1].count = node->keys[idx].count;

    // Copying keys from sibling to child
    for (int i = 0; i < sibling->n; ++i)
        child->keys[i + t] = sibling->keys[i];

    // if child is not a leaf, Copy child pointers from sibling to child
    if (!child->leaf)
    {
        for (int i = 0; i <= sibling->n; ++i)
            child->C[i + t] = sibling->C[i];
    }

    // Moving all keys after idx in the current node one step before to fill the gap
    for (int i = idx + 1; i < node->n; ++i)
        node->keys[i - 1] = node->keys[i];

    // Moving the child pointers after (idx+1) in the current node one step before
    for (int i = idx + 2; i <= node->n; ++i)
        node->C[i - 1] = node->C[i];

    // Updating the key count of child and the current node
    child->n += sibling->n + 1;
    node->n--;

    // Freeing the memory occupied by sibling
    free(sibling->keys);
    free(sibling->C);
    free(sibling);
}

// Function to fill the child node at idx which has less than t-1 keys
static void fill(BTreeNode *node, int idx)
{
    // if the previous child has more than t keys, borrow a key from that child
    if (idx != 0 && node->C[idx - 1]->n >= node->t)
    {
        // borrow from the previous child
        borrowFromPrev(node, idx);
    }
    // else if the next child has more than t keys, borrow a key from that child
    else if (idx != node->n && node->C[idx + 1]->n >= node->t)
    {
        borrowFromNext(node, idx);
    }
    // if both siblings have t-1 keys, merge the child with a sibling
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

    // If count > 1, just decrement the count
    if (node->keys[idx].count > 1)
    {
        node->keys[idx].count--;
        return;
    }

    // otherwise, remove the key by shifting
    // shift all keys after idx to the left
    for (int i = idx + 1; i < node->n; i++)
    {
        node->keys[i - 1] = node->keys[i];
    }
    node->n--;
}

static void removeFromNonLeaf(BTreeNode *node, int idx)
{
    int key = node->keys[idx].value;

    // If the child before key (C[idx]) has at least t keys, find predecessor
    if (node->C[idx]->n >= node->t)
    {
        // get the maximum key(last key) in the predecessor
        int pred = getPredecessor(node, idx);
        // that key will move up to its parent and replace the key to be deleted
        node->keys[idx].value = pred;

        // remove the key we want to delete
        removeFromNode(node->C[idx], pred);
    }
    // Else if the child after key (C[idx+1]) has at least t keys, find successor
    else if (node->C[idx + 1]->n >= node->t)
    {
        // get the minimum key(first key) in the successor
        int succ = getSuccessor(node, idx);
        // that key will move up to its parent and replace the key to be deleted
        node->keys[idx].value = succ;

        // remove the key we want to delete
        removeFromNode(node->C[idx + 1], succ);
    }
    // If both children have t-1 keys, merge them
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

    // The key to be removed is present in this node
    if (idx < node->n && node->keys[idx].value == key)
    {
        if (node->leaf)
        {
            removeFromLeaf(node, idx);
        }
        else
        {
            removeFromNonLeaf(node, idx);
        }
    }
    else
    {
        // The key is not present in this node
        if (node->leaf)
        {
            printf("The key %d does not exist in the tree.\n", key);
            return;
        }

        // Flag indicates whether the key is present in the subtree rooted with the last child
        bool flag = ((idx == node->n) ? true : false);

        // If the child where the key is supposed to exist has less than t keys, fill it
        if (node->C[idx]->n < node->t - 1)
        {
            fill(node, idx);
        }

        // If the last child has been merged, it must have merged with the previous child
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
    if (root != NULL)
    {
        if (!root->leaf)
        {
            for (int i = 0; i <= root->n; i++)
            {
                freeTree(root->C[i]);
            }
        }
    }

    free(root->keys);
    free(root->C);
    free(root);
}
```

`practice.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include "hello.h"

int main()
{
    int t;
    printf("Enter the minimum degree (t) for the B-Tree: ");
    BTree *tree = initializeBTree(t);

    int keys[] = {10, 20, 5, 6, 12, 30, 7, 17, 6, 10, 10};
    int n = sizeof(keys) / sizeof(keys[0]);

    printf("Inserting keys: ");
    for (int i = 0; i < n; i++)
    {
        printf("%d ", keys[i]);
        insert(tree, keys[i]);
    }
    printf("\n");

    printf("Traversal of the constructed B-tree is: \n");
    traverse(tree->root);
    printf("\n");

    // Search for a key
    int key = 6;
    BTreeNode *res = search(tree->root, key);
    if (res != NULL)
    {
        printf("Key %d found in the B-Tree with count %d.\n", key, res->keys[findKey(res, key)].count);
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
    int del_keys[] = {6, 10, 10};
    int del_n = sizeof(del_keys) / sizeof(del_keys[0]);

    for (int i = 0; i < del_n; i++)
    {
        printf("\nDeleting key %d...\n", del_keys[i]);
        removeKey(tree, del_keys[i]);
        printf("Traversal after deletion is: ");
        traverse(tree->root);
        printf("\n");

        res = search(tree->root, del_keys[i]);
        if (res != NULL)
        {
            printf("Key %d found in the B-Tre with count %d.\n", del_keys[i], res->keys[findKey(res, del_keys[i])].count);
        }
        else
        {
            printf("Key %d not found in the B-Tree.\n", del_keys[i]);
        }
    }

    freeTree(tree->root);
    free(tree);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Enter the minimum degree (t) for the B-Tree: 3
Inserting keys: 10 20 5 6 12 30 7 17 6 10 10 
Traversal of the constructed B-tree is: 
5(1) 6(2) 7(1) 10(3) 12(1) 17(1) 20(1) 30(1) 
Key 6 found in the B-Tree with count 2.

Deleting key 6...
Traversal after deletion is: 5(1) 6(1) 7(1) 10(3) 12(1) 17(1) 20(1) 30(1) 
Key 6 found in the B-Tree.

Deleting key 6...
Traversal after deletion is: 5(1) 7(1) 10(3) 12(1) 17(1) 20(1) 30(1) 
Key 6 not found in the B-Tree.

Deleting key 10...
Traversal after deletion is: 5(1) 7(1) 12(3) 17(1) 20(1) 30(1) 
Key 10 not found in the B-Tree.

Deleting key 10...
The key 10 does not exist in the tree.
Traversal after deletion is: 5(1) 7(1) 12(3) 17(1) 20(1) 30(1) 
Key 10 not found in the B-Tree.

```

