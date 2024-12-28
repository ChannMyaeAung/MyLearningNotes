### Understanding Dynamic Resizing and Rehashing

#### **Dynamic Resizing**

**Dynamic Resizing** refers to the ability of a hash table to adjust its number of buckets (i.e., its size) based on the number of elements it contains. When the number of elements grows, the hash table can **resize** to accommodate more elements, reducing the load factor and minimizing collisions. Conversely, if the number of elements decreases significantly, the table can shrink to save memory.

**Key Points:**

- **Load Factor (α):** Defined as `α = n/m`, where `n` is the number of elements and `m` is the number of buckets. A higher load factor increases the chance of collisions.
- **Threshold:** A predefined load factor threshold (commonly around `0.7`) triggers resizing. When `α > 0.7`, the table resizes to maintain performance.

#### **Rehashing**

**Rehashing** is the process of recalculating the hash indices for all existing elements and moving them into a new set of buckets after the table has been resized. This ensures that the elements are evenly distributed according to the new table size.

**Key Points:**

- **Necessary After Resizing:** Since the number of buckets changes, the hash indices of keys typically change, necessitating rehashing.
- **Efficiency:** Rehashing ensures that the hash table maintains its performance characteristics by reducing collisions.

### Implementing a Hash Table with Dynamic Resizing and Rehashing in C

#### Program Code

`practice.h`

```C
#define LOAD_FACTOR_THRESHOLD 0.7
typedef struct HashNode
{
    int key;
    int value;
    struct HashNode *next;
} HashNode;

typedef struct HashTable
{
    int size; // Number of buckets
    int count;
    HashNode **buckets; // Array of pointers to HashNode
} HashTable;

int hashFunction(int key, int size);

HashTable *createHashTable(int size);
// Insert key-value pair into hash table
void insert(HashTable **table_ref, int key, int value);

// Search for a key in the hash table
int search(HashTable *table, int key);

// Delete a key from the hash table
void deleteKey(HashTable **table_ref, int key);

// Resize and rehash the hash table
void resizeAndRehash(HashTable **table_ref, int new_size);
void display(HashTable *table);

void freeHashTable(HashTable *table);
```

`functions.c`

