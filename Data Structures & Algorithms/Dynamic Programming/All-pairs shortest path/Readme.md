# All-Pairs Shortest Paths Problem

**Definition:**

Given a weighted graph G=(V,E), the All-Pairs Shortest Paths problem involves finding the shortest paths between every pair of vertices in V.

**Types of Graphs:**

- **Directed vs. Undirected:** The edges can be directed or undirected.
- **Weighted vs. Unweighted:** Each edge has an associated weight (cost), which can be positive, negative, or zero.
- **Presence of Negative Cycles:** A negative cycle is a cycle whose total sum of edge weights is negative. APSP algorithms must handle or detect negative cycles appropriately.

**Applications:**

- **Network Routing:** Determining optimal routes in computer or transportation networks.
- **Urban Planning:** Designing efficient transportation or utility distribution systems.
- **Social Network Analysis:** Measuring closeness or influence between individuals.
- **Geographical Mapping:** Finding shortest driving or walking routes.

---

## Dynamic Programming Approach: Floyd-Warshall Algorithm

- The **Floyd-Warshall algorithm** is a dynamic programming technique that computes the shortest paths between all pairs of vertices in a graph. 
- It's particularly effective for dense graphs and can handle negative edge weights, provided there are no negative cycles.

---

### **Why Floyd-Warshall Works:**

- **Dynamic Programming Principle:** It builds up solutions to larger subproblems based on solutions to smaller subproblems.
- **Path Relaxation:** It iteratively relaxes(relaxing means updating the shortest distance to a node if a shorter path is found through another node) the paths between vertices by considering intermediate vertices, ensuring that the shortest paths are found.
- **Comprehensive Coverage:** By considering all possible intermediate vertices, it guarantees that all shortest paths are accounted for.

---

### **Algorithm Overview:**

1. **Initialization:**
   - Create a distance matrix `dist[V][V]`, where `dist[i][j]` represents the shortest distance from vertex `i` to vertex `j`.
   - Initialize `dist[i][j]` to the weight of the edge `(i,j)` if it exists; otherwise, set it to infinity (∞).
   - Set `dist[i][i]=0` for all vertices `i`.
2. **Dynamic Programming Iteration:**
   - For each vertex  `k` in  `V`:
     - For each pair of vertices `(i,j)`in `V`:
       - Update `dist[i][j]` as follows: `dist[i][j]=min⁡(dist[i][j],dist[i][k]+dist[k][j])`
       - This step checks if the path from `i` to `j` can be optimized by going through `k`.
3. **Detection of Negative Cycles:**
   - After completion, if any `dist[i][i]<0`, the graph contains a negative cycle.
4. **Result:**
   - The distance matrix `dist` now contains the shortest distances between all pairs of vertices.

---

## C Implementation

#### Program Code

`hello.h`

```c
#define MAX_V 100
#define INF INT_MAX

// func to print the distance matrix
void printDistMatrix(int dist[][MAX_V], int V);

// func to initialize the distance matrix
void initMatrix(int g[][MAX_V], int dist[][MAX_V], int V);

// Floyd-Warshall algo implementation
void floydWarshall(int dist[][MAX_V], int V);

// func to detect negative cycles
int detectNegativeCycle(int dist[][MAX_V], int V);

```

`hello.c`

