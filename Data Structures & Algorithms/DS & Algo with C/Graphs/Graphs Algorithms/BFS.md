### Breadth-First Search (BFS)

BFS explores all neighbors at the current depth before moving to nodes at the next depth level. It uses a queue to keep track of the traversal.

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
    newNode->dest = dest;
    newNode->next = NULL;
    return newNode;
}

// Create a graph
Graph* createGraph(int vertices) {
    Graph* graph = malloc(sizeof(Graph));
    graph->numVertices = vertices;

    graph->array = malloc(vertices * sizeof(AdjList));
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

    // If undirected, add edge from dest to src
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
    for(int i = 0; i < graph->numVertices; i++)
        visited[i] = 0;

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

int main() {
    Graph* graph = createGraph(4);
    addEdge(graph, 0, 1, 0);
    addEdge(graph, 0, 2, 0);
    addEdge(graph, 1, 2, 0);
    addEdge(graph, 2, 0, 0);
    addEdge(graph, 2, 3, 0);
    addEdge(graph, 3, 3, 0);

    printf("Breadth First Traversal starting from vertex 2:\n");
    BFS(graph, 2);

    return 0;
}
```

**Output:**

```shell
Breadth First Traversal starting from vertex 2:
2 3 0 1 
```

