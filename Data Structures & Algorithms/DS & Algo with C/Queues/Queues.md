## Queues

- A **Queue** is a linear data structure that follows the **First In, First Out (FIFO)** principle. Imagine a line at a ticket counter: the first person to join the line is the first to be served.

### **Why Use Queues?**

- **Order Processing:** Useful in scenarios like printer queues, CPU task scheduling.
- **Breadth-First Search (BFS):** Essential in graph traversal algorithms.
- **Buffer Management:** Commonly used in handling streaming data.

### Queue Operations

1. **Enqueue:** Add an element to the end of the queue.
   1. Enqueuing a new element typicaly requires (a) allocating a new `struct` to hold it; (b) making the old tail `struct` point at the new `struct`; and (c) updating the tail pointer to also point to the new `struct`.

2. **Dequeue :** Remove an element from the front of the queue.
   1. Dequeuing an element involves updating the head pointer and freeing the removed `struct`, exactly like a stack pop.

3. **Front:** View the front element without removing it.
4. **IsEmpty:** Check if the queue is empty.

### Queue Implementation Using Linked List

Using a linked list to implement a queue allows for **dynamic memory allocation**, enabling the queue to grow or shrink as needed without a fixed size limit.

### C Code Example # 1

#### Program Code

`practice.h`

```C
#define QUEUE_EMPTY INT_MIN

typedef struct node{
    int value;
    struct node *next;
} node;

typedef struct{
    node *head;
    node *tail;
} queue;

void init_queue(queue *q);

bool enqueue(queue *q, int value);

int dequeue(queue *q);
```

`functions.c`

```C
void init_queue(queue *q){
    q->head = NULL;
    q->tail = NULL;
}

bool enqueue(queue *q, int value){
    // create a new node
    node *newnode = malloc(sizeof(node));
    if(newnode == NULL){
        return false;
    }
    newnode->value = value;
    newnode->next = NULL;
    
    // if there's a tail, link it to the new node
    if(q->tail != NULL){
        q->tail->next = newnode;
    }
    q->tail = newnode;
    
    // if there's no head, set the head to the newnode
    if(q->head == NULL){
        q->head = newnode;
    }
    return true;
}

int dequeue(queue *q){
    // check to see if the queue is empty
    if(q->head == NULL){
        return QUEUE_EMPTY
    }
    
    // save the head of the queue
    node *tmp = q->head;
    
    // save the result we are going to return
    int result = tmp->value;
    
    // take it off
    q->head = q->head->next;
    if(q->head == NULL){
        q->tail = NULL;
    }
    free(tmp);
    return result;
}
```



`practice.c`

```C
int main()
{
    queue q1, q2, q3;

    init_queue(&q1);
    init_queue(&q2);
    init_queue(&q3);

    enqueue(&q1, 56);
    enqueue(&q2, 78);
    enqueue(&q2, 23);
    enqueue(&q2, 988);
    enqueue(&q3, 13);

    int t;
    while ((t = dequeue(&q2)) != QUEUE_EMPTY)
    {
        printf("t = %d\n", t);
    }
    return 0;
}
```

##### **Visualization**

**Queue Operations**

Let's visualize the queue operations step by step.

**1. Initial State**

- **Queues:** `q1`, `q2`, `q3` are empty.

  ```css
  q1: head -> NULL, tail -> NULL
  
  q2: head -> NULL, tail -> NULL
  
  q3: head -> NULL, tail -> NULL
  ```

**2. Enqueue Operations**

- **Enqueue `56` to `q1`:**

  ```css
  q1: head -> [56] -> NULL, tail -> [56]
  ```

- **Enqueue `78` to `q2`:**

  ```css
  q2: head -> [78] -> NULL, tail -> [78]
  ```

- **Enqueue `23` to `q2`:**

  ```css
  q2: head -> [78] -> [23] -> NULL, tail -> [23]
  ```

- **Enqueue `988` to `q2`:**

  ```css
  q2: head -> [78] -> [23] -> [988] -> NULL, tail -> [988]
  ```

- **Enqueue `13` to `q3`:**

  ```css
  q3: head -> [13] -> NULL, tail -> [13]
  ```