```c
// func to print the distance matrix
void printDistMatrix(int dist[][MAX_V], int V)
{
    printf("\nShortest distances between every pair of vertices:\n");
    printf("    ");
    for (int i = 0; i < V; i++)
    {
        printf("%4d ", i);
    }
    printf("\n");
    for (int i = 0; i < V; i++)
    {
        printf("%4d ", i);
        for (int j = 0; j < V; j++)
        {
            if (dist[i][j] == INF)
            {
                printf(" INF ");
            }
            else
            {
                printf("%4d ", dist[i][j]);
            }
        }
        printf("\n");
    }
}

// func to initialize the distance matrix
void initMatrix(int g[][MAX_V], int dist[][MAX_V], int V)
{
    for (int i = 0; i < V; i++)
    {
        for (int j = 0; j < V; j++)
        {
            if (i == j)
            {
                dist[i][j] = 0;
            }
            else if (g[i][j] != 0)
            {
                dist[i][j] = g[i][j];
            }
            else
            {
                dist[i][j] = INF;
            }
        }
    }
}

// Floyd-Warshall algo implementation
void floydWarshall(int dist[][MAX_V], int V)
{
    // adding vertices individually
    for (int k = 0; k < V; k++)
    {
        // pick all vertices as source one by one
        for (int i = 0; i < V; i++)
        {
            // pick all vertices as destination for the above source
            for (int j = 0; j < V; j++)
            {
                // if vertex k is on the shortest path from i to j, then update the value of dist[i][j]
                if (dist[i][k] != INF && dist[k][j] != INF && dist[i][k] + dist[k][j] < dist[i][j])
                {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
}
// func to detect negative cycles
int detectNegativeCycle(int dist[][MAX_V], int V)
{
    for (int i = 0; i < V; i++)
    {
        if (dist[i][i] < 0)
        {
            return 1; // negative cycle detected
        }
    }
    return 0; // no negative cycle detected
}
```

`main.c`

```c
int main()
{
    int V, E;
    int g[MAX_V][MAX_V] = {0};
    int dist[MAX_V][MAX_V];

    printf("All-Pairs Shortest Path using Floyed-Warshall Algorithm\n");
    printf("---------------------------------------------\n");

    // input num of vertices and edges
    printf("Enter the num of vertices: ");
    scanf("%d", &V);
    if (V > MAX_V)
    {
        printf("Maximum allowed vertices are %d.\n", MAX_V);
        return 1;
    }

    printf("Enter the num of edges: ");
    scanf("%d", &E);

    // input edges
    printf("Enter each edge in the format 'source destination weight':\n");
    printf("(Vertices should be numbered from 0 to %d)\n", V - 1);
    for (int i = 0; i < E; i++)
    {
        int src, dest, weight;
        scanf("%d %d %d", &src, &dest, &weight);
        if (src < 0 || src >= V || dest < 0 || dest >= V)
        {
            printf("Invalid vertex number. Please enter vertices between 0 and %d.\n", V - 1);
            i--; // repeat this iteration
            continue;
        }
        g[src][dest] = weight;
        g[dest][src] = weight;
    }

    // initialize the distance matrix
    initMatrix(g, dist, V);

    // run the Floyd-Warshall algorithm
    floydWarshall(dist, V);

    // detect negative cycle
    if (detectNegativeCycle(dist, V))
    {
        printf("Graph contains a negative weight cycle. Shortest paths not defined.\n");
        return 1;
    }

    printDistMatrix(dist, V);
    return 0;
}
```



#### Program Output

**Initialization:**

The adjacency matrix after input:

|      | 0    | 1    | 2    | 3    |
| ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 5    | 0    | 10   |
| 1    | 0    | 0    | 3    | 0    |
| 2    | 0    | 0    | 0    | 1    |
| 3    | 0    | 0    | 0    | 0    |

**Floyd-Warshall Iterations:**

- Intermediate Vertex k = 0:
  - Update paths using vertex 0 as intermediate.
  - Paths updated:
    - `dist[1][3] = min(INF, dist[1][0] + dist[0][3]) = min(INF, 0 + 10) = 10`
    - `dist[2][1] = min(INF, dist[2][0] + dist[0][1]) = INF` (no change)
    - `dist[2][3] = min(1, dist[2][0] + dist[0][3]) = 1` (no change)
    - `dist[3][1] = min(INF, dist[3][0] + dist[0][1]) = INF` (no change)