```C
int hashFunction(int key, int size)
{
    return key % size;
}

// Create a new hash table
HashTable *createHashTable(int size)
{
    HashTable *table = (HashTable *)malloc(sizeof(HashTable));
    if (!table)
    {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    table->size = size;
    table->count = 0;
    table->buckets = (HashNode **)malloc(size * sizeof(HashNode *));
    if (!table->buckets)
    {
        printf("Memory allocation failed for buckets.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++)
    {
        table->buckets[i] = NULL;
    }
    return table;
}

// Insert function with dynamic resizing
void insert(HashTable **table_ref, int key, int value)
{
    HashTable *table = *table_ref;
    int index = hashFunction(key, table->size);

    // Create a new node
    HashNode *newNode = (HashNode *)malloc(sizeof(HashNode));
    if (!newNode)
    {
        printf("Memory allocation failed for HashNode.\n");
        exit(EXIT_FAILURE);
    }
    newNode->key = key;
    newNode->value = value;
    newNode->next = table->buckets[index];
    table->buckets[index] = newNode;
    table->count++;

    printf("Inserted key %d with value %d at index %d.\n", key, value, index);

    // Calculate load factor
    double load_factor = (double)table->count / table->size;
    if (load_factor > LOAD_FACTOR_THRESHOLD)
    {
        // Resize to double the current size
        resizeAndRehash(table_ref, table->size * 2);
    }
}

// Search function
int search(HashTable *table, int key)
{
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    while (node != NULL)
    {
        if (node->key == key)
        {
            return node->value;
        }
        node = node->next;
    }
    // Key not found
    return -1;
}

// Delete function with optional resizing down
void deleteKey(HashTable **table_ref, int key)
{
    HashTable *table = *table_ref;
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    HashNode *prev = NULL;

    while (node != NULL && node->key != key)
    {
        prev = node;
        node = node->next;
    }

    if (node == NULL)
    {
        printf("Key %d not found. Cannot delete.\n", key);
        return;
    }

    if (prev == NULL)
    {
        // Deleting the first node in the bucket
        table->buckets[index] = node->next;
    }
    else
    {
        prev->next = node->next;
    }

    free(node);
    table->count--;
    printf("Key %d deleted from index %d.\n", key, index);

    // Optional: Resize down if load factor is too low
    double load_factor = (double)table->count / table->size;
    if (load_factor < (LOAD_FACTOR_THRESHOLD / 2) && table->size > 7)
    { // Minimum size of 7
        resizeAndRehash(table_ref, table->size / 2);
    }
}

// Resize and rehash function
void resizeAndRehash(HashTable **table_ref, int new_size)
{
    HashTable *old_table = *table_ref;
    HashTable *new_table = createHashTable(new_size);

    printf("Resizing from %d to %d buckets and rehashing...\n", old_table->size, new_size);

    for (int i = 0; i < old_table->size; i++)
    {
        HashNode *node = old_table->buckets[i];
        while (node != NULL)
        {
            insert(&new_table, node->key, node->value);
            node = node->next;
        }
    }
    
    // Free all original nodes in the old table
    for (int i = 0; i < old_table->size; i++)
    {
        HashNode *node = old_table->buckets[i];
        while (node != NULL)
        {
            HashNode *temp = node;
            node = node->next;
            free(temp);
        }
    }

    // Free the old table's buckets array
    free(old_table->buckets);
    // Free the old table structure
    free(old_table);
    // Update the table reference to point to the new table
    *table_ref = new_table;
    printf("Resizing and rehashing completed.\n");
}

// Display function
void display(HashTable *table)
{
    printf("\nHash Table Contents (Size: %d, Count: %d):\n", table->size, table->count);
    for (int i = 0; i < table->size; i++)
    {
        printf("Bucket %d: ", i);
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            printf("(%d, %d) -> ", node->key, node->value);
            node = node->next;
        }
        printf("NULL\n");
    }
}

void freeHashTable(HashTable *table)
{
    for (int i = 0; i < table->size; i++)
    {
        HashNode *node = table->buckets[i];
        while (node != NULL)
        {
            HashNode *temp = node;
            node = node->next;
            free(temp);
        }
    }
    free(table->buckets);
    free(table);
    printf("Hash table memory freed.\n");
}
```

`practice.c`

```C
int main()
{
    // Create a hash table with an initial size of 7 buckets
    HashTable *table = createHashTable(7);

    // Insert key-value pairs
    insert(&table, 10, 100);
    insert(&table, 20, 200);
    insert(&table, 15, 150);
    insert(&table, 7, 70);
    insert(&table, 8, 80);
    insert(&table, 22, 220);
    insert(&table, 1, 10);

    // Display the hash table
    display(table);

    // Insert more elements to trigger resizing
    insert(&table, 17, 170);
    insert(&table, 27, 270);

    // Display the hash table after resizing
    display(table);

    // Search for existing and non-existing keys
    int key = 15;
    int value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d.\n", key, value);
    }
    else
    {
        printf("\nKey %d not found.\n", key);
    }

    key = 99;
    value = search(table, key);
    if (value != -1)
    {
        printf("Key %d found with value %d.\n", key, value);
    }
    else
    {
        printf("Key %d not found.\n", key);
    }

    // Delete some keys
    deleteKey(&table, 20);
    deleteKey(&table, 7);

    // Display the hash table after deletions
    display(table);

    // Delete more keys to potentially trigger resizing down
    deleteKey(&table, 10);
    deleteKey(&table, 15);
    deleteKey(&table, 8);

    // Display the hash table after further deletions
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(&table, 100);

    // Free the hash table
    freeHashTable(table);

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted key 10 with value 100 at index 3.
Inserted key 20 with value 200 at index 6.
Inserted key 15 with value 150 at index 1.
Inserted key 7 with value 70 at index 0.
Inserted key 8 with value 80 at index 1.
Resizing from 7 to 14 buckets and rehashing...
Inserted key 7 with value 70 at index 7.
Inserted key 8 with value 80 at index 8.
Inserted key 15 with value 150 at index 1.
Inserted key 10 with value 100 at index 10.
Inserted key 20 with value 200 at index 6.
Resizing and rehashing completed.
Inserted key 22 with value 220 at index 8.
Inserted key 1 with value 10 at index 1.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: NULL
Inserted key 17 with value 170 at index 3.
Inserted key 27 with value 270 at index 13.

Hash Table Contents (Size: 14, Count: 9):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL

Key 15 found with value 150.
Key 99 not found.
Key 20 deleted from index 6.
Key 7 deleted from index 7.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Bucket 7: NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL
Key 10 deleted from index 10.
Key 15 deleted from index 1.
Key 8 deleted from index 8.
Resizing from 14 to 7 buckets and rehashing...
Inserted key 1 with value 10 at index 1.
Inserted key 17 with value 170 at index 3.
Inserted key 22 with value 220 at index 1.
Inserted key 27 with value 270 at index 6.
Resizing and rehashing completed.

Hash Table Contents (Size: 7, Count: 4):
Bucket 0: NULL
Bucket 1: (22, 220) -> (1, 10) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (27, 270) -> NULL
Key 100 not found. Cannot delete.
Hash table memory freed.

```

