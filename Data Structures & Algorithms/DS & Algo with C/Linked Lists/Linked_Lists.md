## Linked lists

- A **Linked List** is a linear data structure where each element is a separate object called a **node**. Unlike arrays, linked lists are **dynamic** and can easily grow or shrink in size by allocating or deallocating memory during runtime.
- Linked lists are about the simplest data structure beyond arrays.
- They aren't very efficient for many purposes, but have very good performance for certain specialized applications.
- The basic idea is that instead of storing `n` items in one big array, we store each item in its own `struct`, and each of these `structs` includes a pointer to the next `struct` in the list (with a null pointer to indicate that there are no more elements).
- If we follow the pointers we can eventually reach all of the elements.

### Linked Lists use cases

- Implement other data structures: stacks, queues, hash tables
- Dynamic memory allocation

### Types of Linked Lists

1. **Singly Linked List:** Each node points to the next node.
2. **Doubly Linked List:** Each node points to both the next and the previous node.
3. **Circular Linked List:** The last node points back to the first node, forming a circle.

### What linked lists are and are not good for

- Linked lists are good for any task that involves inserting and deleting elements next to an element we already have a pointer to.
- Such operations can usually be done in O(1) time.
- They generally beat arrays (even resizable arrays) if we need to insert or delete in the middle of a list, since an array has to copy any elements above the insertion point to make room.
- If insertion or deletion always happen at the end, an array may be better.
- Linked lists are not good for any operation that requries random access, since reaching an arbitrary element of a linked list takes as much as O(n) time.
- For such applications, arrays are better if we don't need to insert in the middle.
- If we do, we should use some sort of tree.

### Singly Linked List in C

Each node in a singly linked list contains **data** and a **pointer** to the next node.

```c
#include <stdio.h>
#include <stdlib.h>

// Definition of a node
struct Node {
    int data;           // Data part
    struct Node* next;  // Pointer to the next node
};
```

#### Basic Operations

**Insertion**

1. **At the Beginning:** Add a new node at the start of the list.
2. **At the End:** Add a new node at the end of the list.
3. **After a Given Node:** Add a new node after a specified node.

**Deletion**

1. **From the Beginning:** Remove the first node.
2. **From the End:** Remove the last node.
3. **By Value:** Remove a node with a specific value.

**Traversal**

- **Display:** Print all the elements in the list.

### C Code Example (Simple)

#### Program Code

```c
#include <stdio.h>
#include <stdlib.h> // For malloc and free functions

// Define the structure of a node
struct Node {
    int data;
    struct Node* next;
};

int main() {
    // Allocate memory for nodes in the linked list
    struct Node* head = NULL;
    struct Node* second = NULL;
    struct Node* third = NULL;

    // Allocate memory for three nodes
    head = (struct Node*)malloc(sizeof(struct Node));
    second = (struct Node*)malloc(sizeof(struct Node));
    third = (struct Node*)malloc(sizeof(struct Node));

    // Assign data and link nodes
    head->data = 1;       // First node data
    head->next = second;  // Link first node with second node

    second->data = 2;     // Second node data
    second->next = third; // Link second node with third node

    third->data = 3;      // Third node data
    third->next = NULL;   // End of the list

    // Print the linked list
    struct Node* temp = head;
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }

    // Free allocated memory
    free(head);
    free(second);
    free(third);

    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
1 2 3 
```

### C Code Example (advanced)

#### Program Code

`hello.h`

```c
struct Node
{
    int data;
    struct Node *next;
};

struct Node *createNode(int data);

void insertAtBeginning(struct Node **head, int new_data);

void insertAtEnd(struct Node **head_ref, int new_data);

void deleteNode(struct Node **head_ref, int key);

void printList(struct Node *node);
```

`hello.c`

```c
// Function to create a new node with given data
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*) malloc(sizeof(struct Node));
    if (!newNode) { // Check for malloc failure
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Insert a node at the beginning
void insertAtBeginning(struct Node** head_ref, int new_data) {
    struct Node* new_node = createNode(new_data);
    new_node->next = *head_ref; // Link the old list to the new node
    *head_ref = new_node;       // Move the head to point to the new node
}

// Insert a node at the end
void insertAtEnd(struct Node** head_ref, int new_data) {
    struct Node* new_node = createNode(new_data);
    if (*head_ref == NULL) {    // If the list is empty, make the new node as head
        *head_ref = new_node;
        return;
    }
    struct Node* last = *head_ref;
    while (last->next != NULL) { // Traverse to the last node
        last = last->next;
    }
    last->next = new_node; // Link the last node to the new node
}

// Delete a node by value
void deleteNode(struct Node** head_ref, int key) {
    struct Node* temp = *head_ref;
    struct Node* prev = NULL;

    // If head node itself holds the key
    if (temp != NULL && temp->data == key) {
        *head_ref = temp->next; // Changed head
        free(temp);             // Free old head
        return;
    }

    // Search for the key
    while (temp != NULL && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }

    // If key was not present
    if (temp == NULL) return;

    // Unlink the node and free memory
    prev->next = temp->next;
    free(temp);
}

// Traverse and print the linked list
void printList(struct Node* node) {
    while (node != NULL) {
        printf("%d -> ", node->data);
        node = node->next;
    }
    printf("NULL\n");
}
```

