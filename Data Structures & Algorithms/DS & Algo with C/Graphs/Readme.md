# Graphs

- A graph consists of a set of **nodes** or **vertices** together with a set of **edges** or **arcs** where each edge joins two vertices.
- A node also known as a vertex may contains some pieces of data. 
- An edge is a connection between two nodes.

```
A       ----->      B
↑          ↑        ↑
node      edges     node
or			or		 or
vertex   arcs	     vertex
```



## Types of Graphs

### 1. Undirected Graph

- Unless otherwise specified, a graph is **undirected**.
  - Edges do not have a direction. If there is an edge (u, v), we can go from u to v and from v to u.
  - Each edge is an unordered pair {u, v} of vertices and we don't regard either of  the two vertices as having a distinct role from the other.

```css
[Larry]------ [Patrick] 
   |         /    |
   |       /      |
   |      /       |
   [Sandy]----[SpongeBob]----[Squidward]
```

- An example of an undirected graph can be a social media, "Facebook".
- Each node can represent a user. If a user is friends with another user, we can establish a friendship (an edge, a connection between the two nodes (the two users in this case)).
- If two nodes are connected, they have what is known as "adjacency".
- In the above example, Larry is friends with Patrick and Sandy, so Larry has adjacency with Patrick and Sandy. Patrick is friends with Larry, Sandy, Spongebob. Spongebob is friends with Sandy, Patrick and Squidward. Squidward is adjacent to only one neighbor, Spongebob.

### 2. Directed Graph (Digraph)

- It's more common, however, in computing to consider **directed graphs** or **digraphs** in which edges are ordered pairs (u, v). Here the vertex u is the **source** of the edge and vertex v is the **sink** or **target** of the edge.
- Directed edges are usually drawn as arrows and undirected edges as curves or line segments.

```css
    A  ----->   B
    ↑        ↙  |
    |	  ↙     |
    |   ↙       ↓
     E ⇄⇄⇄⇄⇄⇄⇄  C ----> D
```

- In this example, node A have an adjacency to node B. But not the otherway around.
- However, it is valid to have more than a node pointing to another node, and that node could point back to the previous node like we have in the above example, node E and node C.
- An example could be a street-map. Let's say we are working on a travel app. Each node is a possible destination. The single edges could be one way streets. The double edges could be two way streets (like node E and node C above). 

### 3. Weighted or Unweighted

- In weighted graphs, edges carry weights or costs, whereas unweighted graphs have edges without associated weights.

### 4. Cyclic or Acyclic

- A graph with cycles (paths that start and end at the same vertex) is cyclic; otherwise, it's acyclic.

![Screenshot from 2025-01-01 21-54-12](/home/chan/Pictures/Screenshots/Screenshot from 2025-01-01 21-54-12.png)

![Screenshot from 2025-01-01 21-54-17](/home/chan/Pictures/Screenshots/Screenshot from 2025-01-01 21-54-17.png)



## Key Terminologies of Graph

### 1. In-Degree

**Definition:** The **in-degree** of a vertex in a **directed graph** is the number of edges **incoming** to that vertex. In other words, it counts how many edges point **toward** the vertex.

**Applicability:**

- **Directed Graphs (Digraphs):** In-degree is relevant.
- **Undirected Graphs:** In-degree is equivalent to the degree of the vertex.

**Example:** Consider a directed graph with vertices **A**, **B**, and **C**:

```css
A → B
C → B
```

- **In-Degree of B:** 2 (edges from A and C)
- **In-Degree of A:** 0
- **In-Degree of C:** 0

**Visualization:**

```css
A → B ← C
```

------

### 2. Out-Degree

**Definition:** The **out-degree** of a vertex in a **directed graph** is the number of edges **outgoing** from that vertex. It counts how many edges originate **from** the vertex.

**Applicability:**

- **Directed Graphs (Digraphs):** Out-degree is relevant.
- **Undirected Graphs:** Out-degree is equivalent to the degree of the vertex.

**Example:** Using the same directed graph:

```css
A → B
C → B
```

- **Out-Degree of A:** 1 (edge to B)
- **Out-Degree of B:** 0
- **Out-Degree of C:** 1 (edge to B)

