### Dijkstra’s Shortest Path Algorithm

Dijkstra's algorithm finds the shortest path from a single source vertex to all other vertices in a weighted graph with non-negative weights.

**Note:** Implementing Dijkstra's algorithm in C requires handling priority queues (often implemented with min-heaps) for efficiency. 

**Efficiency:** Implementing a min heap reduces the time complexity from O(V²) to O((V + E) log V), which is significant for large graphs.

**Disconnected Graphs:** If the graph is disconnected, some vertices will remain at `INT_MAX`, indicating that they are unreachable from the source.

**Negative Weights:** Dijkstra's Algorithm does not handle negative edge weights. For graphs with negative weights, consider using the **Bellman-Ford** algorithm.

**Applications:** GPS navigation, network routing protocols, and more.

1. **Initialize:**
   - Set the distance to the source vertex as 0 and all others as infinity.
   - Create a priority queue and insert all vertices with their current distance.
2. **Process:**
   - While the queue is not empty:
     - Extract the vertex with the minimum distance.
     - For each neighbor of this vertex, calculate the tentative distance through the current vertex.
     - If this distance is less than the known distance, update it and adjust its position in the priority queue.
3. **Result:**
   - After processing, the distance array contains the shortest distances from the source to all vertices.

Implementing Dijkstra's algorithm efficiently in C involves creating a priority queue with operations like `insert` and `extract-min`, which can be done using a binary heap.

### Implementation Overview

We'll implement Dijkstra's Algorithm using the following components:

1. **Graph Representation:** Adjacency List.
2. **Priority Queue:** Min Heap to efficiently retrieve the vertex with the smallest tentative distance.
3. **Algorithm Steps:**
   - Initialize distances to all vertices as infinite, except the source vertex.
   - Insert all vertices into the Min Heap based on their current tentative distance.
   - Extract the vertex with the smallest distance, and update the distances of its adjacent vertices.
   - Repeat until the Min Heap is empty.

### C Implementation (Min-Heap Optimized)

#### Program Code

`practice.h`

```C
#include <openssl/ssl.h>
#include <openssl/err.h>
#include <stdbool.h>
#include <complex.h>
#include <tgmath.h>
#include <stdint.h>
#include <assert.h>
#ifndef PRACTICE_H
#define PRACTICE_H

typedef struct AdjListNode
{
    int dest;
    int weight;
    struct AdjListNode *next;
} AdjListNode;

typedef struct AdjList
{
    AdjListNode *head;
} AdjList;

typedef struct Graph
{
    int V;          // Num of vertices
    AdjList *array; // Array of adjacency lists
} Graph;

AdjListNode *newAdjListNode(int dest, int weight);
Graph *createGraph(int V);
void addEdge(Graph *graph, int src, int dest, int weight);

typedef struct MinHeapNode
{
    int v;
    int dist;
} MinHeapNode;

typedef struct MinHeap
{
    int size;     // Current number of elements in heap
    int capacity; // Capacity of the heap
    int *pos;     // Position of vertex in heap
    MinHeapNode **array;
} MinHeap;

MinHeapNode *newMinHeapNode(int v, int dist);
MinHeap *createMinHeap(int capacity);
void swapMinHeapNode(MinHeapNode **a, MinHeapNode **b);
void minHeapify(MinHeap *minHeap, int idx);
int isEmpty(MinHeap *minHeap);
MinHeapNode *extractMin(MinHeap *minHeap);

void decreaseKey(MinHeap *minHeap, int v, int dist);
int isInMinHeap(MinHeap *minHeap, int v);

void dijkstra(Graph *graph, int src);

void freeGraph(Graph *graph, int V);

#endif
```

`functions.c`

