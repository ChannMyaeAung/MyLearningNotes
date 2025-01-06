## Overview of the Bellman-Ford Algorithm

- **Bellman-Ford** is an algorithm for finding the shortest paths from a single source vertex to all other vertices in a weighted graph. 

- It is capable of handling graphs with negative edge weights and can detect negative weight cycles, which makes it more versatile than Dijkstra's algorithm in certain scenarios.

**Key Characteristics:**

- **Graph Type:** Directed or undirected (treated as directed with edges in both directions), with possible negative edge weights.
- **Time Complexity:** O(V * E), where V is the number of vertices and E is the number of edges.
- **Space Complexity:** O(V + E) with an adjacency list representation.
- **Applications:** Routing protocols (e.g., Bellman-Ford is used in the RIP protocol), detecting arbitrage opportunities in currency exchange, etc.

**Advantages:**

- Handles graphs with negative edge weights.
- Detects negative weight cycles.

**Disadvantages:**

- Less efficient than Dijkstra's algorithm for graphs without negative weights.
- Higher time complexity (O(V * E)) makes it less suitable for very large graphs.

---

## When to Use Bellman-Ford

- **Negative Edge Weights:** When our graph contains edges with negative weights, and we need to find shortest paths.
- **Negative Cycles Detection:** When we need to check for the existence of negative weight cycles, which indicate that no shortest path exists.
- **Simple Implementation:** Bellman-Ford is straightforward to implement, especially when using an edge list representation.

**Note:** If the graph doesn't have negative edge weights, Dijkstra's algorithm is generally more efficient.

---

## Algorithm Steps

The Bellman-Ford algorithm operates in two main phases:

1. **Relaxation Phase:**
   - Iterate |V| - 1 times over all edges.
   - For each edge (u, v) with weight w, if the current distance to v can be shortened by going through u, update the distance to v.
2. **Negative Cycle Detection Phase:**
   - Iterate once more over all edges.
   - If any distance can still be shortened, a negative weight cycle exists in the graph.

**Why |V| - 1 Iterations?**

In the worst case, the shortest path can have |V| - 1 edges. Therefore, after |V| - 1 iterations, all shortest paths should be found if no negative cycles exist.

---

## C Implementation

We'll implement the Bellman-Ford algorithm in C using an **Edge List** representation for simplicity. This approach is efficient for Bellman-Ford since the algorithm primarily iterates over edges.

#### Program Code

`practice.h`

```C
typedef struct Edge
{
    int src;
    int dest;
    int weight;
} Edge;

typedef struct Graph
{
    int V;      // num of vertices
    int E;      // num of edges
    Edge *edge; // array of edges
} Graph;

Graph *createGraph(int V, int E);
void printArr(int dist[], int V);

int bellmanFord(Graph *g, int src);
void freeGraph(Graph *g);
```

`functions.c`

```C
Graph *createGraph(int V, int E)
{
    Graph *g = malloc(sizeof(Graph));
    g->V = V;
    g->E = E;
    g->edge = malloc(E * sizeof(Edge));
    return g;
}

void printArr(int dist[], int V)
{
    printf("Vertex\tDistance from Source\n");
    for (int i = 0; i < V; i++)
    {
        printf("%d\t\t%d\n", i, dist[i]);
    }
}


int bellmanFord(Graph *g, int src)
{
    int V = g->V;
    int E = g->E;
    int dist[V];

    // step 1: initialize distances from src to all other vertices as INFINITE
    for (int i = 0; i < V; i++)
    {
        dist[i] = INT_MAX;
    }
    dist[src] = 0;

    // Step 2: Relax all edges |V| - 1 times
    // Relaxing an edge means updating the distance to the destination vertex if a shorter path is found through the source vertex.
    for (int i = 1; i <= V - 1; i++)
    {
        for (int j = 0; j < E; j++)
        {
            int u = g->edge[j].src;
            int v = g->edge[j].dest;
            int weight = g->edge[j].weight;
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v])
            {
                dist[v] = dist[u] + weight;
            }
        }
    }

    // Step 3: Check for negative-weight cycles
    // After the relaxation phase, iterate over all edges once more to check if any distance can still be minimized. If so, it indicates the presence of a negative weight cycle.
    for (int i = 0; i < E; i++)
    {
        int u = g->edge[i].src;
        int v = g->edge[i].dest;
        int weight = g->edge[i].weight;
        if (dist[u] != INT_MAX && dist[u] + weight < dist[v])
        {
            printf("Graph contains negative weight cycle\n");
            return 0; // indicates negative weight cycle
        }
    }

    // If no negative cycle is detected, print the shortest distances from the source to all vertices.
    printArr(dist, V);
    return 1; // indicate successful completion
}
void freeGraph(Graph *g)
{
    free(g->edge);
    free(g);
}
```