**Visualization:**

```css
A → B ← C
```

------

### 3. Adjacency

**Definition:** Two vertices are **adjacent** if there is an **edge** connecting them. In other words, they are **neighbors** in the graph.

**Types of Adjacency:**

- **Directed Graphs:** Adjacency can be **outward** or **inward** based on the direction of the edge.
- **Undirected Graphs:** Adjacency is **bidirectional** since edges have no direction.

**Example:** Consider an undirected graph:

```css
A — B — C
```

- Adjacency:
  - A is adjacent to B.
  - B is adjacent to A and C.
  - C is adjacent to B.

**Visualization:**

```css
A — B — C
```

------

### 4. Path

**Definition:** A **path** in a graph is a sequence of **vertices** where each consecutive pair is connected by an **edge**. Paths can be:

- **Simple Path:** No repeated vertices.
- **Closed Path (Cycle):** Starts and ends at the same vertex.

**Applicability:**

- **Directed Graphs:** Paths must follow the direction of edges.
- **Undirected Graphs:** Paths can traverse edges in any direction.

**Example:** In an undirected graph:

```css
A — B — C — D
```

- **Path from A to D:** A → B → C → D
- **Simple Path:** A → B → C → D
- **Closed Path (Cycle):** A → B → C → A (if the edge A—C exists)

**Visualization:**

```css
A — B — C — D
```

------

### 5. Cycle

**Definition:** A **cycle** is a **closed path** where the first and last vertices are the same, and **no other vertices are repeated**. In other words, it's a path that starts and ends at the same vertex without traversing any edge more than once.

**Types of Cycles:**

- **Directed Cycle:** All edges follow the direction consistently.
- **Undirected Cycle:** Edges have no direction, forming a loop.

**Example:** In a directed graph:

```css
A → B → C → A
```

- **Cycle:** A → B → C → A

In an undirected graph:

```css
A — B
|   |
D — C
```

- **Cycle:** A — B — C — D — A

**Visualization:**

*Directed Cycle:*

```css
A → B
↑   ↓
C ← D
```

*Undirected Cycle:*

```css
A — B
|   |
D — C
```

------

### 6. Connectedness

**Definition:** **Connectedness** describes how **vertices** are connected within a graph.

- **Connected Graph:** In an **undirected graph**, every pair of vertices is connected by at least one path.
- **Disconnected Graph:** There exists at least one pair of vertices with no connecting path.
- **Strongly Connected (Directed Graph):** For every pair of vertices **u** and **v**, there is a path from **u** to **v** and a path from **v** to **u**.
- **Weakly Connected (Directed Graph):** The underlying undirected graph is connected, but directed paths may not exist in both directions.

**Examples:**

*Undirected Connected Graph:*

```css
A — B — C
    |
    D
```

- All vertices are connected via paths.

*Undirected Disconnected Graph:*

```css
A — B    C — D
```

- No path between {A, B} and {C, D}.

*Directed Strongly Connected Graph:*

```css
A  → B  → C
↑         |
|_________|
```

- Paths exist between all pairs in both directions.

*Directed Weakly Connected Graph:*

```css
A → B    C
```

- The underlying undirected graph is connected if ignoring edge directions, but no path from C to others.

**Visualization:**

*Connected Undirected Graph:*

```css
A — B — C
    |
    D
```

*Disconnected Undirected Graph:*

```css
A — B    C — D
```

*Strongly Connected Directed Graph:*

```css
A  → B  → C
↑         |
|_________|
```

*Weakly Connected Directed Graph:*

```css
A → B    C
```

### Summary of Graph's Key Terminology

| Term              | Directed Graphs              | Undirected Graphs         | Description                                                  |
| ----------------- | ---------------------------- | ------------------------- | ------------------------------------------------------------ |
| **In-Degree**     | Yes                          | Equivalent to degree      | Number of incoming edges to a vertex.                        |
| **Out-Degree**    | Yes                          | Equivalent to degree      | Number of outgoing edges from a vertex.                      |
| **Adjacency**     | Direction matters            | Bidirectional             | Whether two vertices are connected by an edge.               |
| **Path**          | Follows edge direction       | Any direction             | Sequence of vertices connected by edges, respecting direction in directed graphs. |
| **Cycle**         | Direction consistent         | No direction              | A closed path with no repeated vertices except the starting and ending vertex. |
| **Connectedness** | Strongly or Weakly Connected | Connected or Disconnected | How vertices are interconnected within the graph.            |