**3. Dequeue Operations from `q2`**

- **Dequeue from `q2`:**

  - First Dequeue:
    - Removes `78`.
    - Updates `head` to `23`.

  ```css
  q2: head -> [23] -> [988] -> NULL, tail -> [988]
  ```

  - Second Dequeue:
    - Removes `23`.
    - Updates `head` to `988`.

  ```css
  q2: head -> [988] -> NULL, tail -> [988]
  ```

  - Third Dequeue:
    - Removes `988`.
    - Updates `head` and `tail` to `NULL`.

  ```css
  q2: head -> NULL, tail -> NULL
  ```

  **Final State**

- **Queues:**

  ```css
  q1: head -> [56] -> NULL, tail -> [56]
  
  q2: head -> NULL, tail -> NULL
  
  q3: head -> [13] -> NULL, tail -> [13]
  ```

- **Output:**

```css
t = 78

t = 23

t = 988
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
t = 78
t = 23
t = 988

```



### C Code Example #2

#### Program Code

`practice.h`

```C
typedef struct QueueNode{
    int data;
    struct QueueNode *next;
}QueueNode;

typedef struct{
    QueueNode *head;
    QueueNode *tail;
}Queue;

QueueNode *createNode(int data);
void enqueue(Queue *q, int new_data);
int dequeue(Queue *q);
int front(Queue *q);
int isEmpty(Queue *q);
void printQueue(Queue *q);
```

`functions.c`

```C
// create a new node with the given data and returns a pointer to it
QueueNode *createNode(int data){
    QueueNode *newNode = (QueueNode *)malloc(sizeof(QueueNode));
    if(!newNode){
        printf("Memory allocation error\n");
        exit(1);
    }
    
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Adds a new element to the end of the queue
void enqueue(Queue *q, int new_data){
    QueueNode *newNode = createNode(new_data);
    
    // If the queue is empty, sets both head and tail to the new node
    if(q->tail == NULL){
        q->head = q->tail = newNode;
        printf("Enqueued %d to queue\n", new_data);
        return;
    }
    
    // Otherwise, linkes the new node to the current tail and updates the tail pointer
    //Connects the new node to the end of the existing list, effectively adding it after the current tail.
    q->tail->next = newNode;
    
    // Marks newNode as the new rear (tail) of the queue, ensuring that subsequent additions will follow after it.
    q->tail = newNode; // updates the tail pointer
    printf("Enqueued %d to queue\n", new_data);
}

// Removes and returns the element from the front of the queue
int dequeue(Queue *q){
    
    // Checks if the queue is empty
    if(q->head == NULL){
        printf("Queue underflow\n");
        exit(1);
    }
    
    // save the head node and its data
    QueueNode *temp = q->head;
    int dequeued = temp->data;
    
    // update the head pointer the next node
    q->head = temp->next;
    
    // If the queue becomes empty, set the tail pointer to NULL
    if(q->head == NULL){
        q->tail = NULL;
    }
    
    free(temp);
    
    // Return the data of the removed node
    return dequeued;
}

// Returns the front element of the queue
int front(Queue *q){
    if(q->head == NULL){
        printf("Queue is empty\n");
        exit(1);
    }
    return q->head->data;
}

int isEmpty(Queue *q){
    return q->head == NULL;
}

void printQueue(Queue *q){
    QueueNode *current = q->head;
    printf("Queue: ");
    while(current != NULL){
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}
```

##### Visualization of some code blocks

- ```C
  q->tail->next = newNode;
  q->tail = newNode;
  ```

```css
[Head] --> [Node1] --> [Node2] --> NULL
                           ^
                        [Tail]

```

- **`q->head`** points to **Node1**.
- **`q->tail`** points to **Node2**.
- **Node2->next** is **NULL**.

New Node to Add:

```css
newNode: [Node3] --> NULL
```

After Executing `q->tail->next = newNode;`:

```css
[Front] --> [Node1] --> [Node2] --> [Node3] --> NULL

```

- **Node2->next** now points to **Node3**.
- **Node3->next** remains **NULL**.