```C
#define _XOPEN_SOURCE 700
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <stdarg.h>
#include <pthread.h>
#include <strings.h>
#include <signal.h>
#include <errno.h>
#include <openssl/ssl.h>
#include <openssl/err.h>
#include <stdbool.h>
#include <stdint.h>
#include <math.h>
#include <tgmath.h>
#include <complex.h>
#include <assert.h>
#include <ctype.h>
#include "practice.h"
#include <stdio.h>
#include <stdlib.h>
#include "practice.h"

AdjListNode *newAdjListNode(int dest, int weight)
{
    AdjListNode *newNode = malloc(sizeof(AdjListNode));
    assert(newNode);
    newNode->dest = dest;
    newNode->weight = weight;
    newNode->next = NULL;
    return newNode;
}

Graph *createGraph(int V)
{
    Graph *graph = malloc(sizeof(Graph));
    assert(graph);

    graph->V = V;

    // Create an array of adjacency lists
    graph->array = malloc(V * sizeof(AdjList));
    for (int i = 0; i < V; i++)
    {
        graph->array[i].head = NULL;
    }
    return graph;
}

void addEdge(Graph *graph, int src, int dest, int weight)
{
    // Add an edge from src to dest.
    AdjListNode *newNode = newAdjListNode(dest, weight);
    newNode->next = graph->array[src].head;
    graph->array[src].head = newNode;

    // If the graph is undirected, also add an edge from dest to src
    newNode = newAdjListNode(src, weight);
    newNode->next = graph->array[dest].head;
    graph->array[dest].head = newNode;
}

MinHeapNode *newMinHeapNode(int v, int dist)
{
    MinHeapNode *minHeapNode = malloc(sizeof(MinHeapNode));
    assert(minHeapNode);
    minHeapNode->v = v;
    minHeapNode->dist = dist;
    return minHeapNode;
}
MinHeap *createMinHeap(int capacity)
{
    MinHeap *minHeap = malloc(sizeof(MinHeap));
    minHeap->pos = malloc(capacity * sizeof(int));
    minHeap->size = 0;
    minHeap->capacity = capacity;
    minHeap->array = malloc(capacity * sizeof(MinHeapNode *));
    return minHeap;
}
void swapMinHeapNode(MinHeapNode **a, MinHeapNode **b)
{
    MinHeapNode *t = *a;
    *a = *b;
    *b = t;
}
void minHeapify(MinHeap *minHeap, int idx)
{
    int smallest, left, right;
    smallest = idx;
    left = 2 * idx + 1;
    right = 2 * idx + 2;

    if (left < minHeap->size && minHeap->array[left]->dist < minHeap->array[smallest]->dist)
    {
        smallest = left;
    }

    if (right < minHeap->size && minHeap->array[right]->dist < minHeap->array[smallest]->dist)
    {
        smallest = right;
    }

    if (smallest != idx)
    {
        // the nodes to be swapped in min heap
        MinHeapNode *smallestNode = minHeap->array[smallest];
        MinHeapNode *idxNode = minHeap->array[idx];

        // Swap positions
        minHeap->pos[smallestNode->v] = idx;
        minHeap->pos[idxNode->v] = smallest;

        // swap nodes
        swapMinHeapNode(&minHeap->array[smallest], &minHeap->array[idx]);

        minHeapify(minHeap, smallest);
    }
}
int isEmpty(MinHeap *minHeap)
{
    return minHeap->size == 0;
}
MinHeapNode *extractMin(MinHeap *minHeap)
{
    if (isEmpty(minHeap))
    {
        return NULL;
    }

    // store the root node
    MinHeapNode *root = minHeap->array[0];

    // Replace root with last node
    MinHeapNode *lastNode = minHeap->array[minHeap->size - 1];
    minHeap->array[0] = lastNode;

    // Update position of last node
    minHeap->pos[root->v] = minHeap->size - 1;
    minHeap->pos[lastNode->v] = 0;

    // reduce heap size and heapify root
    minHeap->size--;
    minHeapify(minHeap, 0);
    return root;
}

// func to decrease distance value of a given vertex
void decreaseKey(MinHeap *minHeap, int v, int dist)
{
    // Get the index of v in heap array
    int i = minHeap->pos[v];

    // Get the node and update its distance
    minHeap->array[i]->dist = dist;

    // Travel up while the complete tree is not heapified
    // or the current node is smaller than its parent
    while (i && minHeap->array[i]->dist < minHeap->array[(i - 1) / 2]->dist)
    {
        // Swap with parent
        minHeap->pos[minHeap->array[i]->v] = (i - 1) / 2;
        minHeap->pos[minHeap->array[(i - 1) / 2]->v] = i;
        swapMinHeapNode(&minHeap->array[i], &minHeap->array[(i - 1) / 2]);

        // Move to parent index
        i = (i - 1) / 2;
    }
}
int isInMinHeap(MinHeap *minHeap, int v)
{
    if (minHeap->pos[v] < minHeap->size)
    {
        return 1;
    }
    return 0;
}

// func that implements Dijkstra's single source shortest path algorithm
void dijkstra(Graph *graph, int src)
{
    int V = graph->V;
    int dist[V]; // The output array. dist[i] will hold the shortest distance from src to i

    // initialize min heap
    MinHeap *minHeap = createMinHeap(V);

    // Initialize distances and min heap
    for (int v = 0; v < V; v++)
    {
        dist[v] = INT_MAX;
        minHeap->array[v] = newMinHeapNode(v, dist[v]);
        minHeap->pos[v] = v;
    }

    // set distance of src vertex to itself
    minHeap->array[src]->dist = 0;
    dist[src] = 0;
    decreaseKey(minHeap, src, dist[src]);

    // Initialize size of min heap
    minHeap->size = V;

    // Main loop
    while (!isEmpty(minHeap))
    {
        // Extract the vertex with minimum distance value
        MinHeapNode *minHeapNode = extractMin(minHeap);
        int u = minHeapNode->v;

        // Traverse thru all adjacent vertices of u
        AdjListNode *pCrawl = graph->array[u].head;
        while (pCrawl != NULL)
        {
            int v = pCrawl->dest;

            // if shortest to v is not finalized and distanced to v thru u is less than its current distance
            if (isInMinHeap(minHeap, v) && dist[u] != INT_MAX && pCrawl->weight + dist[u] < dist[v])
            {
                dist[v] = dist[u] + pCrawl->weight;
                decreaseKey(minHeap, v, dist[v]);
            }
            pCrawl = pCrawl->next;
        }
        free(minHeapNode);
    }

    // print the calculated shortest distances
    printf("Vertex \t\t Distance from Source\n");
    for (int i = 0; i < V; i++)
    {
        printf("%d \t\t %d\n", i, dist[i]);
    }

        free(minHeap->array);
    free(minHeap->pos);
    free(minHeap);
}

void freeGraph(Graph *graph, int V)
{
    // Free allocated memory for graph
    for (int v = 0; v < V; ++v)
    {
        AdjListNode *temp = graph->array[v].head;
        while (temp != NULL)
        {
            AdjListNode *toFree = temp;
            temp = temp->next;
            free(toFree);
        }
    }
    free(graph->array);
    free(graph);
}
```

