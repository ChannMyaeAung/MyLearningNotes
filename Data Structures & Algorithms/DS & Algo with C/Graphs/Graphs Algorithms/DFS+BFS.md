# Implementation of BFS and DFS

## Implementation #1 

- Here is a simple implementation of depth-first search, using a recursive algorithm and breadth-first  search, using an iterative algorithm that maintains a queue of vertices.
- In both cases, the algorithm is applied to a sample graph whose vertices are the integers 0 through n - 1 for some n, and in which vertex x has edges to vertices x / 2, 3 * x, and x + 1, whenever these values are also integers in the range 0 through n - 1.
- For large graphs, it may be safer to run an iterative version of DFS that uses an explicit stack instead of a possibly ver deep recursion.

#### Program Code

`hello.h`

```c
typedef int Vertex;

#define VERTEX_NULL (-1)

// neighbors: an array of vertices that the current vertex points to (outgoing edges).
// parent: tracks the parent vertex in search trees (used in DFS and BFS)
struct node
{
    Vertex *neighbors; // array of outgoing edges
    Vertex parent;     // for search
};

struct graph
{
    size_t n;       // num of vertices
    struct node *v; // list of vertices
};
void graphDestroy(struct graph *g);
struct graph *makeSampleGraph(size_t n);
void printGraph(const struct graph *g);
void printPath(const struct graph *g, Vertex u);
void printTree(const struct graph *g);
void dfs(struct graph *g, Vertex root);
void bfs(struct graph *g, Vertex root);
```

`hello.c`

```C
void graphDestroy(struct graph *g)
{
    size_t v;
    // iterate thru each vertex and free its neighbors array
    for (v = 0; v < g->n; v++)
    {
        free(g->v[v].neighbors);
    }
    
    // free the array of vertices
    free(g->v);
    
    // free the graph struct itself
    free(g);
}

// constructs a sample graph with n vertices based on predefined rules.
// this graph has edges from x to x + 1, x to 3 * x, and x to x / 2 (when x is even)
struct graph *makeSampleGraph(size_t n)
{
    struct graph *g;
    size_t v;
    const int allocNeighbors = 4; // allocates a fixed-size array for storing neighbors.
    int i;
	
    
    // allocates memory for the graph struct
    g = malloc(sizeof(*g));
    assert(g);

    g->n = n;
    
    // allocates memory for the array of struct node based on the num of vertices n.
    g->v = malloc(sizeof(struct node) * n);
    assert(g->v);

    // for each vertex v,
    for (v = 0; v < n; v++)
    {
        // initialze its parent to VERTEX_NULL (indicating no parent)
        g->v[v].parent = VERTEX_NULL;

        // fill in neighbors
        g->v[v].neighbors = malloc(sizeof(Vertex) * allocNeighbors);
        i = 0;
        // edging rules
        if (v % 2 == 0) // even vertex
        {
            // if v is even, adds an edge to v / 2.
            g->v[v].neighbors[i++] = v / 2;
        }
        if (3 * v < n) // tri-multiplication
        {
            g->v[v].neighbors[i++] = 3 * v;
        }
        if (v + 1 < n) // increment
        {
            g->v[v].neighbors[i++] = v + 1;
        }
        
        // adds a sentinel value VERTEX_NULL (-1) to indicate the end of neighbors list.
        g->v[v].neighbors[i++] = VERTEX_NULL;
    }
    return g;
}

// output graph in dot format
void printGraph(const struct graph *g)
{
    size_t u;
    size_t i;

    puts("digraph G {");

    for (u = 0; u < g->n; u++)
    {
        for (i = 0; g->v[u].neighbors[i] != VERTEX_NULL; i++)
        {
            printf("%zu -> %d;\n", u, g->v[u].neighbors[i]);
        }
    }
    puts("}");
}

// reconstruct path back to root from u
void printPath(const struct graph *g, Vertex u)
{
    do
    {
        // prints the current vertex
        printf(" %d", u);
        // moves to its parent vertex
        u = g->v[u].parent;
    } while (g->v[u].parent != u); // continues until it reaches the root vertex(where a vertex is its own parent)
}

// print the tree in dot format
void printTree(const struct graph *g)
{
    size_t u;
    puts("digraph G {");

    for (u = 0; u < g->n; u++)
    {
        // if a vertex's parent is not itself, it implies that it's part of the search tree
        if (g->v[u].parent != (Vertex)u)
        {
            // prints an edge from u to its parent
            printf("%zu -> %d;\n", u, g->v[u].parent);
        }
    }
    puts("}");
}

// compute DFS tree starting at root
// this uses a recursive algorithm and will not work for large graphs!
// g: pointer to the graph, parent: the vertex from which child was discovered, child: the current vertex being explored.
void dfsHelper(struct graph *g, Vertex parent, Vertex child)
{
    int i;
    Vertex neighbor;
	
    // visit unvisited vertex
    // checks if child has not been visited (parent == VERTEX_NULL)
    if (g->v[child].parent == VERTEX_NULL)
    {
        // sets child's parent to parent marking it as visited
        g->v[child].parent = parent;
        
        // iterates thru all neighbors of child
        for (i = 0; (neighbor = g->v[child].neighbors[i]) != VERTEX_NULL; i++)
        {
            // recursively calls dfsHelper on each unvisited neighbor
            dfsHelper(g, child, neighbor);
        }
    }
}

void dfs(struct graph *g, Vertex root)
{
    // calls dfshelper with both parent and child set to root, effectively marking the root as its own parent and starting the traversal.
    dfsHelper(g, root, root);
}

void bfs(struct graph *g, Vertex root)
{
    Vertex *q;
    int head; // deq from here
    int tail; // enq from here
    Vertex current;
    Vertex nbr;
    int i;
	
    
    // allocates memory for a queue to manage the order of vertex exploration
    q = malloc(sizeof(Vertex) * g->n);
    assert(q);

    head = tail = 0;

    // push root onto q
    g->v[root].parent = root;
    q[tail++] = root;

    // continues until the queue is empty
    while (head < tail)
    {
        // dequeus the next vertex current from the front of the queue.
        current = q[head++];
	
        
        // iterates through all neighbors of current
        for (i = 0; (nbr = g->v[current].neighbors[i]) != VERTEX_NULL; i++)
        {
            // if a neighbor hasn't been visited, marks its parent and enqueues it.
            if (g->v[nbr].parent == VERTEX_NULL)
            {
                // haven't seen this vertex yet
                // push it
                g->v[nbr].parent = current;
                q[tail++] = nbr;
            }
        }
    }
    free(q);
}
```