#### Checking Memory Leak

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==8613== Memcheck, a memory error detector
==8613== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==8613== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==8613== Command: ./practice
==8613== 
Inserted key 10 with value 100 at index 3.
Inserted key 20 with value 200 at index 6.
Inserted key 15 with value 150 at index 1.
Inserted key 7 with value 70 at index 0.
Inserted key 8 with value 80 at index 1.
Resizing from 7 to 14 buckets and rehashing...
Inserted key 7 with value 70 at index 7.
Inserted key 8 with value 80 at index 8.
Inserted key 15 with value 150 at index 1.
Inserted key 10 with value 100 at index 10.
Inserted key 20 with value 200 at index 6.
Resizing and rehashing completed.
Inserted key 22 with value 220 at index 8.
Inserted key 1 with value 10 at index 1.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: NULL
Inserted key 17 with value 170 at index 3.
Inserted key 27 with value 270 at index 13.

Hash Table Contents (Size: 14, Count: 9):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL
Bucket 7: (7, 70) -> NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL

Key 15 found with value 150.
Key 99 not found.
Key 20 deleted from index 6.
Key 7 deleted from index 7.

Hash Table Contents (Size: 14, Count: 7):
Bucket 0: NULL
Bucket 1: (1, 10) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Bucket 7: NULL
Bucket 8: (22, 220) -> (8, 80) -> NULL
Bucket 9: NULL
Bucket 10: (10, 100) -> NULL
Bucket 11: NULL
Bucket 12: NULL
Bucket 13: (27, 270) -> NULL
Key 10 deleted from index 10.
Key 15 deleted from index 1.
Key 8 deleted from index 8.
Resizing from 14 to 7 buckets and rehashing...
Inserted key 1 with value 10 at index 1.
Inserted key 17 with value 170 at index 3.
Inserted key 22 with value 220 at index 1.
Inserted key 27 with value 270 at index 6.
Resizing and rehashing completed.

Hash Table Contents (Size: 7, Count: 4):
Bucket 0: NULL
Bucket 1: (22, 220) -> (1, 10) -> NULL
Bucket 2: NULL
Bucket 3: (17, 170) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (27, 270) -> NULL
Key 100 not found. Cannot delete.
Hash table memory freed.
==8613== 
==8613== HEAP SUMMARY:
==8613==     in use at exit: 0 bytes in 0 blocks
==8613==   total heap usage: 25 allocs, 25 frees, 1,584 bytes allocated
==8613== 
==8613== All heap blocks were freed -- no leaks are possible
==8613== 
==8613== For lists of detected and suppressed errors, rerun with: -s
==8613== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

