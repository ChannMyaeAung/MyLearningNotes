### 1. Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking. It uses a stack (often implemented via recursion).

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

// Function prototypes
AdjListNode* newAdjListNode(int dest);
Graph* createGraph(int vertices);
void addEdge(Graph* graph, int src, int dest, int directed);
void DFSUtil(Graph* graph, int vertex, int visited[]);
void DFS(Graph* graph, int startVertex);

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

// Recursive DFS utility
void DFSUtil(Graph* graph, int vertex, int visited[]) {
    visited[vertex] = 1;
    printf("%d ", vertex);

    AdjListNode* temp = graph->array[vertex].head;
    while(temp) {
        if(!visited[temp->dest])
            DFSUtil(graph, temp->dest, visited);
        temp = temp->next;
    }
}

// DFS traversal
void DFS(Graph* graph, int startVertex) {
    int* visited = malloc(graph->numVertices * sizeof(int));
    for(int i = 0; i < graph->numVertices; i++)
        visited[i] = 0;

    DFSUtil(graph, startVertex, visited);
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

    printf("Depth First Traversal starting from vertex 2:\n");
    DFS(graph, 2);

    
    return 0;
}
```

**Output:**

```shell
Depth First Traversal starting from vertex 2:
2 3 0 1 
```