`main.c`

```C
int main(int argc, char **argv)
{
    int n;
    struct graph *g;

    if (argc != 3)
    {
        fprintf(stderr, "Usage: %s action \nwhere action =\n g - print graph\n d - print", argv[0]);
        return 1;
    }

    n = atoi(argv[2]);
    g = makeSampleGraph(n);

    switch (argv[1][0])
    {
    case 'g':
        printGraph(g);
        break;
    case 'd':
        dfs(g, 0);
        printTree(g);
        break;
    default:
        fprintf(stderr, "%s: unknown action '%c'\n", argv[0], argv[1][0]);
        return 1;
    }

    graphDestroy(g);
    return 0;
}
```

#### Program Output

Creating a graph with `n = 4` vertices.

| Vertex(v) | Conditions Met                                               | Neighbors Added                                              |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 0         | Even (0 % 2 == 0): Add 0 / 2 = 0.<br/> 3 * 0 = 0 < 4: Adds 0. <br/> 0 + 1 = 1 < 4: Adds 1 | [0, 0, 1, -1] (-1 is sentinel value added to indicate the end of the neighbors list). |
| 1         | 1 is not even. <br/> 3 * 1 = 3 < 4: Adds 3. <br/> 1 + 1 = 2 < 4: Adds 2 | [3, 2, -1]                                                   |
| 2         | 2 % 2 == 0: Adds 2/ 2 = 1. <br/> 3 * 2 = 6 >=4: No edge <br/> 2 + 1 = 3 < 4: Adds 3. | [1, 3, -1]                                                   |
| 3         | 3 is not even. <br/> 3 * 3 = 9 >= 4: No Edge. <br/> 3 + 1 = 4 >= 4: No Edge | [-1]                                                         |

```css
0 → 0 (self-loop)
0 → 0 (another self-loop)
0 → 1
1 → 3
1 → 2
2 → 1
2 → 3
3 (No outgoing edges)
```