After Executing `q->tail = newNode;`:

```css
[Front] --> [Node1] --> [Node2] --> [Node3] --> NULL
                                     ^
                                  [Tail]

```

- **`q->tail`** now points to **Node3**.

- **Node3** is the new rear of the queue.

##### Visualization with Enqueue Operations:

1. **Initial Empty Queue:**

```css
Queue Structure:
[Front] = NULL
[Tail] = NULL
```

2. **Enqueue 10:**

- Since the queue is empty, both `front` and `tail` point to the new node.

```less
Queue Structure:
[Front] --> [10] --> NULL
[Tail] --> [10] --> NULL
```

3. **Enqueue 20:**

- Link `tail->next` to the new node (20).
- Update `tail` to point to 20.

```less
Queue Structure:
[Front] --> [10] --> [20] --> NULL
[Tail] --> [20] --> NULL
```

4. **Enqueue 30:**

- Link `tail->next` to the new node (30).
- Update `tail` to point to 30.

```less
Queue Structure:
[Front] --> [10] --> [20] --> [30] --> NULL
[Tail] --> [30] --> NULL
```

`practice.c`

```C
int main(){
    Queue q;

    q.head = NULL;
    q.tail = NULL;

    enqueue(&q, 10);
    enqueue(&q, 20);
    enqueue(&q, 30);

    printQueue(&q);

    printf("Front element is %d\n", front(&q));

    printf("Dequeued element: %d\n", dequeue(&q));

    printQueue(&q);

    printf("Dequeued element: %d\n", dequeue(&q));

    printQueue(&q);

    printf("Is queue empty? %s\n", isEmpty(&q) ? "Yes" : "No");

    printf("Dequeued element: %d\n", dequeue(&q));

    printf("Is queue empty? %s\n", isEmpty(&q) ? "Yes" : "No");
    return 0;
}
```

##### Visualization

**Initial State**

- **Queue:** Empty (`head = NULL`, `tail = NULL`).

```css
head -> NULL
tail -> NULL
```

**Enqueue Operations**

- **Enqueue `10`:**

  ```css
  head -> [10] -> NULL
  tail -> [10]
  ```

- Enqueue `20`:

  ```css
  head -> [10] -> [20] -> NULL
  tail -> [20]
  ```

- Enqueue `30`:

  ```css
  head -> [10] -> [20] -> [30] -> NULL
  tail -> [30]
  ```

**Dequeue Operations**

- **Dequeue `10`:**

  ```css
  head -> [20] -> [30] -> NULL
  
  tail -> [30]
  ```

- **Dequeue `20`:**

  ```css
  head -> [30] -> NULL
  
  tail -> [30]
  ```

- **Dequeue `30`:**

  ```css
  head -> NULL
  
  tail -> NULL
  ```

### **Final State**

- **Queue:** Empty (`head = NULL`, `tail = NULL`).

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Enqueued 10 to queue
Enqueued 20 to queue
Enqueued 30 to queue
Queue: 10 -> 20 -> 30 -> NULL
Front element is 10
Dequeued element: 10
Queue: 20 -> 30 -> NULL
Dequeued element: 20
Queue: 30 -> NULL
Is queue empty? No
Dequeued element: 30
Is queue empty? Yes
```



### C Code Example #3

#### Program Code

`practice.h`

```C
// Standard Link list element
typedef struct elt{
    struct elt *next;
    int value;
}Elt:

typedef struct queue{
    struct elt *head; // dequeue this next
    struct elt *tail; // enqueue after this
}Queue:

Queue *queueCreate(void);
void enq(Queue *q, int value);
int queueEmpty(const Queue *q);
int deq(Queue *q);
void queuePrint(const Queue *q);
void queueDestroy(Queue *q);
```

`functions.c`

```c
// create a new empty queue
Queue *queueCreate(void){
    Queue *q = malloc(sizeof(Queue));
    assert(q);
    q->head = q->tail = 0;
    
    return q;
}

