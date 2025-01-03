### Depth-First Search (DFS)

- A search algorithm for traversing a tree or graph data structure.

DFS explores as far as possible along each branch before backtracking. It uses a stack (often implemented via recursion).

1. Pick a route.
2. Keep going until we reach a dead end, or a previously visited node.
3. Backtrack to last node that has unvisited adjacent neighbors.

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
- Whenever we visited a node, we will push it onto the stack.
- Let's say we will visit from A to D. Then we will push D to the stack.

```css
[D]
[A]
```

- Then we either go to E or G, let's go to G. We will push it onto the stack.

```css
[G]
[D]
[A]
```

- Then from G to H and to E.

```css
[E]
[H]
[G]
[D]
[A]
```

- Then we circle around back to D. But D is already marked as **visited**. So we are going to backtrack to a node that has **unvisited adjacent neighbors** which is A in this case.
- Then we go to B to C and so on to I eventually.
- The stack will be like this:

```css
[I]
[F]
[C]
[B]
[A]
```



#### **Adjacency List Implementation Example Using DFS:**

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

    // If undirected, also add edge from dest to src
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
    printf("Created graph with %d vertices.\n", graph->numVertices);
    addEdge(graph, 0, 1, 0);
    addEdge(graph, 0, 2, 0);
    addEdge(graph, 1, 2, 0);
    addEdge(graph, 2, 0, 0);
    addEdge(graph, 2, 3, 0);
    addEdge(graph, 3, 3, 0);

    printf("Depth First Traversal starting from vertex 2:\n");
    DFS(graph, 2);
    printf("\n");

    // Create a new graph2
    Graph *graph2 = createGraph(3);
    printf("Created graph2 with %d vertices.\n", graph2->numVertices);
    // Test addEdge on graph2
    addEdge(graph2, 0, 1, 1);
    addEdge(graph2, 1, 2, 1);
    printf("Added edges to graph2.\n");

    // Perform DFS on graph2
    printf("Depth First Traversal of graph2 starting from vertex 0:\n");
    DFS(graph2, 0);
    printf("\n");

    // Free graph2
    freeGraph(graph2);

    freeGraph(graph);
    return 0;
}
```

**Output:**

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==17026== Memcheck, a memory error detector
==17026== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==17026== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==17026== Command: ./final
==17026== 
Created graph with 4 vertices.
Depth First Traversal starting from vertex 2:
2 3 0 1 
Created graph2 with 3 vertices.
Added edges to graph2.
Depth First Traversal of graph2 starting from vertex 0:
0 1 2 
==17026== 
==17026== HEAP SUMMARY:
==17026==     in use at exit: 0 bytes in 0 blocks
==17026==   total heap usage: 21 allocs, 21 frees, 1,364 bytes allocated
==17026== 
==17026== All heap blocks were freed -- no leaks are possible
==17026== 
==17026== For lists of detected and suppressed errors, rerun with: -s
==17026== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**Graph**

```css
**Graph 1 (Undirected)**

Vertices: 0, 1, 2, 3

**Edges:**
- 0 -- 1
- 0 -- 2
- 1 -- 2
- 2 -- 3
- 3 -- 3 (Self-loop)

    0
   / \
  1---2
       |
       3
      / 
  (Self-loop)



**Graph 2 (Directed)**

Vertices: 0, 1, 2

Edges:
- 0 → 1
- 1 → 2

0 → 1 → 2
```

