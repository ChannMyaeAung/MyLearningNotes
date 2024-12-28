### Circular Linked List

A **Circular Linked List** is a variation of the standard linked list where the last node points back to the first node, forming a **circle**. This structure allows traversal of the list starting from any node, looping infinitely through the list. Circular linked lists can be **singly** or **doubly** linked.

**Key Features:**

- **No NULL Pointers:** The last node links back to the first node.
- **Continuous Traversal:** Can cycle through the list endlessly.
- **Efficient for Round-Robin Scheduling:** Ideal for applications requiring cyclic iterations.

#### Structure Definition

A **Singly Circular Linked List** has nodes where each node points to the next node, and the last node points back to the first node. Similarly, a **Doubly Circular Linked List** includes both `next` and `prev` pointers, with the last node's `next` pointing to the first node and the first node's `prev` pointing to the last node.

For simplicity, we'll focus on **Singly Circular Linked Lists**.

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a node in a singly circular linked list
struct Node {
    int data;
    struct Node* next;
};

```

#### Basic Operations

##### Insertion

1. **At the Beginning:**
   - Create a new node.
   - If the list is empty, point the new node to itself.
   - Otherwise, traverse to the last node, set its `next` to the new node.
   - Set the new node's `next` to the head.
   - Update the head to the new node.
2. **At the End:**
   - Create a new node.
   - If the list is empty, point the new node to itself and set it as head.
   - Otherwise, traverse to the last node.
   - Set the last node's `next` to the new node.
   - Set the new node's `next` to the head.
3. **After a Given Node:**
   - Create a new node.
   - Find the specified node.
   - Set the new node's `next` to the specified node's `next`.
   - Set the specified node's `next` to the new node.

##### Deletion

1. **From the Beginning:**
   - If the list is empty, indicate underflow.
   - If the list has only one node, set head to `NULL`.
   - Otherwise, traverse to the last node.
   - Set the last node's `next` to the second node.
   - Free the original head.
   - Update the head to the second node.
2. **From the End:**
   - If the list is empty, indicate underflow.
   - If the list has only one node, set head to `NULL`.
   - Otherwise, traverse to the second last node.
   - Set the second last node's `next` to the head.
   - Free the last node.
3. **By Value:**
   - If the list is empty, indicate underflow.
   - Traverse the list to find the node with the specified value.
   - If found, adjust the `next` pointers to exclude the node.
   - Free the node.

##### Traversal

- **Display:** Start from the head and move through the list until you reach back to the head.

#### C Code Example # 1

##### Program Code

`practice.h`

```C
struct Node
{
    int data;
    struct Node *next;
};
typedef struct Node Node;

Node *createNode(int data);
void insertAtBeginning(Node **head, int new_data);

void insertAtEnd(Node **head, int new_data);

void deleteNode(Node **head, int key);

