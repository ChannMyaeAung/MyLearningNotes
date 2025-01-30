# Implementing Reduce() of Javascript in C

- In JavaScript, the `reduce()` method processes an array by applying a function against an accumulator and each element to reduce the array to a single value. 
- To implement similar functionality in C, we can use **function pointers** to create a generic `reduce` function that applies a binary operation across elements of **an array or a linked list.**

## Using Arrays:

#### Program Code

```c
// define a func pointer type for binary operations
typedef int (*BinaryOp)(int, int);

// Generic reduce func for arrays
int reduce(int *array, size_t length, BinaryOp op, int initial)
{
    int result = initial;
    for (size_t i = 0; i < length; i++)
    {
        result = op(result, array[i]);
    }
    return result;
}

int sum(int a, int b)
{
    return a + b;
}
int main()
{
    int numbers[] = {1, 2, 3, 4, 5};
    size_t length = sizeof(numbers) / sizeof(numbers[0]);

    int initial = 0;

    int total = reduce(numbers, length, sum, initial);
    printf("Sum: %d\n", total);
    return 0;
}
```

#### Program Output

```sh
chan@CMA:~/C_Programming/test$ ./final
Sum: 15
```



## Using Linked Lists:

#### Program Code

`hello.h`

```c
typedef int (*BinaryOp)(int, int);

typedef struct Node
{
    int data;
    struct Node *next;
} Node;

Node *createNode(int data);

int sum(int a, int b);
void append(Node **head, int data);
int reduce_list(Node *head, BinaryOp op, int initial);
```

`hello.c`

```c
Node *createNode(int data)
{
    Node *newNode = malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

int sum(int a, int b)
{
    return a + b;
}
void append(Node **head, int data)
{
    Node *newNode = createNode(data);
    if (*head == NULL)
    {
        *head = newNode;
    }
    else
    {
        Node *current = *head;
        while (current->next != NULL)
        {
            current = current->next;
        }
        current->next = newNode;
    }
}
int reduce_list(Node *head, BinaryOp op, int initial)
{
    int result = initial;
    Node *current = head;
    while (current != NULL)
    {
        result = op(result, current->data);
        current = current->next;
    }
    return result;
}
```

`main.c`

```c
int main()
{
    Node *head = NULL;
    append(&head, 1);
    append(&head, 2);
    append(&head, 3);
    append(&head, 4);
    append(&head, 5);

    int initial = 0;
    int total = reduce_list(head, sum, initial);
    printf("Sum: %d\n", total); // Output: Sum: 15

    // Free allocated memory
    Node *current = head;
    Node *next;
    while (current != NULL)
    {
        next = current->next;
        free(current);
        current = next;
    }

    return 0;
}
```



#### Program Output

```sh
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==12298== Memcheck, a memory error detector
==12298== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==12298== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==12298== Command: ./final
==12298== 
Sum: 15
==12298== 
==12298== HEAP SUMMARY:
==12298==     in use at exit: 0 bytes in 0 blocks
==12298==   total heap usage: 6 allocs, 6 frees, 1,104 bytes allocated
==12298== 
==12298== All heap blocks were freed -- no leaks are possible
==12298== 
==12298== For lists of detected and suppressed errors, rerun with: -s
==12298== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



