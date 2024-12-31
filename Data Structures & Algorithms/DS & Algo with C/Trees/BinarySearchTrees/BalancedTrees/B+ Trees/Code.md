# Implementation of B+Tree in C

#### Program Code

`hello.h`

```C
#include <stdbool.h>
#ifndef HELLO_H
#define HELLO_H

#define M 4 // Order of the B+ Tree

typedef struct BPTreeNode
{
    bool is_leaf;
    int num_keys;
    int keys[M - 1];                // Max keys
    struct BPTreeNode *children[M]; // Pointers to child nodes
    // For leaf nodes
    struct BPTreeNode *next; // Pointer to next leaf
    // Data pointers can be added here if storing data
} BPTreeNode;

BPTreeNode *create_node(bool is_leaf);
BPTreeNode *search(BPTreeNode *root, int key);
void insert_into_leaf(BPTreeNode *leaf, int key);
BPTreeNode *split_leaf(BPTreeNode *leaf, int *new_key);

void insert_into_internal(BPTreeNode *internal, int key, BPTreeNode *child);

BPTreeNode *split_internal(BPTreeNode *internal, int *new_key);

BPTreeNode *insert_into_parent(BPTreeNode *root, BPTreeNode *left, int key, BPTreeNode *right);
BPTreeNode *insert(BPTreeNode *root, int key);
void print_tree(BPTreeNode *root, int level);

void free_tree(BPTreeNode *root);

BPTreeNode *find_parent(BPTreeNode *current, BPTreeNode *child);
```

`hello.c`

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

// Function to create a new B+ Tree node
BPTreeNode *create_node(bool is_leaf)
{
    BPTreeNode *node = malloc(sizeof(BPTreeNode));
    if (node == NULL)
    {
        perror("Failed to allocate memory for node");
        exit(EXIT_FAILURE);
    }
    node->is_leaf = is_leaf;
    node->num_keys = 0;
    for (int i = 0; i < M; i++)
    {
        node->children[i] = NULL;
    }

    node->next = NULL;
    return node;
}

// Recursive function to find the parent of a given child node
BPTreeNode *find_parent(BPTreeNode *current, BPTreeNode *child)
{
    if (current == NULL || current->is_leaf)
        return NULL;

    for (int i = 0; i <= current->num_keys; i++)
    {
        if (current->children[i] == child)
        {
            return current;
        }
        BPTreeNode *parent = find_parent(current->children[i], child);
        if (parent != NULL)
            return parent;
    }
    return NULL;
}

// Function to search for a key in the B+ Tree
BPTreeNode *search(BPTreeNode *root, int key)
{
    if (root == NULL)
    {
        return NULL;
    }

    int i = 0;
    while (i < root->num_keys && key > root->keys[i])
    {
        i++;
    }

    if (root->is_leaf)
    {
        // Search within the leaf node
        for (int j = 0; j < root->num_keys; j++)
        {
            if (root->keys[j] == key)
            {
                return root; // key found
            }
        }
        return NULL; // key not found
    }
    else
    {
        return search(root->children[i], key);
    }
}

// Function to insert a key into a leaf node
void insert_into_leaf(BPTreeNode *leaf, int key)
{
    int i = leaf->num_keys - 1;
    // Find the position to insert the new key
    while (i >= 0 && leaf->keys[i] > key)
    {
        // Shift the keys to the right one space
        leaf->keys[i + 1] = leaf->keys[i];
        i--;
    }
    leaf->keys[i + 1] = key;
    leaf->num_keys++;
}

// Function to split a leaf node
BPTreeNode *split_leaf(BPTreeNode *leaf, int *new_key)
{
    BPTreeNode *new_leaf = create_node(true);
    int mid = (M + 1) / 2;
    new_leaf->num_keys = leaf->num_keys - mid;

    // Move half the keys to the new leaf
    for (int i = 0; i < new_leaf->num_keys; i++)
    {
        new_leaf->keys[i] = leaf->keys[mid + i];
    }
    leaf->num_keys = mid;

    // Update leaf links
    new_leaf->next = leaf->next;
    leaf->next = new_leaf;

    // The first key of the new leaf is promoted to the parent
    *new_key = new_leaf->keys[0];
    return new_leaf;
}

// Function to insert a key and child into an internal node
void insert_into_internal(BPTreeNode *internal, int key, BPTreeNode *child)
{
    int i = internal->num_keys - 1;
    while (i >= 0 && internal->keys[i] > key)
    {
        internal->keys[i + 1] = internal->keys[i];
        internal->children[i + 2] = internal->children[i + 1];
        i--;
    }

    internal->keys[i + 1] = key;
    internal->children[i + 2] = child;
    internal->num_keys++;
}