void traverse(Node *head);
```

`functions.c`

```C
Node *createNode(int data){
    Node *newNode = malloc(sizeof(Node));
    if(!newNode){
        printf("Memory allocation failed\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

void insertAtBeginning(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    if(*head == NULL){
        newNode->next = newNode; // Point to itself
        *head = newNode;
        printf("Inserted %d as the first node.\n", new_data);
        return;
    }
    
    Node *temp = *head;
    
    // traverse to the last node
    while(temp->next != *head){
        temp = temp->next;
    }
    
    temp->next = newNode; // Link last node to new node
    newNode->next = *head; // Link new node to head
    *head = newNode; // Update head to new node
    
    printf("Inserted %d at the beginning.\n", new_data);
}

void insertAtEnd(Node **head, int new_data){
    Node *newNode = createNode(new_data);
    
    if(*head == NULL){ 
        newNode->next = newNode; // Point to itself
        *head = newNode;
        printf("Inserted %d as the first node.\n", new_data);
        return;
    }
    
    Node *temp = *head;
    
    while(temp->next != *head){
        temp = temp->next;
    }
    
    temp->next = newNode; // Link last node to new node
    newNode->next = *head; // Link new node to head
    
    printf("Inserted %d at the end.\n", new_data);
}

void deleteNode(Node **head, int key){
    if(*head == NULL){
        printf("List is empty.\n");
        return;
    }
    
    Node *current = *head;
    Node *prev = NULL;
    
    // If head node is to be deleted
    if(current->data == key){
        // If there's only one node
        if(current->next == *head){
            *head = NULL;
        }else{
            // Find the last node
            Node *last = *head;
            while(last->next != *head){
                last = last->next;
            }
            
            *head = current->next;
            last->next = *head;
        }
        free(current);
        printf("Deleted %d from the list.\n", key);
        return;
    }
    
    // Search for the node to be deleted
    do{
        prev = current;
        current = current->next;
        if(current->data == key){
            prev->next = current->next;
            free(current);
            printf("Deleted %d from the list\n", key);
            return;
        }
    }while(current != *head);
    printf("%d not found in the list.\n", key);
}

void traverse(Node *head){
    if(head == NULL){
        printf("List is empty.\n");
        return;
    }
    
    Node *temp = head;
    printf("Circular Linked List: ");
    do{
        printf("%d -> ", temp->data);
        temp = temp->next;
    }while(temp != head);
    printf("HEAD\n");
}
```

`practice.c`

```C
int main()
{
    Node *head = NULL;

    // insert elements
    insertAtBeginning(&head, 10); // List: 10
    traverse(head);

    insertAtEnd(&head, 20); // List: 10 -> 20
    traverse(head);

    insertAtBeginning(&head, 5); // List: 5 -> 10 -> 20
    traverse(head);

    insertAtEnd(&head, 25); // List: 5 -> 10 -> 20 -> 25
    traverse(head);

    // Delete elements
    deleteNode(&head, 10); // List: 5 -> 20 -> 25
    traverse(head);

    deleteNode(&head, 5); // List: 20 -> 25
    traverse(head);

    deleteNode(&head, 30); // Attempt to delete non-existent element
    return 0;
}
```

##### Visualization

Let's visualize how the **Singly Circular Linked List** operates step-by-step.

Initial Empty List:

```less
[Head] = NULL
```

After Inserting 10 at the Beginning:

```less
[Head] --> [10] --> [10] (points back to itself)
```

- **Head:** Points to the new node (10).
- **Node10->next:** Points back to itself.

After Inserting 20 at the End:

```less
[Head] --> [10] --> [20] --> [10] (points back to head)
```

- **Head:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points back to 10.

After Inserting 5 at the Beginning:

```css
[Head] --> [5] --> [10] --> [20] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points back to 5.

After Inserting 25 at the End:

```css
[Head] --> [5] --> [10] --> [20] --> [25] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 10.
- **Node10->next:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 5.

After Deleting 10:

```css
[Head] --> [5] --> [20] --> [25] --> [5] (points back to head)
```

- **Head:** Points to 5.
- **Node5->next:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 5.

After Deleting 5:

```css
[Head] --> [20] --> [25] --> [20] (points back to head)
```

- **Head:** Points to 20.
- **Node20->next:** Points to 25.
- **Node25->next:** Points back to 20.

Attempting to Delete 30 (Non-existent):

```css
List remains unchanged:
[Head] --> [20] --> [25] --> [20] (points back to head)
```

**Diagram Representation:**

```css
Step-by-Step Operations:

1. Initial State:
[Head] = NULL

2. Insert 10 at Beginning:
[Head] --> [10] --> [10] (self-loop)

3. Insert 20 at End:
[Head] --> [10] --> [20] --> [10]

4. Insert 5 at Beginning:
[Head] --> [5] --> [10] --> [20] --> [5]

5. Insert 25 at End:
[Head] --> [5] --> [10] --> [20] --> [25] --> [5]

6. Delete 10:
[Head] --> [5] --> [20] --> [25] --> [5]

7. Delete 5:
[Head] --> [20] --> [25] --> [20]

8. Attempt to Delete 30:
No change in the list.
```

##### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Inserted 10 as the first node.
Circular Linked List: 10 -> HEAD
Inserted 20 at the end.
Circular Linked List: 10 -> 20 -> HEAD
Inserted 5 at the beginning.
Circular Linked List: 5 -> 10 -> 20 -> HEAD
Inserted 25 at the end.
Circular Linked List: 5 -> 10 -> 20 -> 25 -> HEAD
Deleted 10 from the list.
Circular Linked List: 5 -> 20 -> 25 -> HEAD
Deleted 5 from the list.
Circular Linked List: 20 -> 25 -> HEAD
30 not found in the list.
```

#### Use Cases

**Circular Linked Lists** are useful in scenarios where the data needs to be processed in a cyclic manner or where continuous traversal is required without restarting from the head. Some common real-world applications include:

1. **Round-Robin Scheduling:**
   - **Operating Systems:** Managing process scheduling in a fair and cyclic order.
   - **Real-Time Systems:** Ensuring tasks are executed in a timely, repetitive manner.
2. **Music or Video Players:**
   - **Playlists:** Continuously playing songs or videos in a loop.
3. **Buffer Management:**
   - **Circular Buffers:** Implementing buffers that wrap around, similar to Ring Buffers, for managing streaming data.
4. **Game Development:**
   - **Turn-Based Games:** Managing player turns in a cyclic order.
5. **Data Structures:**
   - **Josephus Problem:** Solving problems that involve circular elimination processes.
6. **Networking:**
   - **Token Ring Networks:** Managing access to the network using a token passed in a circular manner.

**Example: Round-Robin Scheduler**

In a Round-Robin Scheduler, each process is assigned a fixed time slot in a cyclic order. A circular linked list efficiently manages this by allowing the scheduler to cycle through processes endlessly without restarting the traversal.

```css
Processes: P1, P2, P3, P4

Circular Linked List:
[P1] -> [P2] -> [P3] -> [P4] -> [P1] ...
```

The scheduler moves to the next process by following the `next` pointer, and once it reaches the end, it loops back to the beginning seamlessly.