## Why graphs are useful

- Graphs can be used to model any situation where we have things that are related to each other in pairs. 
- All of the following can be represented by graphs:
  - **Family trees**: Nodes are members, with an edge from each parent to each of their children.
  - **Transportation networks**: Nodes are airports, intersections, ports etc. Edges are airline flights, one-way roads, shipping routes etc.
  - **Assignments**: Suppose we are assigning classes to classrooms. Let each node be either a class or a classroom, and put an edge from a class to a classroom if the class is assigned to that room. This is an example of a **bipartite graph**, where the nodes can be divided into two sets S and T and all edges go from S to T.

- Graphs are used in various applications, such as social networks, transportation systems, and network routing.



## Operations on graphs

- What would we like to do to graphs?
- Generally, we first have to build a graph by starting with a set of nodes and adding in any edges we need, and then we want to extract information from it, such as "Is this graph connected?", "What is the shortest path in this graph from s to t?", or "How many edges can I remove from this graph before some nodes become unreachable from other nodes?"
- There are standard algorithms for answering all those questions.
- The information these algorithms need is typically:
  - given a vertex u, what successors does it have
  - and sometimes, given vertices u and v, does the edge (u, v) exist in the graph?

## Graph Representations in C

There are two primary ways to represent graphs in C:

1. **Adjacency Matrix**
2. **Adjacency List**

### 1. Adjacency Matrix

