### Double Hashing

**Double Hashing** is an advanced collision resolution technique in open addressing that uses two distinct hash functions to determine the probe sequence. This method significantly reduces clustering and improves the uniformity of key distribution compared to linear and quadratic probing.

**Key Characteristics:**

- **Two Hash Functions:** Ensures that the probe sequence for each key is unique and less likely to overlap with others.
- **Reduced Clustering:** Minimizes primary and secondary clustering, enhancing performance.
- **Requires Secondary Hash Function to be Non-Zero and Relatively Prime to Table Size:** To ensure that all slots can be probed.

**Advantages:**

1. **Minimizes Clustering:** Double hashing significantly reduces both primary and secondary clustering, leading to more uniform key distribution.
2. **Better Performance:** Maintains efficient probe sequences, especially under high load factors.
3. **Flexible Probing:** Allows customization of the secondary hash function to suit specific needs.

**Disadvantages:**

1. **Complexity:** More complex to implement compared to linear and quadratic probing.
2. **Requires Two Good Hash Functions:** Both hash functions must be carefully designed to ensure they are independent and distribute keys uniformly.
3. **Cannot Probe All Slots If h2(k)h_2(k)h2(k) Shares Common Factors with mmm:** To ensure all slots can be probed, h2(k)h_2(k)h2(k) and mmm should be co-prime.

**Probe Sequence Formula:**

Index = (h<sub>1</sub>(k) + i * h<sub>2</sub>(k)) % m

Where:

- h<sub>1</sub>(k) is the primary hash function.
- h<sub>2</sub>(k) is the secondary hash function.
- m is the size of the hash table.
- i is the probe number (starting from 0).

**Example**:

Suppose h<sub>1</sub>(k) = k % 7, h<sub>2</sub>(k) = 5 - (k % 5), and m = 7.

- Insert 10:
  - h<sub>1</sub>(10) = 3.
  - Place at index 3.
- Insert 17:
  - h<sub>1</sub>(17) = 3 -> Collision
  - h<sub>2</sub>(17) = 5 - (17 % 5) = 5 - 2 = 3.
  - Probe 1: (3 + 1 * 3) % 7 = 6
  - Place at index 6.
- Insert 24:
  - h<sub>1</sub>(24) = 3 -> Collision.
  - h<sub>2</sub>(24) = 5 - (24 % 5) = 5 - 4 = 1
  - Probe 1: (3 + 1 * 1) % 7 = 4.
  - Place at index 4.

### C Code Example (Double Hashing)

#### Program Code

`practice.h`

```C
#define INITIAL_SIZE 7
#define LOAD_FACTOR_THRESHOLD 0.5

typedef struct HashEntry
{
    int key;
    int value;
    bool isOccupied;
    bool isDeleted;
} HashEntry;

typedef struct HashTable
{
    int size;
    int count;
    HashEntry *table;
} HashTable;

int hashFunction1(int key, int size);
int hashFunction2(int key, int size);
HashTable *createHashTable(int size);
bool needsResize(HashTable *ht);
void resizeAndRehash(HashTable **ht_ref, int new_size);
void insert(HashTable **ht_ref, int key, int value);
int search(HashTable *ht, int key);
void deleteKey(HashTable **ht_ref, int key);
void display(HashTable *ht);
void freeHashTable(HashTable *ht);
```

`functions.c`