// Function to split an internal node
BPTreeNode *split_internal(BPTreeNode *internal, int *new_key)
{
    BPTreeNode *new_internal = create_node(false);
    int mid = M / 2;
    *new_key = internal->keys[mid];
    new_internal->num_keys = internal->num_keys - mid - 1;
    for (int i = 0; i < new_internal->num_keys; i++)
    {
        new_internal->keys[i] = internal->keys[mid + 1 + i];
        new_internal->children[i] = internal->children[mid + 1 + i];
    }
    new_internal->children[new_internal->num_keys] = internal->children[internal->num_keys + 1];
    internal->num_keys = mid;
    return new_internal;
}

// Function to insert into parent after splitting
BPTreeNode *insert_into_parent(BPTreeNode *root, BPTreeNode *left, int key, BPTreeNode *right)
{
    // Case 1: If the node being split is the root, create a new root.
    if (left == root)
    {
        BPTreeNode *new_root = create_node(false);
        new_root->keys[0] = key;
        new_root->children[0] = left;
        new_root->children[1] = right;
        new_root->num_keys = 1;
        return new_root;
    }

    // Case 2: Find the parent of the node being split.
    BPTreeNode *parent = find_parent(root, left);

    // If parent is still not found, there's an inconsistency in the tree.
    if (parent == NULL)
    {
        printf("Parent not found\n");
        return root;
    }

    // Insert the key and right child into the parent node.
    insert_into_internal(parent, key, right);

    // If the parent node has overflowed, split it and recursively insert into its parent.
    if (parent->num_keys == M - 1)
    {
        int new_key;
        BPTreeNode *new_internal = split_internal(parent, &new_key);
        root = insert_into_parent(root, parent, new_key, new_internal);
    }

    return root;
}

// Function to insert a key into the B+ Tree
BPTreeNode *insert(BPTreeNode *root, int key)
{
    if (root == NULL)
    {
        root = create_node(true);
        root->keys[0] = key;
        root->num_keys = 1;
        return root;
    }

    // Find the leaf node
    BPTreeNode *leaf = root;
    while (!leaf->is_leaf)
    {
        int i = 0;
        while (i < leaf->num_keys && key > leaf->keys[i])
        {
            i++;
        }
        leaf = leaf->children[i];
    }

    // Insert the key into the leaf node
    insert_into_leaf(leaf, key);

    // Check for overflow
    if (leaf->num_keys == M)
    {
        int new_key;
        BPTreeNode *new_leaf = split_leaf(leaf, &new_key);

        // Insert the new key into the parent
        root = insert_into_parent(root, leaf, new_key, new_leaf);
    }
    return root;
}

// Function to print the B+ Tree structure
void print_tree(BPTreeNode *root, int level)
{
    if (root == NULL)
        return;

    printf("Level %d: ", level);
    for (int i = 0; i < root->num_keys; i++)
    {
        printf("%d ", root->keys[i]);
    }
    printf("\n");

    if (!root->is_leaf)
    {
        for (int i = 0; i <= root->num_keys; i++)
        {
            print_tree(root->children[i], level + 1);
        }
    }
}

// Function to free all nodes in the B+ Tree
void free_tree(BPTreeNode *root)
{
    if (root == NULL)
    {
        return;
    }

    if (!root->is_leaf)
    {
        for (int i = 0; i <= root->num_keys; i++)
        {
            free_tree(root->children[i]);
        }
    }
    free(root);
}
```

`main.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include "hello.h"

int main()
{
    BPTreeNode *root = NULL;
    int keys[] = {10, 20, 5, 6, 12, 30, 7, 17};
    int n = sizeof(keys) / sizeof(keys[0]);

    for (int i = 0; i < n; i++)
    {
        root = insert(root, keys[i]);
    }

    printf("B+ Tree Structure: \n");
    print_tree(root, 0); // Start from level 0

    int search_key = 6;
    BPTreeNode *result = search(root, search_key);
    if (result != NULL)
    {
        printf("Key %d found in the B+ tree.\n", search_key);
    }
    else
    {
        printf("Key %d not found in the B+ tree.\n", search_key);
    }

    free_tree(root);
    root = NULL;
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==19875== Memcheck, a memory error detector
==19875== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==19875== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==19875== Command: ./final
==19875== 
B+ Tree Structure: 
Level 0: 10 20 
Level 1: 5 6 7 
Level 1: 10 12 17 
Level 1: 20 30 
Key 6 found in the B+ tree.
==19875== 
==19875== HEAP SUMMARY:
==19875==     in use at exit: 0 bytes in 0 blocks
==19875==   total heap usage: 5 allocs, 5 frees, 1,280 bytes allocated
==19875== 
==19875== All heap blocks were freed -- no leaks are possible
==19875== 
==19875== For lists of detected and suppressed errors, rerun with: -s
==19875== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

```css
Level 0:        		[ 10 | 20 ]
               		/      	|        \
Level 1:  [5 | 6 | 7]  [10 | 12 | 17]  [20 | 30]
```