`main.c`

```c
int main() {
    struct Node* head = NULL;

    // Insert nodes into the list
    insertAtEnd(&head, 10);          // List: 10
    insertAtBeginning(&head, 20);    // List: 20 -> 10
    insertAtEnd(&head, 30);          // List: 20 -> 10 -> 30
    insertAtBeginning(&head, 40);    // List: 40 -> 20 -> 10 -> 30

    printf("Linked List after insertions: \n");
    printList(head); // Expected Output: 40 -> 20 -> 10 -> 30 -> NULL

    // Delete a node
    deleteNode(&head, 20);
    printf("Linked List after deleting 20: \n");
    printList(head); // Expected Output: 40 -> 10 -> 30 -> NULL

    return 0;
}
```

#### **Explanation:**

1. Creating Nodes:
   - `createNode` function allocates memory for a new node, assigns data, and initializes the `next` pointer to `NULL`.
2. Insertion:
   - `insertAtBeginning` adds a new node at the start by adjusting the head pointer.
   - `insertAtEnd` traverses to the end and links the new node.
3. Deletion:
   - `deleteNode` searches for a node with the specified key and removes it by updating pointers.
4. Traversal:
   - `printList` iterates through the list and prints each node's data.
5. Main Function:
   - Demonstrates inserting and deleting nodes, and displays the list after each operation.

#### **Step-by-Step Walkthrough**

##### **1. Program Start**

- The `main` function begins execution.
- The linked list is initialized as empty with `head = NULL`.

##### **2. Insertion Operations**

**a. Insert `10` at End**

- Function Call: `insertAtEnd(&head, 10);`
- Since `head` is `NULL`, the new node becomes the head.
- List: `10 -> Null`

**b. Insert `20` at Beginning**

- Function Call: `insertAtBeginning(&head, 20);`
- A new node with data `20` is created.
- Its `next` points to the current head (`10`).
- `head` is updated to point to the new node (`20`).
- List: `20 -> 10 -> Null`

**c. Insert `30` at End**

- Function Call: `insertAtEnd(&head, 30);`
- A new node with data `30` is created.
- The function traverses to the end of the list:
  - Starts at `head` (`20`), moves to `10`, then reaches the end.
- The last node's (`10`) `next` is updated to point to the new node (`30`).
- List: `20 -> 10 -> 30 -> Null`

**d. Insert `40` at Beginning**

- Function Call: `insertAtBeginning(&head, 40);`
- A new node with data `40` is created.
- Its `next` points to the current head (`20`).
- `head` is updated to point to the new node (`40`).
- List: `40 -> 20 -> 10 -> 30 -> Null`

##### **3. Print List after Insertions**

- Function Call: `printList(head);`
- Traverses the list from `head`(40):
  - Prints `40`, moves to `20`.
  - Prints `20`, moves to `10`.
  - Prints `10`, moves to `30`.
  - Prints `30`, reaches `NULL`.
- Output: `40 -> 20 -> 10 -> 30 -> Null`

##### **4. Deletion Operation**

**Delete Node with Data `20`**

- Function Call: `deleteNode(&head, 20);`
- Checks if `head` contains the key (20):
  - `head` (`40`) does not match `20`.
- Traverses the list to find the node with `data == 20`:
  - `prev` is `NULL`, `temp` is `head` (`40`).
  - Moves `prev` to `40`, `temp` to `20`.
- Node with `data == 20` is found (`temp`).
- Removes the node:
  - `prev->next` (`40->next`) is updated to `temp->next` (`10`).
- Frees the memory allocated for the node with `20`.
- List after deletion: `40 -> 10 -> 30 -> Null`

##### **5. Print List after Deletion**

- Function Call: `printList(head);`
- Traverses the list from `head` (40):
  - Prints `40`, moves to `10`.
  - Prints `10`, moves to `30`.
  - Prints `30`, reaches `NULL`.
- Output: `40 -> 10 -> 30 -> Null`

##### **6. Program End**

- The `main` function returns `0`, indicating successful execution.
- The program terminates.

------

##### **Visualization of Linked List Changes**

**After Each Operation**

1. Initial List:
   - Empty (`head = NULL`)
2. Insert `10` at End:
   - List: `10 -> Null`
3. Insert `20` at Beginning:
   - List: `20 -> 10 -> Null`
