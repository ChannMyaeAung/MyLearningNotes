### C Code Example: Hash Table with Separate Chaining

We'll implement a simple hash table in C using **Separate Chaining** to handle collisions. This approach uses an array of linked lists, where each linked list represents a bucket.

#### Structure Definitions

1. **Hash Table Node:**

   Each node in the linked list represents a key-value pair.

   ```c
   typedef struct HashNode {
       int key;
       int value;
       struct HashNode* next;
   } HashNode;
   ```

2. **Hash Table:**

   The hash table contains an array of pointers to `HashNode` linked lists.

   ```c
   typedef struct HashTable {
       int size;           // Number of buckets
       HashNode** buckets; // Array of pointers to HashNode
   } HashTable;
   ```

#### Hash Function

A simple hash function using the modulo operator:

```C
int hashFunction(int key, int size) {
    return key % size;
}
```

#### Program Code

`practice.h`

```C
typedef struct HashNode{
    int key;
    int value;
    struct HashNode *next;
}HashNode;

typedef struct HashTable{
    int size; // Number of buckets
    HashNode **buckets; // Array of pointers to HashNode
} HashTable;

int hashFunction(int key, int size);

HashTable *createHashTable(int size);
void insert(HashTable *table, int key, int value);
int search(HashTable *table, int key);
void deleteKey(HashTable *table, int key);
void display(HashTable *table);

void freeHashTable(HashTable *table);
```

`functions.c`

```C
int hashFunction(int key, int size)
{
    return key % size;
}

HashTable *createHashTable(int size)
{
    HashTable *table = malloc(sizeof(HashTable));
    if (!table)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }

    table->size = size;
    table->buckets = malloc(size * sizeof(HashNode *));
    if (!table->buckets)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }
    for (int i = 0; i < size; i++)
    {
        table->buckets[i] = NULL;
    }
    return table;
}
void insert(HashTable *table, int key, int value)
{
    int index = hashFunction(key, table->size);
    HashNode *newNode = malloc(sizeof(HashNode));

    if (!newNode)
    {
        printf("Error: Memory allocation failed\n");
        exit(1);
    }
    newNode->key = key;
    newNode->value = value;
    newNode->next = table->buckets[index];
    table->buckets[index] = newNode;
    printf("Inserted key %d with value %d at index %d\n", key, value, index);
}
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
    // key not found
    return -1;
}
void deleteKey(HashTable *table, int key)
{
    int index = hashFunction(key, table->size);
    HashNode *node = table->buckets[index];
    HashNode *prev = NULL;
    while (node != NULL && node->key != key)
    {

        // place the current node in the prev variable
        prev = node;

        // move to the next node, now the current node is the next node
        node = node->next;
    }

    if (node == NULL)
    {
        printf("Key %d not found.\n", key);
        return;
    }

    if (prev == NULL)
    {
        // deleting the first node in the bucket
        table->buckets[index] = node->next;
    }
    else
    {
        // e.g set the 1st node to point to the 3rd node
        prev->next = node->next;
    }

    free(node);
    printf("Key %d deleted from index %d.\n", key, index);
}
void display(HashTable *table)
{
    printf("\nHash Table Contents:\n");
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
            // save the current node in tmp
            HashNode *tmp = node;

            // set the current node to point to the next node
            node = node->next;

            // free the tmp node effectively freeing the previous node
            free(tmp);
        }
    }
    free(table->buckets);
    free(table);
}
```

`practice.c`

```C
nt main()
{
    // create a hash table with 7 buckets
    HashTable *table = createHashTable(7);

    // Insert key-value pairs
    insert(table, 10, 100);
    insert(table, 20, 200);
    insert(table, 15, 150);
    insert(table, 7, 70);
    insert(table, 8, 80);
    insert(table, 22, 220);
    insert(table, 1, 10);

    display(table);

    // search for keys
    int key = 15;
    int value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d\n", key, value);
    }
    else
    {
        printf("\nKey %d not found\n", key);
    }

    // attempting to search for a non-existent key
    key = 99;
    value = search(table, key);
    if (value != -1)
    {
        printf("\nKey %d found with value %d\n", key, value);
    }
    else
    {
        printf("\nKey %d not found\n", key);
    }

    // delete a key
    deleteKey(table, 20);

    // display the updated hash table
    display(table);

    // Attempt to delete a non-existent key
    deleteKey(table, 99);

    // free the hash table
    freeHashTable(table);

    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted key 10 with value 100 at index 3
Inserted key 20 with value 200 at index 6
Inserted key 15 with value 150 at index 1
Inserted key 7 with value 70 at index 0
Inserted key 8 with value 80 at index 1
Inserted key 22 with value 220 at index 1
Inserted key 1 with value 10 at index 1

Hash Table Contents:
Bucket 0: (7, 70) -> NULL
Bucket 1: (1, 10) -> (22, 220) -> (8, 80) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (10, 100) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: (20, 200) -> NULL

Key 15 found with value 150

Key 99 found with value 1
Key 20 deleted from index 6.

Hash Table Contents:
Bucket 0: (7, 70) -> NULL
Bucket 1: (1, 10) -> (22, 220) -> (8, 80) -> (15, 150) -> NULL
Bucket 2: NULL
Bucket 3: (10, 100) -> NULL
Bucket 4: NULL
Bucket 5: NULL
Bucket 6: NULL
Key 99 not found.
```