```C
// Primary Hash Function
int hashFunction1(int key, int size) {
    return key % size;
}

// Secondary Hash Function
int hashFunction2(int key, int size) {
    return 5 - (key % 5); // Must be non-zero and relatively prime to size
}

// Create a new hash table
HashTable* createHashTable(int size) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (!ht) {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    ht->size = size;
    ht->count = 0;
    ht->table = (HashEntry*) malloc(sizeof(HashEntry) * size);
    if (!ht->table) {
        printf("Memory allocation failed for HashTable entries.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++) {
        ht->table[i].isOccupied = false;
        ht->table[i].isDeleted = false;
    }
    return ht;
}

// Check if the hash table needs resizing
bool needsResize(HashTable* ht) {
    double load_factor = (double) ht->count / ht->size;
    return load_factor > LOAD_FACTOR_THRESHOLD;
}

// Resize and rehash the hash table
void resizeAndRehash(HashTable** ht_ref, int new_size) {
    HashTable* old_ht = *ht_ref;
    HashTable* new_ht = createHashTable(new_size);

    for (int i = 0; i < old_ht->size; i++) {
        if (old_ht->table[i].isOccupied && !old_ht->table[i].isDeleted) {
            // Insert into new hash table using double hashing
            int key = old_ht->table[i].key;
            int value = old_ht->table[i].value;

            // Calculate new indices using new size
            int h1 = hashFunction1(key, new_ht->size);
            int h2 = hashFunction2(key, new_ht->size);
            int index = h1;
            int j = 1;

            while (new_ht->table[index].isOccupied) {
                index = (h1 + j * h2) % new_ht->size;
                j++;
                if (j == new_ht->size) {
                    printf("Failed to rehash key %d during resizing.\n", key);
                    break;
                }
            }

            if (!new_ht->table[index].isOccupied) {
                new_ht->table[index].key = key;
                new_ht->table[index].value = value;
                new_ht->table[index].isOccupied = true;
                new_ht->table[index].isDeleted = false;
                new_ht->count++;
            }
        }
    }

    // Free the old table
    free(old_ht->table);
    free(old_ht);

    // Update the reference
    *ht_ref = new_ht;
    printf("Resized and rehashed to new size %d.\n", new_size);
}

// Insert key-value pair using Double Hashing
void insert(HashTable** ht_ref, int key, int value) {
    HashTable* ht = *ht_ref;

    if (needsResize(ht)) {
        resizeAndRehash(ht_ref, ht->size * 2); // Double the size
        ht = *ht_ref; // Update the local reference after resizing
    }

    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied && !ht->table[index].isDeleted && ht->table[index].key != key) {
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            printf("Hash table is full. Cannot insert key %d.\n", key);
            return;
        }// Primary Hash Function
int hashFunction1(int key, int size) {
    return key % size;
}

// Secondary Hash Function
int hashFunction2(int key, int size) {
    return 5 - (key % 5); // Must be non-zero and relatively prime to size
}

// Create a new hash table
HashTable* createHashTable(int size) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (!ht) {
        printf("Memory allocation failed for HashTable.\n");
        exit(EXIT_FAILURE);
    }
    ht->size = size;
    ht->count = 0;
    ht->table = (HashEntry*) malloc(sizeof(HashEntry) * size);
    if (!ht->table) {
        printf("Memory allocation failed for HashTable entries.\n");
        exit(EXIT_FAILURE);
    }
    for (int i = 0; i < size; i++) {
        ht->table[i].isOccupied = false;
        ht->table[i].isDeleted = false;
    }
    return ht;
}

// Check if the hash table needs resizing
bool needsResize(HashTable* ht) {
    double load_factor = (double) ht->count / ht->size;
    return load_factor > LOAD_FACTOR_THRESHOLD;
}

// Resize and rehash the hash table
void resizeAndRehash(HashTable** ht_ref, int new_size) {
    HashTable* old_ht = *ht_ref;
    HashTable* new_ht = createHashTable(new_size);

    for (int i = 0; i < old_ht->size; i++) {
        if (old_ht->table[i].isOccupied && !old_ht->table[i].isDeleted) {
            // Insert into new hash table using double hashing
            int key = old_ht->table[i].key;
            int value = old_ht->table[i].value;

            // Calculate new indices using new size
            int h1 = hashFunction1(key, new_ht->size);
            int h2 = hashFunction2(key, new_ht->size);
            int index = h1;
            int j = 1; // the probe

            while (new_ht->table[index].isOccupied) {
                index = (h1 + j * h2) % new_ht->size;
                j++;
                if (j == new_ht->size) {
                    printf("Failed to rehash key %d during resizing.\n", key);
                    break;
                }
            }

            if (!new_ht->table[index].isOccupied) {
                new_ht->table[index].key = key;
                new_ht->table[index].value = value;
                new_ht->table[index].isOccupied = true;
                new_ht->table[index].isDeleted = false;
                new_ht->count++;
            }
        }
    }

    // Free the old table
    free(old_ht->table);
    free(old_ht);

    // Update the reference
    *ht_ref = new_ht;
    printf("Resized and rehashed to new size %d.\n", new_size);
}

// Insert key-value pair using Double Hashing
void insert(HashTable** ht_ref, int key, int value) {
    HashTable* ht = *ht_ref;

    if (needsResize(ht)) {
        resizeAndRehash(ht_ref, ht->size * 2); // Double the size
        ht = *ht_ref; // Update the local reference after resizing
    }

    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1; // the probe

    while (ht->table[index].isOccupied && !ht->table[index].isDeleted && ht->table[index].key != key) {
        index = (h1 + j
    }

    if (!ht->table[index].isOccupied || ht->table[index].isDeleted) {
        ht->table[index].key = key;
        ht->table[index].value = value;
        ht->table[index].isOccupied = true;
        ht->table[index].isDeleted = false;
        ht->count++;
        printf("Inserted key %d with value %d at index %d.\n", key, value, index);
    } else {
        // Key already exists, update the value
        ht->table[index].value = value;
        printf("Updated key %d with new value %d at index %d.\n", key, value, index);
    }
}

// Search for a key using Double Hashing
int search(HashTable* ht, int key) {
    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied) {
        if (!ht->table[index].isDeleted && ht->table[index].key == key) {
            return ht->table[index].value;
        }
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            break; // Searched all slots
        }
    }
    return -1; // Not found
}

// Delete a key using Double Hashing
void deleteKey(HashTable** ht_ref, int key) {
    HashTable* ht = *ht_ref;
    int h1 = hashFunction1(key, ht->size);
    int h2 = hashFunction2(key, ht->size);
    int index = h1;
    int j = 1;

    while (ht->table[index].isOccupied) {
        if (!ht->table[index].isDeleted && ht->table[index].key == key) {
            ht->table[index].isDeleted = true;
            ht->count--;
            printf("Key %d deleted from index %d.\n", key, index);

            // Resize down if load factor is too low
            double load_factor = (double) ht->count / ht->size;
            if (load_factor < (LOAD_FACTOR_THRESHOLD / 2) && ht->size > INITIAL_SIZE) {
                resizeAndRehash(ht_ref, ht->size / 2); // Halve the size
            }
            return;
        }
        index = (h1 + j * h2) % ht->size;
        j++;
        if (j == ht->size) {
            break; // Searched all slots
        }
    }
    printf("Key %d not found. Cannot delete.\n", key);
}

// Display the hash table
void display(HashTable* ht) {
    printf("\nHash Table Contents (Size: %d, Count: %d):\n", ht->size, ht->count);
    for (int i = 0; i < ht->size; i++) {
        printf("Slot %d: ", i);
        if (ht->table[i].isOccupied && !ht->table[i].isDeleted) {
            printf("(%d, %d)", ht->table[i].key, ht->table[i].value);
        } else if (ht->table[i].isDeleted) {
            printf("DELETED");
        } else {
            printf("NULL");
        }
        printf("\n");
    }
}

// Free the hash table
void freeHashTable(HashTable* ht) {
    free(ht->table);
    free(ht);
    printf("Hash table memory freed.\n");
}
```