4. Insert `30` at End:
   - List: `20 -> 10 -> 30 -> Null`
5. Insert `40` at Beginning:
   - List: `40 -> 20 -> 10 -> 30 -> Null`
6. Delete Node with Data `20`:
   - List: `40 -> 10 -> 30 -> Null`

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Linked List after insertions: 
40 -> 20 -> 10 -> 30 -> Null
Linked List after deleting 20: 
40 -> 10 -> 30 -> Null
```

### C Code Example (advanced) - 2

#### Program Code

```c
struct Node
{
    int value;
    struct Node *next;
};

typedef struct Node node_t;

void printList(node_t *head){
    node_t *temp = head;
    
    while(temp != NULL){
        printf("%d - ", temp->value);
        temp = temp->next;
    }
    printf("\n");
}

node_t *create_new_node(int value){
    node_t *result = malloc(sizeof(node_t));
    result->value = value;
    result->next = NULL;
    return result;
}

// head-> = [0, NULL]
// node_to_insert = 1, 1->next = [0, NULL]
// now the head points to -> [1, next] -> [0, NULL]]
node_t *insert_at_head(node_t **head, node_t *node_to_insert){
    node_to_insert->next = *head;
    *head = node_to_insert;
    return node_to_insert;
}

// 13 = node_to_insert, 75 = newnode
// Let's say 13 is pointing to 12, 13->next = 12
// now 75->next = 13->next which is = 12
// then set 13->next to 75 (newnode)
node_t *insert_after_node(node_t *node_to_insert, node_t *newnode){
    // set the newnode next pointer to where the node_to_insert is pointing
    newnode->next = node_to_insert->next;
    
    // for successful insert, set the current node next pointer to point to the newnode
    node_to_insert->next = newnode; 
}

node_t *find_node(node_t *head, int value){
    node_t *tmp = head;
    while(tmp != NULL){
        //If a match is found, the function returns the pointer to that node.
        if(tmp->value == value){
            return tmp;
        }
        
        // If the current node doesn't match, it moves to the next node:
        tmp = tmp->next;
    }
    
    // Return NULL if Not Found
    return NULL;
}

int main(){
    node_t *head = NULL;
    node_t *tmp;
    
    for(int i = 0; i < 25; i++){
        tmp = create_new_node(i);
        insert_at_head(&head, tmp);
    }
    
    tmp = find_node(head, 13);
    printf("found node with %d\n", tmp->value);
    
    insert_after_node(tmp, create_new_node(75));
    
    printList(head);
    return 0;
}
```



- The double pointer `node_t **head` allows the `insert_at_head` function to modify the original `head`

   pointer from the calling function (`main`). This is necessary because we need to update the `head` of the linked list to point to the new node we've inserted.

  - **`node_t **head`**: A pointer to the pointer `head`. By passing the address of `head`, the function can directly modify the pointer in `main`.
  - **Modifying `*head`**: By dereferencing the double pointer, `*head`, we access and modify the original `head` pointer.

##### Visualization

**`insert_after_node`** function:

**Before Insertion:**

```css
... -> [14] -> [13] -> [12] -> ...
```

- Nodes Involved:
  - **`node_to_insert`**: Node with `value = 13`
  - **`newnode`**: Node with `value = 75`
- Steps:
  1. Set `newnode->next` to `node_to_insert->next`
     - `newnode->next` points to `[12]`.
  2. Update `node_to_insert->next` to `newnode`
     - `node_to_insert->next` points to `newnode`.

**After Insertion:**

```css
... -> [14] -> [13] -> [75] -> [12] -> ...
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
found node with 13
24 - 23 - 22 - 21 - 20 - 19 - 18 - 17 - 16 - 15 - 14 - 13 - 75 - 12 - 11 - 10 - 9 - 8 - 7 - 6 - 5 - 4 - 3 - 2 - 1 - 0 - 
```



```c
struct elt{
    struct elt *next; // pointer to next element in the list
    int contents; // contents of this element
};
```



- In comparison to arrays, inserting or removing elements in linkedList can be cheap: at the front of the list, inserting a new element just requires allocating another `struct` and hooking up a few pointers, while removing an element just requires moving the pointer to the first element to point to the second element instead, and then freeing the first element.

![Screenshot from 2024-12-01 21-53-26](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-01 21-53-26.png)

- To make this work, we need to change two pointers:
  - The head pointer and the `next` pointer in the element holding 0.
  - These operations aren't affected by the size of the rest of the list and so take O(1) time.
  - Removal is the reverse of installation.
  - We patch out the first element by shifting the head pointer to the second element, then deallocate it with `free`.
  - We do have to be careful to get any data we need out of it before calling `free()`.
  - This is also O(1) opeartion.
- The fact that we can add and remove elements at the start of linked lists for cheap makes them particularly useful for implementing a **stack**.