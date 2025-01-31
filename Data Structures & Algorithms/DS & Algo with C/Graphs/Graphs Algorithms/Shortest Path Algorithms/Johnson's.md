# Johnson's Algorithm

- **Johnson's Algorithm** is an efficient method to find the shortest paths between all pairs of vertices in a sparse, weighted, directed graph. 
- It accommodates negative edge weights but requires that the graph does not contain any negative weight cycles.

---

## Why Use Johnson's Algorithm?

- **Efficiency on Sparse Graphs:** Johnson's Algorithm runs in O(V * E+V<sup>2</sup> * log⁡ V) time when implemented with a Fibonacci heap, making it more efficient than Floyd-Warshall's O(V<sup>3</sup>) for sparse graphs.
- **Handling Negative Weights:** Unlike Dijkstra's Algorithm, which cannot handle negative edge weights, Johnson's Algorithm can process graphs with negative weights by reweighting the edges to eliminate negative weights.

---

## Applications

- **Network Routing:** Determining optimal paths in communication networks.
- **Urban Planning:** Designing efficient transportation or utility distribution systems.
- **Bioinformatics:** Comparing genetic sequences.
- **Social Network Analysis:** Measuring closeness or influence between individuals.

---

## Algorithm Overview

Johnson's Algorithm combines the strengths of both the Bellman-Ford and Dijkstra's algorithms to compute all-pairs shortest paths efficiently. The algorithm proceeds in the following steps:

1. **Add a New Vertex:** Introduce a new vertex `q` and connect it to every other vertex in the graph with an edge of weight 0.

2. **Run Bellman-Ford Algorithm:** Execute the Bellman-Ford algorithm from the new vertex `q` to compute a potential function `h(v)` for each vertex `v`. This potential is used to reweight the original graph to eliminate negative edge weights.

3. **Reweight the Graph:** Adjust the weights of the original graph's edges using the potential `h` such that all edge weights become non-negative. The new weight `w′(u,v)` is calculated as:

   `w′(u,v)=w(u,v)+h(u)−h(v)`

4. **Remove the Added Vertex:** Delete the newly added vertex `q` and its associated edges.

5. **Run Dijkstra's Algorithm:** For each vertex `u` in the original graph, perform Dijkstra's algorithm to find the shortest paths from `u` to all other vertices using the reweighted graph.

6. **Adjust the Shortest Paths:** Convert the reweighted shortest path distances back to the original graph's weights using the potential `h`:

   `d(u,v)=d′(u,v)−h(u)+h(v)`

7. **Detect Negative Weight Cycles:** If the Bellman-Ford algorithm detects a negative weight cycle, terminate as shortest paths are undefined.

---

### Why It Works

- The key idea is to reweight the edges to eliminate negative weights without altering the relative shortest path relationships. 
- By computing the potential `h` using Bellman-Ford, the algorithm ensures that all reweighted edge weights are non-negative, making Dijkstra's Algorithm applicable.

---

## Key Components

### Graph Representation

For efficient implementation, the graph is represented using an **adjacency list**, which is suitable for sparse graphs. Each vertex maintains a list of its outgoing edges, each characterized by the destination vertex and the edge weight.

### Bellman-Ford Algorithm

**Purpose:** Detect negative weight cycles and compute the potential `h(v)` for each vertex.

**Key Steps:**

1. Initialize the distance to all vertices as infinity, except the source vertex `q`, which is set to 0.
2. Relax all edges `V−1` times, where V is the number of vertices.
3. Check for negative weight cycles by attempting to relax the edges one more time. If any distance can still be updated, a negative weight cycle exists.

### Dijkstra's Algorithm

**Purpose:** Find the shortest paths from a single source vertex to all other vertices in a graph with non-negative edge weights.

**Key Steps:**

1. Initialize the distance to all vertices as infinity, except the source vertex, which is set to 0.
2. Use a priority queue to select the vertex with the smallest tentative distance.
3. Relax all adjacent edges of the selected vertex.
4. Repeat until all vertices have been processed.

**Implementation Note:** To achieve O(E +  V * log⁡ V) time per Dijkstra run, a **min-heap** or **priority queue** is used.

------

## C Implementation of Johnson's Algorithm

Implementing Johnson's Algorithm in C involves several components:

1. **Graph Representation:** Using adjacency lists for efficiency.
2. **Edge Structure:** To store destination and weight.
3. **Bellman-Ford Implementation:** To compute potentials and detect negative cycles.
4. **Dijkstra's Implementation:** Efficiently finding shortest paths using a priority queue.
5. **Reweighting Mechanism:** Adjusting edge weights based on potentials.
6. **Result Adjustment:** Converting reweighted distances back to original weights.

