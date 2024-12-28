## Deques

- Suppose we want a data structure that represents a line of elements where we can push or pop elements at either end.
- Such a data structure is known as a `deque` (pronounced like "deck") and can be implemented with all operations taking O(1) time by a **doubly-linked list**, where each element has a pointer to both its successor and its predecessor.
- A **Deque** (pronounced "deck") stands for **Double-Ended Queue**. 
- It's a data structure that allows insertion and deletion of elements from both the **front** and **rear** ends. This flexibility makes deques more versatile than standard queues or stacks.

**Key Features:**

- **Double-Ended:** Operations can be performed at both ends.
- **Dynamic Size:** Can grow or shrink as needed.
- **Efficient Operations:** Insertion and deletion at both ends occur in constant time.

### Structure Definition

A deque can be efficiently implemented using a **Doubly Linked List** because of its bidirectional pointers.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a deque (using doubly linked list)
struct DequeNode {
    int data;
    struct DequeNode* next;
    struct DequeNode* prev;
};

```

### Basic Operations

#### Insert Front

- Create a new node.
- Set the new node's `next` to the current front.
- Set the current front's `prev` to the new node.
- Update the front pointer to the new node.
- If the deque was empty, also set the rear pointer to the new node.

#### Insert Rear

- Create a new node.
- Set the current rear's `next` to the new node.
- Set the new node's `prev` to the current rear.
- Update the rear pointer to the new node.
- If the deque was empty, also set the front pointer to the new node.

#### Delete Front

- If the deque is empty, indicate underflow.
- Remove the front node.
- Update the front pointer to the next node.
- If the deque becomes empty, set the rear pointer to `NULL`.
- Free the removed node.

#### Delete Rear

- If the deque is empty, indicate underflow.
- Remove the rear node.
- Update the rear pointer to the previous node.
- If the deque becomes empty, set the front pointer to `NULL`.
- Free the removed node.

### C Code Example

#### Program Code

`practice.h`

```C
struct DequeNode
{
    int data;
    struct DequeNode *next;
    struct DequeNode *prev;
};

typedef struct DequeNode DequeNode;

DequeNode *createNode(int data);
void insertFront(DequeNode **head, DequeNode **tail, int data);
void insertRear(DequeNode **head, DequeNode **tail, int data);

int deleteFront(DequeNode **head, DequeNode **tail);

int deleteRear(DequeNode **head, DequeNode **tail);

void traverseDeque(DequeNode *head);
```

`functions.c`

```C
DequeNode *createNode(int data){
    DequeNode *newNode = malloc(sizeof(DequeNode));
    if(!newNode){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void insertFront(DequeNode **head, DequeNode **tail, int data){
    DequeNode *newNode = createNode(data);
    newNode->next = *head;
    
    // if there's a current head node, set that node prev pointer to newNode placing the newNode before that.
    if(*head != NULL){
        *head->prev = newNode;
    }
    
    // the newNode now becomes the head element.
    *head = newNode;
    
    // If deque was empty
    if(*tail == NULL){
        *tail = newNode;
    }
    
    printf("Inserted %d at the front\n", data);
}

void insertRear(DequeNode **head, DequeNode **tail, int data){
    DequeNode *newNode = createNode(data);
    newNode->prev = *tail;
    
    // if there's a tail node, set that node's next pointer to point to the newNode, placing the newNode after that.
    if(*tail != NULL){
        (*tail)->next = newNode;
    }
    
    // The newNode becomes the tail element
    *tail = newNode;
    
    // if deque was empty
    if(*head == NULL){
        *head = newNode;
    }
    
    printf("Inserted %d at the rear\n", data);
}

int deleteFront(DequeNode **head, DequeNode **tail){
    if(*head == NULL){
        printf("Deque is empty\n");
        exit(1);
    }
    
    // store the current head node in a temp node
    dequeNode *temp = *head;
    
    // store its data in data variable
    int data = temp->data;
    
    // set the head pointer to point to the next node
    *head = temp->next;
    
    if(*head != NULL){
        (*head)->prev = NULL;
    }else{ // if deque becomes empty
        *tail = NULL;
    }
    
    free(temp);
    printf("Deleted %d from the front\n", data);
    return data.
}

int deleteRear(DequeNode **head, DequeNode **tail){
    if(*tail == NULL){
        printf("Deque is empty\n");
        exit(1);
    }
    
    DequeNode *temp = *tail;
    int data = temp->data;
    *tail = temp->prev;
    
    if(*tail != NULL){
        (*tail)->next = NULL;
    }else{
        *head = NULL;
    }
    
    free(temp);
    printf("Deleted %d from the rear.\n", data);
    return data;
}

void traverseDeque(DequeNode *head){
    printf("Deque (Front to Rear): ");
    while(head != NULL){
        printf("%d ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}
```

`practice.c`

```C
int main()
{
    DequeNode *head = NULL;
    DequeNode *tail = NULL;

    // Insert elements
    insertFront(&head, &tail, 10); // Deque: 10
    traverseDeque(head);

    insertRear(&head, &tail, 20); // Deque: 10 <-> 20
    traverseDeque(head);

    insertFront(&head, &tail, 5); // Deque: 5 <-> 10 <-> 20
    traverseDeque(head);

    insertRear(&head, &tail, 25); // Deque: 5 <-> 10 <-> 20 <-> 25
    traverseDeque(head);

    // Delete elements
    deleteFront(&head, &tail); // Deque: 10 <-> 20 <-> 25
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque: 10 <-> 20
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque: 10
    traverseDeque(head);

    deleteRear(&head, &tail); // Deque is empty
    traverseDeque(head);

    free(head);
    head = NULL;
    tail = NULL;

    printf("Deque is empty\n");

    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 at the front
Deque (Front to Rear): 10 NULL
Inserted 20 at the rear
Deque (Front to Rear): 10 20 NULL
Inserted 5 at the front
Deque (Front to Rear): 5 10 20 NULL
Inserted 25 at the rear
Deque (Front to Rear): 5 10 20 25 NULL
Deleted 5 from the front
Deque (Front to Rear): 10 20 25 NULL
Deleted 25 from the rear.
Deque (Front to Rear): 10 20 NULL
Deleted 20 from the rear.
Deque (Front to Rear): 10 NULL
Deleted 10 from the rear.
Deque (Front to Rear): NULL
Deque is empty

```



### Summary

#### Deques (Double-Ended Queues)

- **Double-Ended:** Supports insertion and deletion from both the front and rear ends.
- **LIFO and FIFO Combined:** Can function as both a stack and a queue.
- **Dynamic Size:** Grows and shrinks as needed without fixed size limitations.
- **Implementation:** Often implemented using a doubly linked list for efficient operations.
- **Use Cases:** Sliding window algorithms, task scheduling, browser history management, etc.