```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final g 4
==13065== Memcheck, a memory error detector
==13065== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==13065== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==13065== Command: ./final g 4
==13065== 
digraph G {
0 -> 0;
0 -> 0;
0 -> 1;
1 -> 3;
1 -> 2;
2 -> 1;
2 -> 3;
}
==13065== 
==13065== HEAP SUMMARY:
==13065==     in use at exit: 0 bytes in 0 blocks
==13065==   total heap usage: 7 allocs, 7 frees, 1,168 bytes allocated
==13065== 
==13065== All heap blocks were freed -- no leaks are possible
==13065== 
==13065== For lists of detected and suppressed errors, rerun with: -s
==13065== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

- **Graph Structure:**
  - Vertex `0` has two self-loops and one edge to `1`.
  - Vertex `1` has edges to `3` and `2`.
  - Vertex `2` has edges to `1` and `3`.
  - Vertex `3` has no outgoing edges (except potential self-loop if added).

#### Outputing with 'd' (DFS + Tree)

**Depth-First Search (DFS) Traversal (`dfs`)**

**Purpose:** Traverses the graph starting from the root vertex (`0`) and builds a DFS tree by recording parent-child relationships.

**Process:**

1. **Initialization:**
   - Start at **Vertex 0**.
   - Mark **Vertex 0** as visited (`parent[0] = 0`).
2. **Exploration from Vertex 0:**
   - **Neighbors:** `0`, `0`, `1`.
   - **First Neighbor (`0`):** Already visited (self-loop), skip.
   - **Second Neighbor (`0`):** Already visited (self-loop), skip.
   - **Third Neighbor (`1`):** Not visited, proceed to visit **Vertex 1**.
3. **Exploration from Vertex 1:**
   - Mark **Vertex 1** as visited (`parent[1] = 0`).
   - **Neighbors:** `3`, `2`.
   - **First Neighbor (`3`):** Not visited, proceed to visit **Vertex 3**.
   - **Second Neighbor (`2`):** Not visited, proceed to visit **Vertex 2**.
4. **Exploration from Vertex 3:**
   - Mark **Vertex 3** as visited (`parent[3] = 1`).
   - **Neighbors:** *(None)*.
   - Backtrack to **Vertex 1**.
5. **Exploration from Vertex 2:**
   - Mark **Vertex 2** as visited (`parent[2] = 1`).
   - **Neighbors:** `1`, `3`.
   - **First Neighbor (`1`):** Already visited, skip.
   - **Second Neighbor (`3`):** Already visited, skip.
   - Backtrack to **Vertex 1**, then to **Vertex 0**.

**Final Parent Relationships**:

| Vertex | Parent |
| ------ | ------ |
| 0      | 0      |
| 1      | 0      |
| 2      | 1      |
| 3      | 1      |



```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final d 4
==19638== Memcheck, a memory error detector
==19638== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==19638== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==19638== Command: ./final d 4
==19638== 
digraph G {
1 -> 0;
2 -> 1;
3 -> 1;
}
==19638== 
==19638== HEAP SUMMARY:
==19638==     in use at exit: 0 bytes in 0 blocks
==19638==   total heap usage: 7 allocs, 7 frees, 1,168 bytes allocated
==19638== 
==19638== All heap blocks were freed -- no leaks are possible
==19638== 
==19638== For lists of detected and suppressed errors, rerun with: -s
==19638== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

1. **`1 -> 0;`**
   - **Vertex 1** was **discovered by Vertex 0** during the DFS traversal.
   - This means **Vertex 0** is the **parent** of **Vertex 1** in the DFS tree.
2. **`2 -> 1;`**
   - **Vertex 2** was **discovered by Vertex 1**.
   - **Vertex 1** is the **parent** of **Vertex 2**.
3. **`3 -> 1;`**
   - **Vertex 3** was **discovered by Vertex 1**.
   - **Vertex 1** is the **parent** of **Vertex 3**.

```css
0
|
1
/ \
2  3
```

- **Vertex 0** is the **root**.
- **Vertex 1** is a child of **Vertex 0**.
- **Vertices 2 and 3** are children of **Vertex 1**.

---

## Combined Implementation #2

`hello.h`