// add a new value to back of queue
void enq(Queue *q, int value){
    Elt *e = malloc(sizeof(Queue));
    assert(e);
    
    e->value = value;
    
    // Because I will be the tail, nobody is behind me
    e->next = 0;
    
    if(q->tail == 0){
        // If the queue was empty, I become the head
        q->head = e;
    }else{
        // otherwise, I get in line after the old tail
        q->tail->next = e;
    }
    
    // I become the new tail
    q->tail = e;
}

int queueEmpty(const Queue *q){
    return q->head == 0;
}

// Remove and return value from front of queue
int deq(Queue *q){
    int ret;
    Elt *e;
    assert(!queueEmpty(q));
    
    ret = q->head->value;
    
    // patch out first element
    e = q->head;
    q->head = e->next;
    
    free(e);
    
    return ret;
}

// print contents of queue on a single line, head first
void queuePrint(const Queue *q){
    Elt *e;
    for(e = q->head; e != 0; e = e->next){
        printf("%d ", e->value);
    }
    putchar('\n');
}

// free a queue and all of its elements
void queueDestroy(Queue *q){
    while(!queueEmpty(q)){
        deq(q);
    }
    free(q);
}
```

`practice.c`

```C
int main()
{
    Queue *q = queueCreate();

    for (int i = 0; i < 5; i++)
    {
        printf("enq %d\n", i);
        enq(q, i);
        queuePrint(q);
    }

    while (!queueEmpty(q))
    {
        printf("deq gets %d\n", deq(q));
        queuePrint(q);
    }

    queueDestroy(q);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
enq 0
0 
enq 1
0 1 
enq 2
0 1 2 
enq 3
0 1 2 3 
enq 4
0 1 2 3 4 
deq gets 0
1 2 3 4 
deq gets 1
2 3 4 
deq gets 2
3 4 
deq gets 3
4 
deq gets 4

```

### Real World Applications:

#### 1. Order Processing Systems

**Description:** Queues manage the order in which tasks are processed, ensuring that the first task added is the first to be handled.

**Real-World Analogy:** A line of customers waiting at a checkout counter.

**Use Case Example:**

- **Customer Service:** Handling customer inquiries in the order they arrive.

**Visualization:**

```css
Front of Queue        Rear of Queue
     |                     |
     v                     v
[Customer1] -> [Customer2] -> [Customer3] -> NULL
```

#### 2. Print Queue

**Description:** Queues manage print jobs sent to a printer, processing them in the order they were received.

**Real-World Analogy:** A queue of print documents waiting to be printed.

**Use Case Example:**

- **Office Printers:** Documents are printed in the order they were submitted to the printer.

**Visualization:**

```less
Front of Print Queue        Rear of Print Queue
          |                             |
          v                             v
    [Doc1] -> [Doc2] -> [Doc3] -> NULL
```

#### 3. Task Scheduling

**Description:** Operating systems use queues to manage and schedule tasks or processes, ensuring efficient CPU utilization.

**Real-World Analogy:** A to-do list where tasks are executed in the order they are added.

**Use Case Example:**

- **CPU Scheduling:** Managing processes to execute based on priority and arrival time.

**Visualization:**

```less
Task Queue
-------------
| Task1      |
-------------
| Task2      |
-------------
| Task3      |
-------------
| Task4      |
-------------
```

#### 4. Breadth-First Search (BFS) in Graphs

**Description:** Queues are integral to BFS algorithms, which explore nodes level by level in graph or tree structures.

**Real-World Analogy:** Exploring friends of friends in a social network one level at a time.

**Use Case Example:**

- **Networking:** Finding the shortest path in network routing protocols.

**Visualization:**

```less
Queue during BFS:
[Node1] -> [Node2] -> [Node3] -> [Node4] -> NULL
```

#### 5. Buffering Data Streams

**Description:** Queues are used to manage data streams, buffering incoming data before processing to handle varying data rates.

**Real-World Analogy:** A conveyor belt where items are queued before being processed.

**Use Case Example:**

- **Streaming Services:** Buffering video data to ensure smooth playback.

**Visualization:**

```less
Data Buffer Queue
---------------------
| Data Packet1      |
---------------------
| Data Packet2      |
---------------------
| Data Packet3      |
---------------------
| ...               |
---------------------
```

---