- Intermediate Vertex k = 1:
  - Update paths using vertex 1 as intermediate.
  - Paths updated:
    - `dist[0][2] = min(INF, dist[0][1] + dist[1][2]) = min(INF, 5 + 3) = 8`
    - `dist[0][3] = min(10, dist[0][1] + dist[1][3]) = min(10, 5 + 10) = 10` (no change)
    - `dist[2][3] = min(1, dist[2][1] + dist[1][3]) = min(1, INF + 10) = 1` (no change)
    - `dist[3][2] = min(INF, dist[3][1] + dist[1][2]) = INF` (no change)
- Intermediate Vertex k = 2:
  - Update paths using vertex 2 as intermediate.
  - Paths updated:
    - `dist[0][3] = min(10, dist[0][2] + dist[2][3]) = min(10, 8 + 1) = 9`
    - `dist[1][3] = min(10, dist[1][2] + dist[2][3]) = min(10, 3 + 1) = 4`
    - `dist[3][3] = min(0, dist[3][2] + dist[2][3]) = min(0, INF + 1) = 0` (no change)
- Intermediate Vertex k = 3:
  - Update paths using vertex 3 as intermediate.
  - No updates needed as all paths through vertex 3 are not shorter.

**Final Distance Matrix:**

|      | 0    | 1    | 2    | 3    |
| ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 5    | 8    | 9    |
| 1    | 0    | 0    | 3    | 4    |
| 2    | 0    | 0    | 0    | 1    |
| 3    | 0    | 0    | 0    | 0    |

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==27482== Memcheck, a memory error detector
==27482== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==27482== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==27482== Command: ./final
==27482== 
All-Pairs Shortest Path using Floyed-Warshall Algorithm
---------------------------------------------
Enter the num of vertices: 4
Enter the num of edges: 4
Enter each edge in the format 'source destination weight':
(Vertices should be numbered from 0 to 3)
0 1 5
0 3 10
1 2 3
2 3 1

Shortest distances between every pair of vertices:
       0    1    2    3 
   0    0    5    8    9 
   1  INF    0    3    4 
   2  INF  INF    0    1 
   3  INF  INF  INF    0 
==27482== 
==27482== HEAP SUMMARY:
==27482==     in use at exit: 0 bytes in 0 blocks
==27482==   total heap usage: 2 allocs, 2 frees, 2,048 bytes allocated
==27482== 
==27482== All heap blocks were freed -- no leaks are possible
==27482== 
==27482== For lists of detected and suppressed errors, rerun with: -s
==27482== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

#### Program Output #2

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==36362== Memcheck, a memory error detector
==36362== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==36362== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==36362== Command: ./final
==36362== 
All-Pairs Shortest Path using Floyed-Warshall Algorithm
---------------------------------------------
Enter the num of vertices: 4
Enter the num of edges: 5
Enter each edge in the format 'source destination weight':
(Vertices should be numbered from 0 to 3)
0 1 1
0 2 4
1 2 -3
1 3 2
2 3 3

Shortest distances between every pair of vertices:
       0    1    2    3 
   0    0    1   -2    1 
   1  INF    0   -3    0 
   2  INF  INF    0    3 
   3  INF  INF  INF    0 
==36362== 
==36362== HEAP SUMMARY:
==36362==     in use at exit: 0 bytes in 0 blocks
==36362==   total heap usage: 2 allocs, 2 frees, 2,048 bytes allocated
==36362== 
==36362== All heap blocks were freed -- no leaks are possible
==36362== 
==36362== For lists of detected and suppressed errors, rerun with: -s
==36362== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

## Time and Space Complexity

### **Time Complexity:**

- The Floyd-Warshall algorithm involves three nested loops, each iterating over the set of vertices.
- **Overall Time Complexity:** O(V<sup>3</sup>), where V is the number of vertices.

### **Space Complexity:**

- The algorithm utilizes two `V × V` matrices: one for the input graph and another for storing the shortest distances.
- **Overall Space Complexity:** O(V<sup>2</sup>)