#### Program Code

`practice.h`

```c
typedef struct Edge
{
    int dest;
    int weight;
    struct Edge *next;
} Edge;

typedef struct Graph
{
    int V;
    Edge **adj;
} Graph;

// func to create a new edge
Edge *newEdge(int dest, int weight);

// func to create a graph with V vertices
Graph *createGraph(int V);

// func to add an edge to the graph
void addEdge(Graph *g, int src, int dest, int weight);

// Min Heap Node
typedef struct MinHeapNode
{
    int v;
    int dist;
} MinHeapNode;

// Min Heap structure
typedef struct MinHeap
{
    int size;
    int capacity;
    int *pos; // position of vertex in heap
    MinHeapNode **array;
} MinHeap;

// func to create a new Min Heap Node
MinHeapNode *newMinHeapNode(int v, int dist);

// func to create a Min Heap
MinHeap *createMinHeap(int capacity);

// func to swap two min heap nodes
void swapMinHeapNode(MinHeapNode **a, MinHeapNode **b);

// func to heapify at given index
void minHeapify(MinHeap *minHeap, int idx);

// func to check if min heap is empty
int isEmpty(MinHeap *minHeap);

// func to extract the minimum element from min heap
MinHeapNode *extractMin(MinHeap *minHeap);

// func to decrease distance value of a given vertex
void decreaseKey(MinHeap *minHeap, int v, int dist);

// func to check if a given vertex is in min heap
int isInMinHeap(MinHeap *minHeap, int v);

// bellman-ford algo to detect negative cycles and compute potentials
int bellmanFord(Graph *g, int src, int *h);

// Dijkstra algorithm using min heap
void dijkstra(Graph *g, int src, int *dist, int V);

// Johnson's algorithm implementation
int johnson(Graph *g, int V, int **all_dist);

// func to print all-pairs shortest paths
void printAllPairsShortestPaths(int V, int **all_dist);

void freeGraph(Graph *g);
#endif

```

`functions.c`

