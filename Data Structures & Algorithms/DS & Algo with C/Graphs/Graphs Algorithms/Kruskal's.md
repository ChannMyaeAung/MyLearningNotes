# Kruskal's Algorithm

- Kruskal's algorithm is a greedy algorithm used to find a Minimum Spanning Tree (MST) for a weighted, undirected graph. 
- A Minimum Spanning Tree is a subgraph of the original graph in which we connect all the vertices, and the total cost of the edges included is the minimum among all Spanning Trees.
- The goal of the algorithm is to connect all vertices with the minimum possible total edge weight while avoiding cycles.

## **Real-World Applications of Kruskal's Algorithm:**

Kruskal's algorithm finds extensive use in various fields due to its efficiency in creating minimal spanning trees, which represent the most economical way to connect all points (vertices) without cycles. Some notable applications include:

1. **Network Design:**
   - **Telecommunication Networks:**
     Designing optimal wiring or cable layouts to connect multiple cities or nodes with minimum cost.
   - **Computer Networks:**
     Building efficient and cost-effective LANs, WANs, or internet backbone infrastructure.
2. **Electrical Grid Design:**
   - Creating the most cost-effective way of connecting power stations, substations, and consumers while minimizing the amount of wiring needed.
3. **Road and Railway Networks:**
   - Planning road systems or railway lines to connect a set of cities or stations with minimal construction cost.
4. **Clustering in Machine Learning:**
   - In hierarchical clustering methods, Kruskal's algorithm can be used to construct a minimum spanning tree which helps in clustering data points by progressively cutting the longest edges in the MST.
5. **Approximation Algorithms:**
   - **Traveling Salesman Problem (TSP):**
     A minimum spanning tree can serve as a starting point or a heuristic for approximation algorithms tackling the TSP.
6. **Computer Graphics and Image Processing:**
   - **Segmentation:**
     In some segmentation algorithms, MSTs help in grouping similar pixels or features together.

These applications leverage Kruskal's algorithm for its simplicity, efficiency, and capability to produce optimal solutions in scenarios where the structure of connections matters most and costs must be minimized.

## Algorithm Steps:

1. **Sort Edges:** Start by sorting all the edges of the graph in non-decreasing order of their weights.
2. **Initialize Disjoint Sets:** Create a disjoint-set (union-find) data structure where each vertex starts in its own set. This structure helps quickly check whether adding an edge will form a cycle.
3. Select Edges:
   - Iterate over the sorted edges. For each edge  `(u, v)`:
     - Use the union-find structure to check if vertices `u` and `v` belong to different sets.
     - If they are in different sets, include this edge in the MST and merge the sets (union operation).
     - If they are in the same set, skip the edge to avoid forming a cycle.
4. **Stop Condition:** The algorithm stops when there are `V-1` edges in the MST (where `V` is the number of vertices), or when all edges have been processed.

**Union-Find Structure:**

- **Find:** Determine which subset a particular element belongs to. This can be optimized using path compression.
- **Union:** Join two subsets into a single subset if they are not already in the same subset. This can be optimized using union by rank or size.



## C Implementation

Below is an example of Kruskal's algorithm implemented in C. This code includes the necessary structures and functions for sorting edges, performing union-find operations, and constructing the MST.

#### Program Code

`hello.h`

```C
// represent an edge
typedef struct
{
    int src, dest;
    int weight;
} Edge;

// represent a graph
typedef struct
{
    int V, E;
    Edge *edges;
} Graph;

// to represent a subset for union-find
typedef struct
{
    int parent;
    int rank;
} Subset;

Graph *createGraph(int V, int E);
int find(Subset subsets[], int i);
void Union(Subset subsets[], int x, int y);
int compareEdges(const void *a, const void *b);
void KruskalMST(Graph *g);
```

`hello.c`

