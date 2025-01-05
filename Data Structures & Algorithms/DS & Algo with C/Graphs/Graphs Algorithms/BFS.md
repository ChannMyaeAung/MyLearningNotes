### Breadth-First Search (BFS)

BFS explores all neighbors at the current depth before moving to nodes at the next depth level. It uses a queue to keep track of the traversal.

- This is done one "Level" at a time rather than one "branch" at a time.

#### **Example Explanation**:

```css
A----B----C
|         |
|		  |
D----E    F
|    |    |
|	 |    |
G----H    I
```

- Let's say we are at Node A and we need to travel to Node I. But we don't know where Node I is.
- Instead of a stack, we will use queue.
- We will add unvisted vertex(node) to the queue.
- Currently, We are at A.

```css
Queue
[A]
```

- We haven't visited any of the neighbors of A which are B and D.
- So we will add these two to the queue.

```css
Queue
[A]
[B]
[D]
```

- Then add unvisited adjacent neighbors of both B and D which are C(neighbor of B), and E & G (neighbors of D).

```css
Queue
[A]
[B]
[D]
[C]
[E]
[G]
```

- For the next level, we have F (neighbor of C) and H (neighbor of E).

```css
Queue
[A]
[B]
[D]
[C]
[E]
[G]
[F]
[H]
```

- And Lastly, I.

```css
Queue
[A]
[B]
[D]
[C]
[E]
[G]
[F]
[H]
[I]
```



**Adjacency List Implementation Example:**

```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_VERTICES 100

typedef struct AdjListNode {
    int dest;
    struct AdjListNode* next;
} AdjListNode;

typedef struct AdjList {
    AdjListNode* head;
} AdjList;

typedef struct Graph {
    int numVertices;
    AdjList* array;
} Graph;

// Queue implementation for BFS
typedef struct Queue {
    int items[MAX_VERTICES];
    int front;
    int rear;
} Queue;

// Function prototypes
AdjListNode* newAdjListNode(int dest);
Graph* createGraph(int vertices);
void addEdge(Graph* graph, int src, int dest, int directed);
void enqueue(Queue* q, int value);
int dequeue(Queue* q);
int isEmpty(Queue* q);
void BFS(Graph* graph, int startVertex);

// Create a new adjacency list node
AdjListNode* newAdjListNode(int dest) {
    AdjListNode* newNode = malloc(sizeof(AdjListNode));
    assert(newNode);
    newNode->dest = dest;
    newNode->next = NULL;
    return newNode;
}

// Create a graph
Graph* createGraph(int vertices) {
    Graph* graph = malloc(sizeof(Graph));
    assert(graph);
    graph->numVertices = vertices;

    graph->array = malloc(vertices * sizeof(AdjList));
    assert(graph->array);
    for(int i = 0; i < vertices; i++)
        graph->array[i].head = NULL;

    return graph;
}

// Add edge to the graph
void addEdge(Graph* graph, int src, int dest, int directed) {
    // Add edge from src to dest
    AdjListNode* newNode = newAdjListNode(dest);
    newNode->next = graph->array[src].head;
    graph->array[src].head = newNode;

    // If undirected, also add edge from dest to src
    if(!directed) {
        newNode = newAdjListNode(src);
        newNode->next = graph->array[dest].head;
        graph->array[dest].head = newNode;
    }
}

// Initialize the queue
void initQueue(Queue* q) {
    q->front = -1;
    q->rear = -1;
}

// Check if the queue is empty
int isEmpty(Queue* q) {
    return q->rear == -1;
}

// Add an element to the queue
void enqueue(Queue* q, int value) {
    if(q->rear == MAX_VERTICES - 1) {
        printf("\nQueue is Full!!");
        return;
    }
    if(q->front == -1)
        q->front = 0;
    q->rear++;
    q->items[q->rear] = value;
}

// Remove an element from the queue
int dequeue(Queue* q) {
    if(isEmpty(q)) {
        printf("\nQueue is Empty!!");
        return -1;
    }
    int item = q->items[q->front];
    q->front++;
    if(q->front > q->rear)
        q->front = q->rear = -1;
    return item;
}

// BFS traversal
void BFS(Graph* graph, int startVertex) {
    int* visited = malloc(graph->numVertices * sizeof(int));
    assert(visited);
    for(int i = 0; i < graph->numVertices; i++){
        visited[i] = 0;
    }

    Queue q;
    initQueue(&q);

    visited[startVertex] = 1;
    enqueue(&q, startVertex);

    while(!isEmpty(&q)) {
        int currentVertex = dequeue(&q);
        printf("%d ", currentVertex);

        AdjListNode* temp = graph->array[currentVertex].head;
        while(temp) {
            int adjVertex = temp->dest;
            if(!visited[adjVertex]) {
                visited[adjVertex] = 1;
                enqueue(&q, adjVertex);
            }
            temp = temp->next;
        }
    }
    free(visited);
}

void freeGraph(Graph *graph)
{
    for (int v = 0; v < graph->numVertices; v++)
    {
        AdjListNode *temp = graph->array[v].head;
        while (temp)
        {
            AdjListNode *toFree = temp;
            temp = temp->next;
            free(toFree);
        }
    }
    free(graph->array);
    free(graph);
}

int main()
{
    Graph *graph = createGraph(4);
    addEdge(graph, 0, 1, 0);
    addEdge(graph, 0, 2, 0);
    addEdge(graph, 1, 2, 0);
    addEdge(graph, 2, 0, 0);
    addEdge(graph, 2, 3, 0);
    addEdge(graph, 3, 3, 0);

    printf("Breadth First Traversal starting from vertex 0:\n");
    BFS(graph, 0);
    printf("\n");

    // Create another directed graph for testing
    Graph *graph2 = createGraph(5);
    addEdge(graph2, 0, 1, 1);
    addEdge(graph2, 0, 4, 1);
    addEdge(graph2, 1, 2, 1);
    addEdge(graph2, 1, 3, 1);
    addEdge(graph2, 1, 4, 1);
    addEdge(graph2, 2, 3, 1);
    addEdge(graph2, 3, 4, 1);

    printf("Breadth First Traversal starting from vertex 0 in graph2:\n");
    BFS(graph2, 0);
    printf("\n");

    freeGraph(graph);
    freeGraph(graph2);

    return 0;
}
```

**Output:**

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==10433== Memcheck, a memory error detector
==10433== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==10433== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==10433== Command: ./final
==10433== 
Breadth First Traversal starting from vertex 0:
0 2 1 3 
Breadth First Traversal starting from vertex 0 in graph2:
0 4 1 3 2 
==10433== 
==10433== HEAP SUMMARY:
==10433==     in use at exit: 0 bytes in 0 blocks
==10433==   total heap usage: 26 allocs, 26 frees, 1,468 bytes allocated
==10433== 
==10433== All heap blocks were freed -- no leaks are possible
==10433== 
==10433== For lists of detected and suppressed errors, rerun with: -s
==10433== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



**Graph 1**: Undirected Graph

```css
    0
   / \
  1---2
       |
       3
       |
      (Self-loop)
     
      
```

**Edges:**

- 0 -- 1
- 0 -- 2
- 1 -- 2
- 2 -- 3
- 3 -- 3 (Self-loop)



**Graph 2**: Directed Graph

```css
-----------------
↑	|--------- ↘↓
0 → 1 → 2 → 3 → 4
    ↓      
    3   
```

**Edges:**

- 0 → 1
- 0 → 4
- 1 → 2
- 1 → 3
- 1 → 4
- 2 → 3
- 3 → 4