```c
// func to create a new edge
Edge *newEdge(int dest, int weight)
{
    Edge *edge = malloc(sizeof(Edge));
    assert(edge != NULL);
    edge->dest = dest;
    edge->weight = weight;
    edge->next = NULL;
    return edge;
}

// func to create a graph with V vertices
Graph *createGraph(int V)
{
    Graph *g = malloc(sizeof(Graph));
    assert(g != NULL);
    g->V = V;
    g->adj = malloc(V * sizeof(Edge *));
    assert(g->adj != NULL);
    for (int i = 0; i < V; i++)
    {
        g->adj[i] = NULL;
    }
    return g;
}

// func to add an edge to the graph
void addEdge(Graph *g, int src, int dest, int weight)
{
    Edge *e = newEdge(dest, weight);
    e->next = g->adj[src];
    g->adj[src] = e;
}

// func to create a new Min Heap Node
MinHeapNode *newMinHeapNode(int v, int dist)
{
    MinHeapNode *node = malloc(sizeof(MinHeapNode));
    assert(node != NULL);
    node->v = v;
    node->dist = dist;
    return node;
}

// func to create a Min Heap
MinHeap *createMinHeap(int capacity)
{
    MinHeap *minHeap = malloc(sizeof(MinHeap));
    assert(minHeap != NULL);
    minHeap->size = 0;
    minHeap->capacity = capacity;
    minHeap->pos = malloc(capacity * sizeof(int));
    assert(minHeap->pos != NULL);
    minHeap->array = malloc(capacity * sizeof(MinHeapNode *));
    assert(minHeap->array != NULL);
    return minHeap;
}

// func to swap two min heap nodes
void swapMinHeapNode(MinHeapNode **a, MinHeapNode **b)
{
    MinHeapNode *t = *a;
    *a = *b;
    *b = t;
}

// func to heapify at given index
void minHeapify(MinHeap *minHeap, int idx)
{
    int smallest = idx;
    int left = 2 * idx + 1;
    int right = 2 * idx + 2;

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

// func to check if min heap is empty
int isEmpty(MinHeap *minHeap)
{
    return minHeap->size == 0;
}

// func to extract the minimum element from min heap
MinHeapNode *extractMin(MinHeap *minHeap)
{
    if (isEmpty(minHeap))
    {
        return NULL;
    }

    // store the root node
    MinHeapNode *root = minHeap->array[0];

    // replace the root with last node
    MinHeapNode *lastNode = minHeap->array[minHeap->size - 1];
    minHeap->array[0] = lastNode;

    // update position of last node
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
    // get the index of vertex in heap array
    int i = minHeap->pos[v];

    // get the node and update its distance value
    minHeap->array[i]->dist = dist;

    // travel up while the complete tree is not heapified
    while (i && minHeap->array[i]->dist < minHeap->array[(i - 1) / 2]->dist)
    {
        // swap this node with its parent
        minHeap->pos[minHeap->array[i]->v] = (i - 1) / 2;
        minHeap->pos[minHeap->array[(i - 1) / 2]->v] = i;

        // move to parent index
        i = (i - 1) / 2;
    }
}

// func to check if a given vertex is in min heap
int isInMinHeap(MinHeap *minHeap, int v)
{
    if (minHeap->pos[v] < minHeap->size)
    {
        return 1;
    }
    return 0;
}

// bellman-ford algo to detect negative cycles and compute potentials
int bellmanFord(Graph *g, int src, int *h)
{
    int V = g->V;

    // initialize distances
    for (int i = 0; i < V; i++)
    {
        h[i] = INT_MAX;
    }
    h[src] = 0;

    // relax all edges V-1 times
    for (int i = 1; i < V; i++)
    {
        for (int u = 0; u < V; u++)
        {
            Edge *e = g->adj[u];
            while (e != NULL)
            {
                if (h[u] != INT_MAX && h[u] + e->weight < h[e->dest])
                {
                    h[e->dest] = h[u] + e->weight;
                }
                e = e->next;
            }
        }
    }

    // check for negative weight cycles
    for (int u = 0; u < V; u++)
    {
        Edge *e = g->adj[u];
        while (e != NULL)
        {
            if (h[u] != INT_MAX && h[u] + e->weight < h[e->dest])
            {
                // negative cycle detected
                return 0;
            }
            e = e->next;
        }
    }
    return 1; // no negative cycle
}

// Dijkstra algorithm using min heap
void dijkstra(Graph *g, int src, int *dist, int V)
{
    MinHeap *minHeap = createMinHeap(V);

    // initialize distances and insert all vertices into min heap
    for (int v = 0; v < V; v++)
    {
        dist[v] = INT_MAX;
        minHeap->array[v] = newMinHeapNode(v, dist[v]);
        minHeap->pos[v] = v;
    }

    // distance of src vertex from itself is always 0
    minHeap->array[src]->dist = 0;
    dist[src] = 0;
    decreaseKey(minHeap, src, dist[src]);

    minHeap->size = V;

    // iterate while min heap is not empty
    while (!isEmpty(minHeap))
    {
        // extract the vertex with minimum distance
        MinHeapNode *minNode = extractMin(minHeap);
        int u = minNode->v;

        // traverse thru all adj vertices of u
        Edge *e = g->adj[u];
        while (e != NULL)
        {
            int v = e->dest;

            // if shortest path to v is not finalized and distance to v thru u is less than current distance
            if (isInMinHeap(minHeap, v) && dist[u] != INT_MAX && e->weight + dist[u] < dist[v])
            {
                dist[v] = dist[u] + e->weight;

                decreaseKey(minHeap, v, dist[v]);
            }
            e = e->next;
        }
    }

    // free the allocated memory for min heap
    for (int i = 0; i < V; i++)
    {
        free(minHeap->array[i]);
    }
    free(minHeap->array);
    free(minHeap->pos);
    free(minHeap);
}

// Johnson's algorithm implementation
int johnson(Graph *g, int V, int **all_dist)
{
    // Step 1: Add a new vertex q connected to every other vertex with edge weight 0
    Graph *new_g = createGraph(V + 1);
    for (int u = 0; u < V; u++)
    {
        Edge *e = g->adj[u];
        while (e != NULL)
        {
            addEdge(new_g, u, e->dest, e->weight);
            e = e->next;
        }

        // add edge from new vertex V to u with weight 0
        addEdge(new_g, V, u, 0);
    }

    // Step 2: Run Bellman-Ford from new vertex to compute h(v)
    int *h = malloc((V + 1) * sizeof(int));
    if (!bellmanFord(new_g, V, h))
    {
        printf("Graph contains a negative weight cycle. Johnson's algorithm cannot proceed.\n");
        return 0;
    }

    // Step 3: Reweight the edges of the original graph
    // New weight w'(u, v) = w(u, v) + h(u) - h(v)
    for (int u = 0; u < V; u++)
    {
        Edge *e = g->adj[u];
        while (e != NULL)
        {
            e->weight = e->weight + h[u] - h[e->dest];
            e = e->next;
        }
    }

    // Step 4: Remove the added vertex and its edges (already done by using the original graph)
    free(new_g->adj);
    free(new_g);

    // step 5: initialize all_distances
    for (int u = 0; u < V; u++)
    {
        all_dist[u] = malloc(V * sizeof(int));
        for (int v = 0; v < V; v++)
        {
            all_dist[u][v] = INT_MAX;
        }
    }

    // step 6: run dijkstra's algo for each vertex
    for (int u = 0; u < V; u++)
    {
        int *dist = malloc(V * sizeof(int));
        dijkstra(g, u, dist, V);
        for (int v = 0; v < V; v++)
        {
            if (dist[v] != INT_MAX)
            {
                all_dist[u][v] = dist[v] - h[u] + h[v];
            }
            else
            {
                all_dist[u][v] = INT_MAX;
            }
        }
        free(dist);
    }
    free(h);
    return 1; // success
}

// func to print all-pairs shortest paths
void printAllPairsShortestPaths(int V, int **all_dist)
{
    printf("\nAll-Pairs Shortest Paths:\n");
    printf("    ");
    for (int v = 0; v < V; v++)
        printf(" %d  ", v);
    printf("\n");
    for (int u = 0; u < V; u++)
    {
        printf(" %d  ", u);
        for (int v = 0; v < V; v++)
        {
            if (all_dist[u][v] == INT_MAX)
                printf("INF ");
            else
                printf("%d  ", all_dist[u][v]);
        }
        printf("\n");
    }
}
```



