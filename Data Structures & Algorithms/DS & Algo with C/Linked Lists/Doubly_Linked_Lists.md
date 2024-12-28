### Doubly Linked List

A **Doubly Linked List** is a type of linked list where each node contains pointers to both the **next** and **previous** nodes in the sequence. This bidirectional nature allows traversal in both directions, making certain operations more efficient compared to a singly linked list.

**Key Features:**

- **Bidirectional Traversal:** Can move forward and backward through the list.
- **Dynamic Size:** Can easily grow or shrink by adding or removing nodes.
- **Efficient Insertion/Deletion:** Especially at both ends or any given node.

#### Structure Definition

Each node in a doubly linked list contains three parts:

1. **Data:** The value stored in the node.
2. **Next Pointer:** Points to the next node in the list.
3. **Previous Pointer:** Points to the previous node in the list.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a doubly linked list
struct Node {
    int data;
    struct Node* next;
    struct Node* prev;
};
```

#### Basic Operations

##### Insertion

1. **At the Beginning:**
   - Create a new node.
   - Set the new node's `next` to the current head.
   - Set the current head's `prev` to the new node.
   - Update the head to the new node.
2. **At the End:**
   - Create a new node.
   - Traverse to the last node.
   - Set the last node's `next` to the new node.
   - Set the new node's `prev` to the last node.
3. **After a Given Node:**
   - Create a new node.
   - Adjust the `next` and `prev` pointers of neighboring nodes to include the new node.

##### Deletion

1. **From the Beginning:**
   - Update the head to the next node.
   - Set the new head's `prev` to `NULL`.
   - Free the old head node.
2. **From the End:**
   - Traverse to the last node.
   - Update the second last node's `next` to `NULL`.
   - Free the last node.
3. **By Value:**
   - Search for the node containing the value.
   - Adjust the `next` and `prev` pointers of neighboring nodes to exclude the target node.
   - Free the target node.

##### Traversal

- **Forward Traversal:** Start from the head and move using the `next` pointers.
- **Backward Traversal:** Start from the tail and move using the `prev` pointers.

#### C Code Implementation # 1

##### Program Code

`practice.h`

```C
// Definition of a node in a doubly linked list
struct Node
{
    int data;
    struct Node *next;
    struct Node *prev;
};

typedef struct Node Node;

Node *createNode(int data);
void insertAtBeginning(Node **head, int new_data);

void insertAtEnd(Node **head, int new_data);

void deleteNode(Node **head, int key);

void traverseForward(Node *node);

void traverseBackward(Node *tail);

Node *findTail(Node *head);
```

`functions.c`

```C
Node *createNode(int data){
    Node *newNode = malloc(sizeof(Node));
    if(newNode == NULL){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void insertAtBeginning(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    // since we are adding the element at the beginning, we have to set our newNode next to poitn to the head element.
    newNode->next = *head;
    
    // If there's a head element, set the head prev pointer to our newNode effectively placing the head right after our newNode
    if(*head != NULL){
        (*head)->prev = newNode;
    }
    
    // the head will now point to newNode
    // i.e newNode will become the first element (head)
    *head = newNode;
    printf("Inserted %d at the beginning\n", new_data);
}

void insertAtEnd(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    // We will traverse the node starting from the head
    Node *last = *head;
    
    // If the list is empty, make the new node as head
    if(*head == NULL){
        *head = newNode;
        printf("Inserted %d as the first element.\n", new_data);
        return;
    }
    
    // while the next node is not equal to NULL which means we are not at the last node, 
    while(last->next != NULL){
        last = last->next; // continue traversing from the head till the last
    }
    
    // if we've found the last element, set the next pointer of that element to the newNode, effectively inserting our newNode at the end of the list
    // last <-> newNode
    last->next = newNode;
    newNode->prev = last; // point the prev node back to the last element in the list
    
    printf("Inserted %d at the end.\n", new_data);
}

void deleteNode(Node **head, int key){
    Node *temp = *head;
    
    // search for the node to be deleted
    while(temp != NULL && temp->data != key){
        temp = temp->next;
    }
    
    // if the node wasn't found
    if(temp == NULL){
        printf("Element %d not found.\n", key);
        return;
    }
    
    // if the node to be deleted is the head
    if(*head == temp){
        *head = temp->next;
    }
    
    // Change next only if the node to be deleted is not the last node
    if(temp->next != NULL){
        
        // [3] <-> [4] <-> [5]
        // [4] = temp, 
        // temp->next->prev = [5]'s prev
        // temp->prev = [3]
        // set [5]'s prev pointer to [3], removing [4]
        temp->next->prev = temp->prev;
    }
    
    // Change prev only if the node to be deleted is not the first node
    if(temp->prev != NULL){
        // [3] <-> [4] <-> [5]
        // [4] = temp, 
        // temp->prev->next = [3]'s next
        // temp->next = [5]
        // set [3]'s next to [5] removing [4]
        temp->prev->next = temp->next;
    }
    
    free(temp);
    printf("Deleted %d from the list.\n", key);
}

void traverseForward(Node *node){
    printf("Forward Traversal: ");
    while(node != NULL){
        printf("%d <-> ", node->data);
        node = node->next;
    }
    printf("NULL\n");
}

void traverseBackward(Node *tail){
    printf("Backward Traversal: ");
    while(tail != NULL){
        printf("%d <-> ", tail->data);
        tail = tail->prev;
    }
    printf("NULL\n");
}

Node *findTail(Node *head){
    Node *tail = head;
    
    // If the node is empty, return NULL
    if(head == NULL){
        return NULL;
    }
    
    // Keep traversing till the end of the list
    while(tail->next != NULL){
        tail = tail->next;
    }
    return tail;
}
```

`practice.c`

```C
int main(){
    Node *node = NULL;
    
    // Inserting elements
    insertAtBeginning(&node, 10); // List: 10
    insertAtEnd(&node, 20); // List: 10 <-> 20
    insertAtBeginning(&node, 5); // List: 5 <-> 10 <-> 20
    
    insertAtEnd(&node, 25); // List: 5 <-> 10 <-> 20 <-> 25

    // Traversing the list
    traverseForward(node);
    Node *tail = findTail(node);
    traverseBackward(tail);
    
    deleteNode(&node, 10); // List: 5 <-> 20 <-> 25
    
    // Traversing the list again after deleting
    traverseForward(node);
    tail = findTail(node);
    traverseBackward(tail);
    
    // Attempting to delete a non-existent element
    deleteNode(&head, 100);
    return 0;
}
```



##### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 at the beginning
Inserted 20 at the end.
Inserted 5 at the beginning
Inserted 25 at the end.
Forward Traversal: 5 <-> 10 <-> 20 <-> 25 <-> NULL
Backward Traversal: 25 <-> 20 <-> 10 <-> 5 <-> NULL
Deleted 10 from the list.
Forward Traversal: 5 <-> 20 <-> 25 <-> NULL
Backward Traversal: 25 <-> 20 <-> 5 <-> NULL
Element 100 not found.

```

#### Summary

##### Doubly Linked Lists

- **Bidirectional Traversal:** Can move both forward and backward through the list.
- **Nodes:** Each node contains data and pointers to both the next and previous nodes.
- **Dynamic Size:** Easily grows or shrinks by adding or removing nodes.
- **Operations:** Efficient insertion and deletion at both ends or any given node.
- **Use Cases:** Implementing other data structures (e.g., stacks, queues, deques), navigation systems, etc.