`practice.c`

```C
int main() {
    // Create a hash table with initial size
    HashTable* table = createHashTable(INITIAL_SIZE);

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

    // Search for keys
    int key = 15;
    int value = search(table, key);
    if (value != -1) {
        printf("\nKey %d found with value %d.\n", key, value);
    } else {
        printf("\nKey %d not found.\n", key);
    }

    key = 99;
    value = search(table, key);
    if (value != -1) {
        printf("Key %d found with value %d.\n", key, value);
    } else {
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
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==15629== Memcheck, a memory error detector
==15629== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15629== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15629== Command: ./practice
==15629== 
Inserted key 10 with value 100 at index 3
Inserted key 20 with value 200 at index 6
Inserted key 15 with value 150 at index 1
Inserted key 7 with value 70 at index 0
Resized and rehashed to new size 14
Inserted key 8 with value 80 at index 8
Inserted key 22 with value 220 at index 11
Inserted key 1 with value 10 at index 5

Hash Table Contents (Size: 14, Count: 7):
Slot 0: NULL
Slot 1: (15, 150)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: (1, 10)
Slot 6: (20, 200)
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: (22, 220)
Slot 12: NULL
Slot 13: NULL
Inserted key 17 with value 170 at index 3
Resized and rehashed to new size 28
Inserted key 27 with value 270 at index 27

Hash Table Contents (Size: 28, Count: 9):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: (7, 70)
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: (20, 200)
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)

Key 15 found with value 150.

Key 99 not found.
Key 20 deleted from index 20.
Key 7 deleted from index 7.

Hash Table Contents (Size: 28, Count: 7):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: NULL
Slot 4: NULL
Slot 5: NULL
Slot 6: NULL
Slot 7: DELETED
Slot 8: (8, 80)
Slot 9: NULL
Slot 10: (10, 100)
Slot 11: NULL
Slot 12: NULL
Slot 13: NULL
Slot 14: NULL
Slot 15: (15, 150)
Slot 16: NULL
Slot 17: (17, 170)
Slot 18: NULL
Slot 19: NULL
Slot 20: DELETED
Slot 21: NULL
Slot 22: (22, 220)
Slot 23: NULL
Slot 24: NULL
Slot 25: NULL
Slot 26: NULL
Slot 27: (27, 270)
Key 10 deleted from index 10.
Resized and rehashed to new size 14
Key 15 deleted from index 6.
Key 8 deleted from index 8.

Hash Table Contents (Size: 14, Count: 4):
Slot 0: NULL
Slot 1: (1, 10)
Slot 2: NULL
Slot 3: (17, 170)
Slot 4: NULL
Slot 5: NULL
Slot 6: DELETED
Slot 7: NULL
Slot 8: DELETED
Slot 9: NULL
Slot 10: NULL
Slot 11: (22, 220)
Slot 12: NULL
Slot 13: (27, 270)
Key 100 not found. Cannot delete.
Hash table memory freed.
==15629== 
==15629== HEAP SUMMARY:
==15629==     in use at exit: 0 bytes in 0 blocks
==15629==   total heap usage: 9 allocs, 9 frees, 1,844 bytes allocated
==15629== 
==15629== All heap blocks were freed -- no leaks are possible
==15629== 
==15629== For lists of detected and suppressed errors, rerun with: -s
==15629== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