- An adjacency matrix is a 2D array where each cell `(i, j)` indicates whether there's an edge from vertex `i` to vertex `j`. 
- An adjacency matrix for a graph of N vertices is an N * N matrix M.
- For an undirected graph, if there is an edge between vertex `i` and vertex `j`, we mark `M[i][j] = 1` (or the weight if it's weighted) and `M[j][i] = 1`.
- For a directed graph, `M[i][j] = 1` does not necessarily imply `M[j][i] = 1`.
- For weighted graphs, the cell can store the weight instead of a simple boolean.

```css
    A  ----->   B
    ↑        ↙  |
    |	  ↙     |
    |   ↙       ↓
     E ⇄⇄⇄⇄⇄⇄⇄  C ----> D
```

**Adjacency Matrix for the above graph**:

|      | A    | B    | C    | D    | E    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| A    | 0    | 1    | 0    | 0    | 0    |
| B    | 0    | 0    | 1    | 0    | 1    |
| C    | 0    | 0    | 0    | 1    | 1    |
| D    | 0    | 0    | 0    | 0    | 0    |
| E    | 1    | 0    | 1    | 0    | 0    |

- **Rows and Columns:** Each row represents a source vertex, and each column represents a destination vertex.
- Values:
  - **`1`** indicates the presence of an edge between the vertices.
  - **`0`** indicates no direct edge.
- For example, if we take a look at the first row, row A column B, there is an adjacency from node A to B.  But if we take a look at row A column C, this is 0. so there is no adjacency between node A and C. If there is, then we will replace the 0 with 1.

**Pros:**

- Simple and straightforward.
- Checking the existence of an edge is O(1).

**Cons:**

- Consumes O(V²) space, which can be inefficient for sparse graphs.
- Iterating over all neighbors of a vertex takes O(V) time.



#### **Implementation Example:**

- `adjMatrix`: A 2D array representing the adjacency matrix. If `adjMatrix[i][j]` is `1`, there is an edge between vertex `i` and vertex `j`; otherwise, there is no edge.

```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_VERTICES 100

typedef struct Graph {
    int numVertices;
    int adjMatrix[MAX_VERTICES][MAX_VERTICES];
} Graph;

// Initialize the graph with a specified number of vertices and sets all edges to 0 (no edges)
void initGraph(Graph *g, int vertices) {
    g->numVertices = vertices;
    for(int i = 0; i < vertices; i++){
        for(int j = 0; j < vertices; j++){
            g->adjMatrix[i][j] = 0;
        }
    }         
}

// Add an edge from src vertex to dest vertex.
// directed: flag indicating whether the edge is directed (1) or undirected (0)
void addEdge(Graph *g, int src, int dest, int directed) {
    
    // validates src and dest are within the valid range
    if(src >= g->numVertices || dest >= g->numVertices || src < 0 || dest < 0)
    {
        printf("Invalid vertex number.\n");
        return;
    }
    
    // sets to 1 to indicate an edge
    g->adjMatrix[src][dest] = 1;
    
    // if directed is 0, the edge is bidirectional
    if(!directed)
        g->adjMatrix[dest][src] = 1;
}

// Print the adjacency matrix of the graph
void printGraph(Graph *g)
{
    printf("Adjacency Matrix:\n   ");
    for(int i = 0; i < g->numVertices; i++)
    {
        printf("%d ", i);
    }
    printf("\n");

    for(int i = 0; i < g->numVertices; i++)
    {
        printf("%d: ", i);
        for(int j = 0; j < g->numVertices; j++)
        {
            printf("%d ", g->adjMatrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    Graph g;
    initGraph(&g, 5);
    addEdge(&g, 0, 1, 0);
    addEdge(&g, 0, 4, 0);
    addEdge(&g, 1, 2, 0);
    addEdge(&g, 1, 3, 0);
    addEdge(&g, 1, 4, 0);
    addEdge(&g, 2, 3, 0);
    addEdge(&g, 3, 4, 0);

    printGraph(&g);
    return 0;
}
```

**Output:**

```shell
chan@CMA:~/C_Programming/test$ ./final
Adjacency Matrix:
   0 1 2 3 4 
0: 0 1 0 0 1 
1: 1 0 1 1 1 
2: 0 1 0 1 0 
3: 0 1 1 0 1 
4: 1 1 0 1 0 

```

- **Vertex 0**: Connected to **1** and **4**.
- **Vertex 1**: Connected to **0**, **2**, **3**, and **4**.
- **Vertex 2**: Connected to **1** and **3**.
- **Vertex 3**: Connected to **1**, **2**, and **4**.
- **Vertex 4**: Connected to **0**, **1**, and **3**.

**Graph Diagram**

```css
      0
     / \
    1---4
   /|   |
  2 |   |
   \|   |
    3---|
```

- Since all edges are undirected (`directed = 0`), the connections are bidirectional.

### 2. Adjacency List

An adjacency list represents a graph as an array of lists. The array index represents each vertex, and each list contains the adjacent vertices (and possibly weights).

- These lists may be represented as linked lists or in languages like C may be represented by variable-length arrays.
- Adjacency lists are more commonly used than adjacency matrices.

**Pros:**

- Efficient for sparse graphs, using O(V + E) space.
- Iterating over neighbors is faster since it only traverses existing edges.

**Cons:**

- Checking for the existence of a specific edge can take O(V) time in the worst case.

#### **Implementation Example #1:**

```c
#include <stdio.h>
#include <stdlib.h>

// Structure to represent an adjacency list node
typedef struct AdjListNode {
    int dest;
    struct AdjListNode* next;
} AdjListNode;

// Structure to represent an adjacency list
typedef struct AdjList {
    AdjListNode* head; // pointer to the first node in the adjacency list
} AdjList;

// Structure to represent the graph
typedef struct Graph {
    int numVertices;
    AdjList* array; // array of adjacency lists
} Graph;

// Function to create a new adjacency list node with the specified destination vertex
AdjListNode* newAdjListNode(int dest) {
    AdjListNode* newNode = malloc(sizeof(AdjListNode));
    if(!newNode){
        fprintf(stderr, "Memory allocation failed\n");
        exit(1);
    }
    newNode->dest = dest;
    newNode->next = NULL;
    return newNode;
}

// Initialize the graph with the given number of vertices
Graph* createGraph(int vertices) {
    // allocate memory for the Graph structure
    Graph* graph = malloc(sizeof(Graph));
    if(!graph){
        printf("Memory allocation error\n");
        exit(1);
    }
    
    // set te num of Vertices
    graph->numVertices = vertices;

    // Create an array of adjacency lists
    graph->array = malloc(vertices * sizeof(AdjList));
    if(!graph->array){
        printf("Memory allocation error\n");
        free(graph);
        exit(1);
    }
    
    // Initialize each adjacency list as empty
    for(int i = 0; i < vertices; i++)
        graph->array[i].head = NULL;

    return graph;
}

// Add edge to the graph
void addEdge(Graph* graph, int src, int dest, int directed) {
    // Add an edge from src to dest
    AdjListNode* newNode = newAdjListNode(dest);
    
    // insert at the beginning of the src vertex's adjacency list
    newNode->next = graph->array[src].head;
    graph->array[src].head = newNode;

    // If undirected, also add an edge from dest to src
    if(!directed) {
        newNode = newAdjListNode(src);
        newNode->next = graph->array[dest].head;
        graph->array[dest].head = newNode;
    }
}

// Print the adjacency list representation
void printGraph(Graph* graph) {
    for(int v = 0; v < graph->numVertices; v++) {
        AdjListNode* temp = graph->array[v].head;
        printf("Vertex %d:", v);
        while(temp) {
            printf(" -> %d", temp->dest);
            temp = temp->next;
        }
        printf("\n");
    }
}

void freeGraph(Graph *graph){
    for(int v = 0; v < graph->numVertices; v++){
        AdjListNode *temp = graph->array[v].head;
        while(temp){
            AdjListNode *toFree = temp;
            temp = temp->next;
            free(toFree);
        }
    }
    free(graph->array);
    free(graph);
}

int main() {
    int V = 5;
    Graph* graph = createGraph(V);
    addEdge(graph, 0, 1, 0);
    addEdge(graph, 0, 4, 0);
    addEdge(graph, 1, 2, 0);
    addEdge(graph, 1, 3, 0);
    addEdge(graph, 1, 4, 0);
    addEdge(graph, 2, 3, 0);
    addEdge(graph, 3, 4, 0);

    printGraph(graph);

    // Free memory (not shown for brevity)
    return 0;
}
```

**Output:**

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==15325== Memcheck, a memory error detector
==15325== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15325== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15325== Command: ./final
==15325== 
Vertex 0:  -> 4 -> 1
Vertex 1:  -> 4 -> 3 -> 2 -> 0
Vertex 2:  -> 3 -> 1
Vertex 3:  -> 4 -> 2 -> 1
Vertex 4:  -> 3 -> 1 -> 0
==15325== 
==15325== HEAP SUMMARY:
==15325==     in use at exit: 0 bytes in 0 blocks
==15325==   total heap usage: 17 allocs, 17 frees, 1,304 bytes allocated
==15325== 
==15325== All heap blocks were freed -- no leaks are possible
==15325== 
==15325== For lists of detected and suppressed errors, rerun with: -s
==15325== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**Graph**

```css
        0
       / \
      /   \
     1-----4
    / \   /
   /   \ /
  2-----3
```

#### What Are Source and Sink in a Directed Graph?

1. **Source (in-degree = 0)**
   A **source** in a directed graph is a vertex with **no incoming edges**. In other words, there is no edge pointing **into** this vertex.
2. **Sink (out-degree = 0)**
   A **sink** in a directed graph is a vertex with **no outgoing edges**. In other words, there is no edge **going out** of this vertex.
3. In an **undirected** graph, every edge is bidirectional, so the notion of source/sink in the same sense (in-degree/out-degree) does not directly apply.
4. In **flow networks** (a special case of directed graphs with capacities), the terms **source** and **sink** often refer to the **designated** start and end points for flow, but the idea that a “sink” has no outgoing edges can still hold in many contexts.

#### Example

- If we have a directed edge (u→v), then u is the **origin** of that edge, and v is the **destination**.
- A **source** vertex has **outgoing** edges but **no** incoming edges (nobody points to it).
- A **sink** vertex has **incoming** edges but **no** outgoing edges (it does not point to any other vertex).

#### **Implementation Example #2**:

- **Source (in-degree = 0)**
  A **source** in a directed graph is a vertex with **no incoming edges**. In other words, there is no edge pointing **into** this vertex.

`hello.h`

```C
#include <stdbool.h>
#ifndef HELLO_H
#define HELLO_H

struct graph
{
    int n; // num of vertices
    int m; // number of edges
    
    // represents the successors (outgoin edges of a vertex) with
    struct successors
    {
        int d;   // num of successors
        int len; // number of slots in array
        int isSorted;
        int list[]; // actual list of successors start here
    } *alist[];     // array list
};

typedef struct graph *Graph;
Graph graphCreate(int n);
void graphDestroy(Graph g);
void graphAddEdge(Graph g, int u, int v); // u = source, v = sink
int graphVertexCount(Graph g);
int graphEdgeCount(Graph g);
int graphOutDegree(Graph g, int source);
int graphHasEdge(Graph g, int source, int sink);

void graphForeach(Graph g, int source, void (*f)(Graph g, int source, int sink, void *data), void *data);

void printGraph(Graph g);
void printEdge(Graph g, int source, int sink, void *data);
#endif // HELLO_H
```

`functions.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <strings.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <dirent.h>
#include <unistd.h>
#include <limits.h>
#include <glob.h>
#include <assert.h>
#include "hello.h"

// create a new graph with n vertices labeled 0..n-1 and no edges
Graph graphCreate(int n)
{
    Graph g;
    int i;
    g = malloc(sizeof(struct graph) + sizeof(struct successors *) * n);
    assert(g);

    g->n = n;
    g->m = 0;

    for (i = 0; i < n; i++)
    {
        g->alist[i] = malloc(sizeof(struct successors));
        assert(g->alist[i]);
        g->alist[i]->d = 0;
        g->alist[i]->len = 0;
        g->alist[i]->isSorted = 1;
    }
    return g;
}

// free all space used by graph
void graphDestroy(Graph g)
{
    for (int i = 0; i < g->n; i++)
    {
        free(g->alist[i]);
    }
    free(g);
}

// add an edge to an existing graph
// add an edge from vertex u to v
void graphAddEdge(Graph g, int u, int v)
{
    assert(u >= 0);
    assert(u < g->n);
    assert(v >= 0);

    // do we need to grow the list?
    while (g->alist[u]->d >= g->alist[u]->len)
    {
        // +1 because it might have been 0
        g->alist[u]->len = g->alist[u]->len * 2 + 1;
        g->alist[u] = realloc(g->alist[u], sizeof(struct successors) + sizeof(int) * g->alist[u]->len);
    }

    // now add the new sink
    g->alist[u]->list[g->alist[u]->d++] = v;
    
    // marks the successor list as unsorted
    g->alist[u]->isSorted = 0;

    // bump edge count
    g->m++;
}
// return the number of vertices in the graph
int graphVertexCount(Graph g)
{
    return g->n;
}
int graphEdgeCount(Graph g)
{
    return g->m;
}

// return the out-degree of a vertex
int graphOutDegree(Graph g, int source)
{
    assert(source >= 0);
    assert(source < g->n);
    return g->alist[source]->d;
}

// when we are willing to call bsearch
#define BSEARCH_THRESHOLD (10)

static int intcmp(const void *a, const void *b)
{
    return *((const int *)a) - *((const int *)b);
}

// return 1 if an edge (source to sink) exists, 0 otherwise
int graphHasEdge(Graph g, int source, int sink)
{
    int i;
    assert(source >= 0);
    assert(source < g->n);
    assert(sink >= 0);
    assert(sink < g->n);

    // if the number of successors (outgoing edges) meets the threshold, we will use bsearch, otherwise, we will use linear search
    if (graphOutDegree(g, source) >= BSEARCH_THRESHOLD)
    {
        // make sure it is sorted
        if (!g->alist[source]->isSorted)
        {
            qsort(g->alist[source]->list, g->alist[source]->d, sizeof(int), intcmp);
        }

        // call bsearch to do binary search
        return bsearch(&sink, g->alist[source]->list, g->alist[source]->d, sizeof(int), intcmp) != 0;
    }
    else
    {
        // just do a simple linear search
        for (i = 0; i < g->alist[source]->d; i++)
        {
            if (g->alist[source]->list[i] == sink)
            {
                return 1;
            }
        }
        return 0;
    }
}

// Iterates over all outgoing edges from source, applying the callback functioin f to each edge.
void graphForeach(Graph g, int source, void (*f)(Graph g, int source, int sink, void *data), void *data)
{
    int i;
    assert(source >= 0);
    assert(source < g->n);

    for (i = 0; i < g->alist[source]->d; i++)
    {
        f(g, source, g->alist[source]->list[i], data);
    }
}

void printGraph(Graph g)
{
    for (int i = 0; i < g->n; i++)
    {
        printf("Vertex %d: ", i);
        for (int j = 0; j < g->alist[i]->d; j++)
        {
            printf(" -> %d", g->alist[i]->list[j]);
        }
        printf("\n");
    }
}

void printEdge(Graph g, int source, int sink, void *data){
    printf("Edge from %d to %d\n", source, sink);
}
```

`main.c`

```C
#include <stdlib.h>
#include <stdio.h>
#include "hello.h"

int main()
{
    int V = 5;
    Graph graph = graphCreate(V);
    graphAddEdge(graph, 0, 1);
    graphAddEdge(graph, 0, 4);
    graphAddEdge(graph, 1, 2);
    graphAddEdge(graph, 1, 3);
    graphAddEdge(graph, 1, 4);
    graphAddEdge(graph, 2, 3);
    graphAddEdge(graph, 3, 4);
    printGraph(graph);

    // Test graphVertexCount
    int vertexCount = graphVertexCount(graph);
    printf("Number of vertices: %d\n", vertexCount);

    // Test graphEdgeCount
    int edgeCount = graphEdgeCount(graph);
    printf("Number of edges: %d\n", edgeCount);

    // Test graphHasEdge
    printf("Has edge(0, 1): %s\n", graphHasEdge(graph, 0, 1) ? "Yes" : "No");
    printf("Has edge(1, 4): %s\n", graphHasEdge(graph, 1, 4) ? "Yes" : "No");
    printf("Has edge(2, 4): %s\n", graphHasEdge(graph, 2, 4) ? "Yes" : "No");

    // Test graphOutDegree
    for (int i = 0; i < graphVertexCount(graph); i++)
    {
        printf("Out-degree of vertext %d: %d\n", i, graphOutDegree(graph, i));
    }

    // Test graphForeach
    printf("All edges in the graph:\n");
    graphForeach(graph, 0, printEdge, NULL);
    graphForeach(graph, 1, printEdge, NULL);
    graphForeach(graph, 2, printEdge, NULL);
    graphForeach(graph, 3, printEdge, NULL);
    graphForeach(graph, 4, printEdge, NULL);

        graphDestroy(graph);
    return 0;
}
```

**Output**:

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==10430== Memcheck, a memory error detector
==10430== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==10430== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==10430== Command: ./final
==10430== 
Vertex 0:  -> 1 -> 4
Vertex 1:  -> 2 -> 3 -> 4
Vertex 2:  -> 3
Vertex 3:  -> 4
Vertex 4: 
Number of vertices: 5
Number of edges: 7
Has edge(0, 1): Yes
Has edge(1, 4): Yes
Has edge(2, 4): No
Out-degree of vertext 0: 2
Out-degree of vertext 1: 3
Out-degree of vertext 2: 1
Out-degree of vertext 3: 1
Out-degree of vertext 4: 0
All edges in the graph:
Edge from 0 to 1
Edge from 0 to 4
Edge from 1 to 2
Edge from 1 to 3
Edge from 1 to 4
Edge from 2 to 3
Edge from 3 to 4
==10430== 
==10430== HEAP SUMMARY:
==10430==     in use at exit: 0 bytes in 0 blocks
==10430==   total heap usage: 13 allocs, 13 frees, 1,244 bytes allocated
==10430== 
==10430== All heap blocks were freed -- no leaks are possible
==10430== 
==10430== For lists of detected and suppressed errors, rerun with: -s
==10430== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

**Graph**:

```css
Graph Representation:

0
↓  ↘
1 → 4
↓ ↘
2 → 3
      ↘
       4
```

- **Vertex 4** has an out-degree of **0**, meaning it has no outgoing edges. This makes **vertex 4** the **sink**.
- There is no edge leading **into** vertex 0, so its in-degree = 0. Thus, **vertex 0** is the **source**.

#### Choosing Between Adjacency Matrix and Adjacency List

- **Use an adjacency matrix when:**
  - The graph is dense (number of edges is close to V²).
  - We need to frequently check for the existence of specific edges.
- **Use an adjacency list when:**
  - The graph is sparse (number of edges is much less than V²).
  - We need to efficiently iterate over the neighbors of a vertex.

---

### 3. Edge List

- An **edge list** is one way to represent a graph. 
- It consists of a list (or array) of edges, where each edge is represented by a pair (or tuple) of vertices that it connects, and possibly a weight if the graph is weighted.



#### Implementation in C 

Below is an example C program that uses an edge list to represent a graph and performs a simple traversal-like operation (printing all edges).

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int u;
    int v;
} Edge;

