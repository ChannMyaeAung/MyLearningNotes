## Generic Containers

In the C programming language, **generic containers** refer to data structures that can store elements of any data type. Unlike languages that natively support generics (such as C++ with templates or Java with generics), C does not provide built-in mechanisms for creating truly type-agnostic data structures. However, developers have devised various techniques to implement generic behavior in C, enabling the creation of versatile and reusable containers.

- **Generic containers** are data structures designed to handle multiple data types without requiring specific implementations for each type. 
- They enhance code reusability and flexibility, allowing developers to write more abstract and maintainable code.
- In C, achieving genericity involves creating containers that can operate on data of any type, typically by abstracting the data type through pointers or other mechanisms.

### Techniques to Implement Generic Containers in C

#### 1. Using `void*` Pointers

The most common method to achieve genericity in C is by using `void*` (pointer to `void`) types. Since `void*` can point to any data type, it allows functions and data structures to handle different types uniformly.

**Pros:**

- Simple to implement.
- Highly flexible.

**Cons:**

- Lack of type safety.
- Requires explicit casting, which can lead to runtime errors.
- Potential performance overhead due to pointer dereferencing.

#### 2. Macros

C preprocessor macros can generate type-specific code during compilation. By defining macros that take the data type as a parameter, developers can create type-safe generic containers.

**Pros:**

- Type safety is enforced at compile-time.
- No runtime overhead.

**Cons:**

- Code can become harder to read and debug.
- Increased compilation time.
- Potential for code bloat due to multiple instances of similar code.

#### 3. Code Generation

Using external scripts or tools to generate type-specific implementations from a generic template. Tools like `m4`, `GPP`, or custom scripts can automate this process.

**Pros:**

- Maintains type safety.
- Reduces manual coding effort.

**Cons:**

- Adds complexity to the build process.
- Requires maintaining separate template files.

#### 4. `_Generic` Keyword (C11)

Introduced in the C11 standard, the `_Generic` keyword allows for compile-time type selection, enabling a form of generic programming by selecting different code paths based on the type of a variable.

**Pros:**

- Type safety is maintained.
- No runtime overhead.
- Cleaner syntax compared to macros.

**Cons:**

- Limited to selecting existing functions or operations.
- Not as flexible as true generic programming found in other languages.

### Common Generic Containers in C

#### 1. Generic Linked Lists

A linked list where each node contains a `void*` pointer to data, allowing storage of any data type.

**Structure Example:**

```c
typedef struct Node {
    void* data;
    struct Node* next;
} Node;

typedef struct LinkedList {
    Node* head;
    Node* tail;
    size_t size;
} LinkedList;
```

#### 2. Dynamic Arrays (Vectors)

Dynamic arrays that can resize and hold elements of any type via `void*` pointers.

**Structure Example:**

```c
typedef struct Vector {
    void** items;
    size_t capacity;
    size_t size;
} Vector;
```

#### 3. Stacks and Queues

Stacks and queues implemented using `void*` pointers to allow generic data storage.

**Structure Example (Stack):**

```c
typedef struct Stack {
    void** items;
    size_t capacity;
    size_t top;
} Stack;
```

#### 4. Hash Tables

Hash tables that use `void*` for keys and/or values, enabling storage of various data types.

**Structure Example:**

```c
typedef struct HashTableEntry {
    void* key;
    void* value;
    struct HashTableEntry* next;
} HashTableEntry;

typedef struct HashTable {
    HashTableEntry** buckets;
    size_t num_buckets;
} HashTable;
```



### C Code Example

A simple implementation of a generic linked list using `void*`:

`practice.h`

```C
typedef struct Node
{
    void *data;
    struct Node *next;
} Node;

Node *create_node(void *data);
void append(Node **head, void *data);
void print_int_list(Node *head);

void free_list(Node *head);
```

`functions.c`

```C
Node *create_node(void *data)
{
    Node *new_node = malloc(sizeof(Node));
    if (new_node == NULL)
    {
        printf("Error: malloc failed\n");
        exit(1);
    }
    new_node->data = data;
    new_node->next = NULL;
    return new_node;
}
void append(Node **head, void *data)
{
    Node *new_node = create_node(data);
    if (*head == NULL)
    {
        *head = new_node;
        return;
    }

    Node *current = *head;
    while (current->next != NULL)
    {
        current = current->next;
    }
    current->next = new_node;
}
void print_int_list(Node *head)
{
    Node *current = head;
    while (current != NULL)
    {
        printf("%d -> ", *(int *)current->data);
        current = current->next;
    }
    printf("NULL\n");
}

void free_list(Node *head)
{
    Node *current = head;
    while (current != NULL)
    {
        Node *tmp = current;
        current = current->next;
        free(tmp);
    }
}
```

`practice.c`

```C
int main()
{
    Node *head = NULL;
    int a = 1, b = 2, c = 3;

    append(&head, &a);
    append(&head, &b);
    append(&head, &c);

    print_int_list(head);

    free_list(head);
    return 0;
}

```



```shell
chan@CMA:~/C_Programming/practice$ make valgrind
clang -std=c23 -Wall -Wextra -g -c practice.c -o ./obj/practice.o 
clang -std=c23 -Wall -Wextra -g -c functions.c -o ./obj/functions.o 
clang -std=c23 -Wall -Wextra -g -c functions_2.c -o ./obj/functions_2.o
ar -rcs ./libs/libfunctions.a ./obj/functions.o ./obj/functions_2.o
clang -std=c23 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto
valgrind --leak-check=full ./practice 
==12231== Memcheck, a memory error detector
==12231== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==12231== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==12231== Command: ./practice
==12231== 
1 -> 2 -> 3 -> NULL
==12231== 
==12231== HEAP SUMMARY:
==12231==     in use at exit: 0 bytes in 0 blocks
==12231==   total heap usage: 4 allocs, 4 frees, 1,072 bytes allocated
==12231== 
==12231== All heap blocks were freed -- no leaks are possible
==12231== 
==12231== For lists of detected and suppressed errors, rerun with: -s
==12231== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



### Advantages and Disadvantages

#### Advantages

1. **Reusability:** Write once, use with any data type.
2. **Maintainability:** Easier to maintain a single generic implementation rather than multiple type-specific versions.
3. **Flexibility:** Can handle various data types without modification.

#### Disadvantages

1. **Type Safety:** Using `void*` can lead to runtime errors if not managed carefully.
2. **Performance Overhead:** Indirection through pointers can incur performance penalties.
3. **Complexity:** Implementing and using generic containers can be more complex, especially with macros or code generation.
4. **Debugging Difficulty:** Errors related to type casting or pointer misuse can be harder to trace.