```C
Graph *createGraph(int V, int E)
{
    Graph *graph = malloc(sizeof(Graph));
    graph->V = V;
    graph->E = E;
    graph->edges = malloc(E * sizeof(Edge));
    return graph;
}


// a utility func to find set of an element i (uses path compression for efficiency)
int find(Subset subsets[], int i)
{
    if (subsets[i].parent != i)
    {
        subsets[i].parent = find(subsets, subsets[i].parent);
    }
    return subsets[i].parent;
}

// a func that does union of two sets of x and y (uses union by rank)
// merges two subsets into a single subset based on rank, minimizing tree height
void Union(Subset subsets[], int x, int y)
{
    int xroot = find(subsets, x);
    int yroot = find(subsets, y);

    // Attach smaller rank tree under root of higher rank tree
    if (subsets[xroot].rank < subsets[yroot].rank)
    {
        subsets[xroot].parent = yroot;
    }
    else if (subsets[xroot].rank > subsets[yroot].rank)
    {
        subsets[yroot].parent = xroot;
    }
    else
    {
        subsets[yroot].parent = xroot;
        subsets[xroot].rank++;
    }
}
// compare func for qsort
int compareEdges(const void *a, const void *b)
{
    Edge *A = (Edge *)a;
    Edge *B = (Edge *)b;
    return A->weight - B->weight;
}


void KruskalMST(Graph *g)
{
    int V = g->V;

    Edge result[V]; // this will store the resultant MST

    int e = 0; // Index for result[]
    int i = 0; // Index for sorted edges

    // step 1: Sort all edges in non-decreasing order of their weight.
    qsort(g->edges, g->E, sizeof(Edge), compareEdges);

    // Allocate memory for creating V subsets
    Subset *subsets = malloc(V * sizeof(Subset));

    // Create V subsets with single elements
    for (int v = 0; v < V; v++)
    {
        subsets[v].parent = v;
        subsets[v].rank = 0;
    }

    // Num of edges to be taken is equal to V - 1
    while (e < V - 1 && i < g->E)
    {
        // step 2: Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far.
        Edge next_edge = g->edges[i++];

        int x = find(subsets, next_edge.src);
        int y = find(subsets, next_edge.dest);

        // if including this edge does not cause a cycle, include it in the result
        if (x != y)
        {
            result[e++] = next_edge;
            Union(subsets, x, y);
        }
        // Else discard the next_edge
    }

    // print the contents of the MST
    printf("Following are the edges in the constructed MST:\n");
    int minCost = 0;
    for (i = 0; i < e; i++)
    {
        printf("%d -- %d == %d\n", result[i].src, result[i].dest, result[i].weight);
        minCost += result[i].weight;
    }
    printf("Minimum Cost Spanning Tree: %d\n", minCost);

    free(subsets);
}
```

`main.c`

```C
int main()
{
    int V, E;
    printf("Enter number of vertices and edges: ");
    scanf("%d %d", &V, &E);

    Graph *graph = createGraph(V, E);

    printf("Enter each edge in the format: src dest weight\n");
    for (int i = 0; i < E; i++)
    {
        scanf("%d %d %d", &graph->edges[i].src, &graph->edges[i].dest, &graph->edges[i].weight);
    }

    KruskalMST(graph);

    free(graph->edges);
    free(graph);
    return 0;
}
```



#### Code explanation

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==9783== Memcheck, a memory error detector
==9783== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==9783== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==9783== Command: ./final
==9783== 
Enter number of vertices and edges: 4 5
Enter each edge in the format: src dest weight
0 1 10
0 2 6
0 3 5
1 3 15
2 3 4
Following are the edges in the constructed MST:
2 -- 3 == 4
0 -- 3 == 5
0 -- 1 == 10
Minimum Cost Spanning Tree: 19
==9783== 
==9783== HEAP SUMMARY:
==9783==     in use at exit: 0 bytes in 0 blocks
==9783==   total heap usage: 5 allocs, 5 frees, 2,156 bytes allocated
==9783== 
==9783== All heap blocks were freed -- no leaks are possible
==9783== 
==9783== For lists of detected and suppressed errors, rerun with: -s
==9783== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

This program demonstrates how Kruskal's algorithm efficiently builds a minimum spanning tree 

- by always selecting the smallest possible edge that does not form a cycle, using a union-find data structure to keep track of connected components.

---