```C
#define MAX 100

extern int graph[MAX][MAX]; // adjacency matrix representation of the graph
extern int visited[MAX];    // visited array for traversals
extern int n;               // num of nodes in the graph

void dfs(int v);
void bfs(int start);
```

`hello.c`

```C

void dfs(int v)
{
    // mark the current node as visited and print the node
    visited[v] = 1;
    printf("%d ", v);
	
    for (int i = 0; i < n; i++)
    {
        // if there's an edge and the node is unvisited
        if (graph[v][i] == 1 && !visited[i])
        {
            // recursively visit the node
            dfs(i);
        }
    }
}
void bfs(int start)
{
    int queue[MAX];
    int front = 0, rear = 0;

    // initialize visited array for BFS
    for (int i = 0; i < n; i++)
    {
        visited[i] = 0;
    }

    // mark the starting node as visited
    visited[start] = 1;
    
    // enqueue the starting node
    queue[rear++] = start;

    while (front < rear)
    {
        int v = queue[front++];
        printf("%d ", v);

        for (int i = 0; i < n; i++)
        {
            if (graph[v][i] == 1 && !visited[i])
            {
                visited[i] = 1;
                queue[rear++] = i;
            }
        }
    }
}
```

`main.c`

```c
#include <stdlib.h>
#include <stdio.h>
#include "hello.h"

int main()
{
    int edges, u, v;

    printf("Enter the number of nodes: ");
    scanf("%d", &n);

    printf("Enter the number of edges: ");
    scanf("%d", &edges);

    // Initialize adjacency matrix with zeros
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            graph[i][j] = 0;
        }
    }

    // Define each edge
    printf("Enter each edge in the format 'u v' (0-indexed):\n");
    for (int i = 0; i < edges; i++)
    {
        scanf("%d %d", &u, &v);
        graph[u][v] = 1;
        graph[v][u] = 1; // since the graph is undirected
    }

    // perform DFS starting from vertex 0
    printf("DFS traversal starting from vertex 0: ");
    for (int i = 0; i < n; i++)
    {
        visited[i] = 0;
    }
    dfs(0);
    printf("\n");

    // Perform BFS starting from vertex 0
    printf("BFS traversal starting from vertex 0: ");
    bfs(0);
    printf("\n");
    return 0;
}
```

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==20626== Memcheck, a memory error detector
==20626== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==20626== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==20626== Command: ./final
==20626== 
Enter the number of nodes: 5
Enter the number of edges: 5
Enter each edge in the format 'u v' (0-indexed):
0 1
0 2
1 2
1 3
2 4
DFS traversal starting from vertex 0: 0 1 2 4 3 
BFS traversal starting from vertex 0: 0 1 2 3 4 
==20626== 
==20626== HEAP SUMMARY:
==20626==     in use at exit: 0 bytes in 0 blocks
==20626==   total heap usage: 2 allocs, 2 frees, 2,048 bytes allocated
==20626== 
==20626== All heap blocks were freed -- no leaks are possible
==20626== 
==20626== For lists of detected and suppressed errors, rerun with: -s
==20626== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

```css
    0
   / \
  1---2
  |    \
  3     4

```

**Possible DFS Order** (depending on neighbor iteration order):
Starting from 0, neighbors are 1 and 2.

- Visit 0.
- Go to 1 (neighbor of 0).
- From 1, neighbors are 0 (visited), 2, 3.
  - Skip 0 as it's visited, go to 2.
  - From 2, neighbors are 0 (visited), 1 (visited), 4.
    - Skip 0, 1, and go to 4.
    - 4's neighbor 2 is visited, so backtrack.
  - Back to 1, next neighbor is 3.
    - Visit 3, then backtrack.
- Back to 0, next neighbor is 2, but 2 is already visited.

**BFS Traversal** starting from vertex 0:

- The BFS algorithm will explore neighbors level by level.
- **BFS Order:**
  - Start at 0: enqueued neighbors 1, 2.
  - Dequeue 0, then enqueue its neighbors (1,2).
  - Dequeue 1: enqueue neighbor 3 (since 2 is already enqueued or visited).
  - Dequeue 2: enqueue neighbor 4 (since 0,1 already visited or enqueued).
  - Dequeue 3: no new neighbors.
  - Dequeue 4: no new neighbors.