`practice.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include "practice.h"

int main()
{
    int V = 9;
    Graph *graph = createGraph(V);
    addEdge(graph, 0, 1, 4);
    addEdge(graph, 0, 7, 8);
    addEdge(graph, 1, 2, 8);
    addEdge(graph, 1, 7, 11);
    addEdge(graph, 2, 3, 7);
    addEdge(graph, 2, 8, 2);
    addEdge(graph, 2, 5, 4);
    addEdge(graph, 3, 4, 9);
    addEdge(graph, 3, 5, 14);
    addEdge(graph, 4, 5, 10);
    addEdge(graph, 5, 6, 2);
    addEdge(graph, 6, 7, 1);
    addEdge(graph, 6, 8, 6);
    addEdge(graph, 7, 8, 7);

    dijkstra(graph, 0);
    freeGraph(graph, V);
    return 0;
}

```

##### Explanation

Let's break down the implementation step-by-step to understand how Dijkstra's Algorithm works in this C program.

1. **Graph Representation**

- **Adjacency List:** The graph is represented using an adjacency list, where each vertex has a list of its adjacent vertices along with the weights of the connecting edges.
- **Edge Addition:** The `addEdge` function adds an edge to the graph by creating a new adjacency list node for both the source and destination vertices since the graph is undirected.

2. **Min Heap (Priority Queue)**

- **Purpose:** To efficiently select the vertex with the smallest tentative distance at each step.
- **Structure:**
  - **MinHeapNode:** Contains the vertex number (`v`) and its current distance (`dist`) from the source.
  - **MinHeap:** Contains an array of pointers to `MinHeapNode`, the current size, capacity, and a position array to keep track of each vertex's position in the heap.
- **Key Functions:**
  - **createMinHeap:** Initializes the min heap with a given capacity.
  - **extractMin:** Removes and returns the node with the smallest distance.
  - **decreaseKey:** Updates the distance value of a given vertex and repositions it in the heap to maintain the min-heap property.
  - **minHeapify:** Ensures the heap maintains its min-heap property after any changes.

3. **Dijkstra's Algorithm**

- **Initialization:**
  - Set the distance to all vertices as infinity (`INT_MAX`), except the source vertex, which is set to 0.
  - Insert all vertices into the min heap with their initial distances.
- **Main Loop:**
  - While the min heap is not empty:
    - Extract the vertex `u` with the minimum distance from the heap.
    - Iterate through all adjacent vertices `v` of `u`.
    - If the path from the source to `v` via `u` is shorter than the current known distance to `v`, update `dist[v]` and decrease the key in the min heap.
- **Termination:**
  - After processing all vertices, the `dist` array contains the shortest distances from the source to every other vertex.

4. **Memory Management**

- **Allocation:** Memory is dynamically allocated for the graph, adjacency list nodes, min heap nodes, and the min heap itself.
- **Deallocation:** After completing the algorithm, all dynamically allocated memory is freed to prevent memory leaks.

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==14370== Memcheck, a memory error detector
==14370== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==14370== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==14370== Command: ./practice
==14370== 
Vertex 		 Distance from Source
0 		 0
1 		 4
2 		 12
3 		 19
4 		 21
5 		 11
6 		 9
7 		 8
8 		 14
==14370== 
==14370== HEAP SUMMARY:
==14370==     in use at exit: 0 bytes in 0 blocks
==14370==   total heap usage: 43 allocs, 43 frees, 1,764 bytes allocated
==14370== 
==14370== All heap blocks were freed -- no leaks are possible
==14370== 
==14370== For lists of detected and suppressed errors, rerun with: -s
==14370== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**Explanation:**