`practice.c`

```c
int main()
{
    int V, E;
    printf("Johnson's All-Pairs Shortest Path Algorithm\n");
    printf("-------------------------------------------\n");

    // Input number of vertices and edges
    printf("Enter the number of vertices: ");
    scanf("%d", &V);

    printf("Enter the number of edges: ");
    scanf("%d", &E);

    // Create a graph
    Graph *graph = createGraph(V);

    printf("Enter each edge in the format 'source destination weight':\n");
    printf("(Vertices should be numbered from 0 to %d)\n", V - 1);
    for (int i = 0; i < E; i++)
    {
        int src, dest, weight;
        scanf("%d %d %d", &src, &dest, &weight);
        if (src < 0 || src >= V || dest < 0 || dest >= V)
        {
            printf("Invalid vertex number. Please enter vertices between 0 and %d.\n", V - 1);
            i--; // Retry this iteration
            continue;
        }
        addEdge(graph, src, dest, weight);
    }

    // Allocate memory for all_distances
    int **all_distances = (int **)malloc(V * sizeof(int *));
    if (all_distances == NULL)
    {
        fprintf(stderr, "Memory allocation failed for all_distances.\n");
        freeGraph(graph);
        return 1;
    }

    // Run Johnson's algorithm
    if (johnson(graph, V, all_distances))
    {
        // Print all-pairs shortest paths
        printAllPairsShortestPaths(V, all_distances);
    }
    else
    {
        printf("Johnson's algorithm failed due to negative weight cycle.\n");
    }

    // Free allocated memory
    for (int u = 0; u < V; u++)
        free(all_distances[u]);
    free(all_distances);

    // Free the graph
    freeGraph(graph);

    return 0;
}
```



#### Program Output

```sh
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==15806== Memcheck, a memory error detector
==15806== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15806== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15806== Command: ./practice
==15806== 
Johnson's All-Pairs Shortest Path Algorithm
-------------------------------------------
Enter the number of vertices: 5
Enter the number of edges: 7
Enter each edge in the format 'source destination weight':
(Vertices should be numbered from 0 to 4)
0 1 -1
0 2 4
1 2 3
1 3 2
1 4 2
3 2 5
3 1 1

All-Pairs Shortest Paths:
     0   1   2   3   4  
 0  0  0  2  1  1  
 1  INF 1  3  2  2  
 2  INF INF 0  INF INF 
 3  INF 2  4  0  3  
 4  INF INF INF INF 0  
==15806== 
==15806== HEAP SUMMARY:
==15806==     in use at exit: 0 bytes in 0 blocks
==15806==   total heap usage: 77 allocs, 77 frees, 3,356 bytes allocated
==15806== 
==15806== All heap blocks were freed -- no leaks are possible
==15806== 
==15806== For lists of detected and suppressed errors, rerun with: -s
==15806== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