---

### C Code Example (Chaining) 

#### Program Code

`practice.h`

```C
#define MAX_NAME 256
#define TABLE_SIZE 10

typedef struct person
{
    char name[MAX_NAME];
    int age;
    struct person *next;
} person;

unsigned int hash(char *name);
void init_hash_table();

bool hash_table_insert(person *p);

person *hash_table_lookup(char *name);
person *hash_table_delete(char *name);
void print_table();
```

`functions.c`

```C
person *hash_table[TABLE_SIZE];

// computes an index based on the person's name
unsigned int hash(char *name)
{
    int length = strnlen(name, MAX_NAME);
    unsigned int hash_value = 0;
    for (int i = 0; i < length; i++)
    {
        hash_value += name[i];
        hash_value = (hash_value * name[i]) % TABLE_SIZE;
    }
    return hash_value;
}

// Initialize hash table by setting all slots to NULL
void init_hash_table()
{
    for (int i = 0; i < TABLE_SIZE; i++)
    {
        hash_table[i] = NULL;
    }
}

bool hash_table_insert(person *p)
{
    if (p == NULL)
    {
        return false;
    }

    // get a random hash value between 0 and TABLE_SIZE and set it to index
    int index = hash(p->name);

    // Insert at the beginning of the linked list for separate chaining
    // example: jacob.next = hash_table[2] which is NULL
    // hash_table[2] = &jacob
    p->next = hash_table[index];
    hash_table[index] = p;
    return true;
}

person *hash_table_lookup(char *name)
{
    int index = hash(name);
    person *tmp = hash_table[index];
    
    // traverse the list until the match is found
    while (tmp != NULL && strncmp(tmp->name, name, MAX_NAME) != 0)
    {
        tmp = tmp->next;
    }
    
    // return the matched name
    return tmp;
}


person *hash_table_delete(char *name)
{
    unsigned int index = hash(name);

    person *tmp = hash_table[index];
    person *prev = NULL;
    while (tmp != NULL && strncmp(tmp->name, name, MAX_NAME) != 0)
    {
        prev = tmp;
        tmp = tmp->next;
    }

    if (tmp == NULL)
    {
        return NULL; // Not found
    }

    if (prev == NULL)
    {
        // Deleting the first node in the linked list
        hash_table[index] = tmp->next;
    }
    else
    {
        // Deleting a node beyond the first in the linked list
        prev->next = tmp->next;
    }
    return tmp; // Return the deleted node
}

void print_table()
{
    printf("Start\n");
    for (int i = 0; i < TABLE_SIZE; i++)
    {
        if (hash_table[i] == NULL)
        {
            printf("\t%i\t---\n", i);
        }
        else
        {
            printf("\t%i\t", i);
            person *tmp = hash_table[i];
            while (tmp != NULL)
            {
                printf("%s - ", tmp->name);
                tmp = tmp->next;
            }
            printf("\n");
        }
    }
    printf("End\n");
}
```

- Delete Operation:
  - **Deleting "Kate"**:
    - **Hash Index**: `hash("Kate") = 2`.
    - **Search**: Traverse `hash_table[2]` to find "Kate".
    - **Deletion:**
      - Update `prev->next` to skip "Kate".
      - If "Kate" is the first node, update `hash_table[2]` to `kate.next`.
  - **Lookup "Kate" Again**:
    - **Result**: "Kate" not found.

`practice.c`

```C
int main()
{
    init_hash_table();
    print_table();
    person jacob = {.name = "Jacob", .age = 25};
    person kate = {.name = "Kate", .age = 28};
    person sarah = {.name = "Sarah", .age = 32};
    person penc = {.name = "PENC", .age = 24};
    person john = {.name = "John", .age = 27};
    person moon = {.name = "Moon", .age = 26};
    person johnson = {.name = "Johnson", .age = 19};
    person joseph = {.name = "Joseph", .age = 21};
    person steve = {.name = "Steve", .age = 22};
    person ember = {.name = "Ember", .age = 56};

    hash_table_insert(&jacob);
    hash_table_insert(&kate);
    hash_table_insert(&sarah);
    hash_table_insert(&penc);
    hash_table_insert(&john);
    hash_table_insert(&moon);
    hash_table_insert(&johnson);
    hash_table_insert(&joseph);
    hash_table_insert(&steve);
    hash_table_insert(&ember);
    print_table();

    person *tmp = hash_table_lookup("PENC");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    tmp = hash_table_lookup("Jacob");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    hash_table_delete("Kate");
    tmp = hash_table_lookup("Kate");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    print_table();
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Start
	0	---
	1	---
	2	---
	3	---
	4	---
	5	---
	6	---
	7	---
	8	---
	9	---
End
Start
	0	Joseph - Johnson - Moon - John - 
	1	Kate - 
	2	Jacob - 
	3	Steve - PENC - 
	4	Sarah - 
	5	---
	6	Ember - 
	7	---
	8	---
	9	---
End
Found PENC
Found Jacob
Not found
Start
	0	Joseph - Johnson - Moon - John - 
	1	---
	2	Jacob - 
	3	Steve - PENC - 
	4	Sarah - 
	5	---
	6	Ember - 
	7	---
	8	---
	9	---
End

```

