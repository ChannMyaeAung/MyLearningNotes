# Implementation of B+Tree in C

`hello.h`

```C
#include <stdbool.h>
#ifndef HELLO_H
#define HELLO_H

#define M 4 //

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
void print_tree(BPTreeNode *root);

void free_tree(BPTreeNode *root);
#endif // HELLO_H
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

void insert_into_leaf(BPTreeNode *leaf, int key)
{
    int i = leaf->num_keys - 1;
    // find the position to insert the new key
    while (i >= 0 && leaf->keys[i] > key)
    {

        // Shift the keys to the right one space
        leaf->keys[i + 1] = leaf->keys[i];
        i--;
    }
    leaf->keys[i + 1] = key;
    leaf->num_keys++;
}

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

// function to split an internal node and return the new internal node
BPTreeNode *split_internal(BPTreeNode *internal, int *new_key)
{
    BPTreeNode *new_internal = create_node(false);
    int mid = M / 2;
    *new_key = internal->keys[mid];
    new_internal->num_keys = internal->num_keys - mid - 1;

    // Move keys and children to the new internal node
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
    BPTreeNode *parent = NULL;
    BPTreeNode *current = root;

    while (!current->is_leaf)
    {
        int i;
        for (i = 0; i < current->num_keys; i++)
        {
            if (current->children[i] == left)
            {
                parent = current;
                break;
            }
            else if (key < current->keys[i])
            {
                current = current->children[i];
                break;
            }
            else if (left->keys[0] > current->keys[i])
            {
                // Continue to the next child if the new key is greater
                if (i == current->num_keys - 1)
                {
                    current = current->children[i + 1];
                    break;
                }
            }
        }

        if (parent != NULL)
        {
            break;
        }

        // If we have iterated through all keys and not found the parent
        if (i == current->num_keys)
        {
            current = current->children[i];
        }
    }

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

    // insert the key into the leaf node
    insert_into_leaf(leaf, key);

    // Check for overflow
    if (leaf->num_keys == M)
    {
        int new_key;
        BPTreeNode *new_leaf = split_leaf(leaf, &new_key);

        // insert the new key into the parent
        root = insert_into_parent(root, leaf, new_key, new_leaf);
    }
    return root;
}

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
    print_tree(root, 0);

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