typedef struct {
    int numVertices;
    int numEdges;
    Edge* edges;
} Graph;

Graph* createGraph(int vertices, int edgesCount) {
    Graph* graph = (Graph*)malloc(sizeof(Graph));
    graph->numVertices = vertices;
    graph->numEdges = edgesCount;
    graph->edges = (Edge*)malloc(edgesCount * sizeof(Edge));
    return graph;
}

void addEdge(Graph* graph, int edgeIndex, int u, int v) {
    if(edgeIndex < graph->numEdges) {
        graph->edges[edgeIndex].u = u;
        graph->edges[edgeIndex].v = v;
    }
}

void printEdges(Graph* graph) {
    printf("Edge List:\n");
    for(int i = 0; i < graph->numEdges; i++) {
        printf("Edge %d: %d -- %d\n", i, graph->edges[i].u, graph->edges[i].v);
    }
}

int main() {
    int vertices, edgesCount;
    printf("Enter number of vertices: ");
    scanf("%d", &vertices);

    printf("Enter number of edges: ");
    scanf("%d", &edgesCount);

    Graph* graph = createGraph(vertices, edgesCount);

    printf("Enter edges in format 'u v' (0-indexed vertices):\n");
    for(int i = 0; i < edgesCount; i++) {
        int u, v;
        scanf("%d %d", &u, &v);
        addEdge(graph, i, u, v);
    }

    printEdges(graph);

    // Clean up
    free(graph->edges);
    free(graph);
    return 0;
}