`practice.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include "hello.h"

int main()
{
    /*
        Example Graph:
        Number of vertices V = 5
        Number of edges E = 8

        Edge list:
        0 -> 1 (weight -1)
        0 -> 2 (weight 4)
        1 -> 2 (weight 3)
        1 -> 3 (weight 2)
        1 -> 4 (weight 2)
        3 -> 2 (weight 5)
        3 -> 1 (weight 1)
        4 -> 3 (weight -3)
    */
    int V = 5;
    int E = 8;

    Graph *g = createGraph(V, E);

    // adding edges
    g->edge[0].src = 0;
    g->edge[0].dest = 1;
    g->edge[0].weight = -1;

    g->edge[1].src = 0;
    g->edge[1].dest = 2;
    g->edge[1].weight = 4;

    g->edge[2].src = 1;
    g->edge[2].dest = 2;
    g->edge[2].weight = 3;

    g->edge[3].src = 1;
    g->edge[3].dest = 3;
    g->edge[3].weight = 2;

    g->edge[4].src = 1;
    g->edge[4].dest = 4;
    g->edge[4].weight = 2;

    g->edge[5].src = 3;
    g->edge[5].dest = 2;
    g->edge[5].weight = 5;

    g->edge[6].src = 3;
    g->edge[6].dest = 1;
    g->edge[6].weight = 1;

    g->edge[7].src = 4;
    g->edge[7].dest = 3;
    g->edge[7].weight = -3;

    // Run Bellman-Ford algorithm from source vertex 0
    int src = 0;
    if (bellmanFord(g, src))
    {
        // Successfully found shortest paths
    }

    freeGraph(g);
    return 0;
}
```

#### Program Output & Explanation

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==20508== Memcheck, a memory error detector
==20508== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20508== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20508== Command: ./final
==20508== 
Vertex	Distance from Source
0		0
1		-1
2		2
3		-2
4		1
==20508== 
==20508== HEAP SUMMARY:
==20508==     in use at exit: 0 bytes in 0 blocks
==20508==   total heap usage: 3 allocs, 3 frees, 1,136 bytes allocated
==20508== 
==20508== All heap blocks were freed -- no leaks are possible
==20508== 
==20508== For lists of detected and suppressed errors, rerun with: -s
==20508== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**Explanation:**

- *
- **Vertex 0:** Distance is **0** (source vertex).
- **Vertex 1:** Shortest path is directly from **0 → 1** with a total weight of **-1**.
- **Vertex 2:** Shortest path is **0 → 1 → 2** with a total weight of **-1 + 3 = 2**.
- **Vertex 3:** Shortest path is **0 → 1 → 4 → 3** with a total weight of **-1 + 2 + (-3) = -2**.
- **Vertex 4:** Shortest path is **0 → 1 → 4** with a total weight of **-1 + 2 = 1**.

```css
Shortest Paths from Vertex 0:

0 → 1 (Weight: -1)

0 → 1 → 2 (Weight: 2)

0 → 1 → 4 → 3 (Weight: -2)

0 → 1 → 4 (Weight: 1)
```



