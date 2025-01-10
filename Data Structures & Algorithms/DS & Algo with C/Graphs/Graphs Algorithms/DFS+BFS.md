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

#### Outputing with 'd' (Tree)

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
```

`hello.c`

```C
```

`main.c`

```c
```