```

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==17289== Memcheck, a memory error detector
==17289== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==17289== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==17289== Command: ./practice
==17289== 
Enter number of vertices: 4
Enter number of edges: 3
Enter edges in format 'u v' (0-indexed vertices):
0 1
1 2
2 3
Edge List:
Edge 0: 0 -- 1
Edge 1: 1 -- 2
Edge 2: 2 -- 3
==17289== 
==17289== HEAP SUMMARY:
==17289==     in use at exit: 0 bytes in 0 blocks
==17289==   total heap usage: 4 allocs, 4 frees, 2,088 bytes allocated
==17289== 
==17289== All heap blocks were freed -- no leaks are possible
==17289== 
==17289== For lists of detected and suppressed errors, rerun with: -s
==17289== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

- This means we have 4 vertices (`0, 1, 2, 3`) and 3 edges connecting them as follows: (0,1), (1,2), and (2,3).
- For the first edge, `u = 0` and `v = 1`, so `edges[0]` becomes {0, 1}.

- For the second edge, `u = 1` and `v = 2`, so `edges[1]` becomes {1, 2}.

- For the third edge, `u = 2` and `v = 3`, so `edges[2]` becomes {2, 3}.

### Summary of Use Cases:

- **Edge List**: Best for algorithms that need to process all edges or when edge modifications occur frequently. Not as ideal for neighbor queries.
- **Adjacency Matrix**: Best for dense graphs or when very fast edge lookups are required.
- **Adjacency List**: Generally preferred for most graph algorithms on sparse graphs due to its balance of space and neighbor iteration efficiency.