- **Vertex `0`:** Distance `0` (source vertex).
- **Vertex `1`:** The shortest path from `0` to `1` has a distance of `4`.
- **Vertex `2`:** The shortest path from `0` to `2` has a distance of `12`.
- **Vertex `3`:** The shortest path from `0` to `3` has a distance of `19`.
- **Vertex `4`:** The shortest path from `0` to `4` has a distance of `21`.
- **Vertex `5`:** The shortest path from `0` to `5` has a distance of `11`.
- **Vertex `6`:** The shortest path from `0` to `6` has a distance of `9`.
- **Vertex `7`:** The shortest path from `0` to `7` has a distance of `8`.
- **Vertex `8`:** The shortest path from `0` to `8` has a distance of `14`.

**Example Interpretation:**

- **Shortest Path from `0` to `5`:**

  - Path: `0 -> 1 -> 2 -> 5`
  - Total Weight: `4 (0->1) + 8 (1->2) + 4 (2->5) = 16`

  *However, the output shows a distance of `11`, indicating a more efficient path exists.*

- **Optimal Path:**

  - Path: `0 -> 7 -> 6 -> 5`
  - Total Weight: `8 (0->7) + 1 (7->6) + 2 (6->5) = 11`

This demonstrates that Dijkstra's algorithm successfully identified the most efficient paths based on the edge weights.

---

### C implementation (O(V<sup>2</sup>))

#### Program Code

`hello.h`

```c
#define V 6

int minDistance(int dist[], bool sptSet[]);
void printSolution(int dist[], int parent[]);
void dijkstra(int graph[V][V], int src);

```

`hello.c`

```c
int minDistance(int dist[], bool sptSet[])
{
    int min = INT_MAX, min_idx;

    for (int v = 0; v < V; v++)
    {
        if (!sptSet[v] && dist[v] <= min)
        {
            min = dist[v];
            min_idx = v;
        }
    }
    return min_idx;
}
void printSolution(int dist[], int parent[])
{
    printf("Vertex\tDistance\tParent\n");
    for (int i = 0; i < V; i++)
    {
        if (dist[i] == INT_MAX)
        {
            printf("Infinity\tNo path\n");
            continue;
        }
        printf("%d\t%d\t\t", i, dist[i]);
        // Reconstruct the path from source to i
        int path[V];
        int count = 0;
        int crawl = i;
        path[count++] = crawl;
        while (parent[crawl] != -1)
        {
            crawl = parent[crawl];
            path[count++] = crawl;
        }

        // Print the path in reverse order
        for (int j = count - 1; j >= 0; j--)
        {
            printf("%d", path[j]);
            if (j > 0)
                printf(" -> ");
            else
                printf("\n");
        }
    }
}
void dijkstra(int graph[V][V], int src)
{
    int dist[V];    // distance array to store the shortest distance from the source
    bool sptSet[V]; // shortest path tree set
    int parent[V];  // array to store the parent of each node

    // initialize all distances as INFINITE and sptSet[] as false
    for (int i = 0; i < V; i++)
    {
        dist[i] = INT_MAX;
        sptSet[i] = false;
        parent[i] = -1; // no parent initially
    }

    // distance of src vertex from itself is always 0
    dist[src] = 0;

    // find the shortest path for all vertices
    for (int count = 0; count < V - 1; count++)
    {
        int u = minDistance(dist, sptSet); // pick the min distance vertex

        if (u == -1)
        {
            break; // no reachable vertex remaining
        }

        // mark the vertex as processed
        sptSet[u] = true;

        // Update dist value of the adjacent vertices of the picked vertex.
        for (int v = 0; v < V; v++)
        {
            // Update dist[v] only if there is an edge from u to v,
            // and total weight of path from src to v through u is smaller than current value of dist[v]
            if (graph[u][v] != 0 && !sptSet[v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v])
            {
                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }
    printSolution(dist, parent);
}
```

`main.c`

```c
int main()
{
    int graph[V][V] = {
        {0, 5, 2, 0, 0, 0},
        {0, 0, 0, 4, 2, 0},
        {0, 0, 0, 0, 7, 0},
        {0, 0, 0, 0, 0, 3},
        {0, 0, 0, 6, 0, 1},
        {0, 0, 0, 0, 0, 0},
    };

    int source = 0; // Starting vertex
    dijkstra(graph, source);
    return 0;
}

```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Vertex	Distance	Parent
0	0		0
1	5		0 -> 1
2	2		0 -> 2
3	9		0 -> 1 -> 3
4	7		0 -> 1 -> 4
5	8		0 -> 1 -> 4 -> 5
```

