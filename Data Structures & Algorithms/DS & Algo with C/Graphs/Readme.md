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

### Adjacency Matrix

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



**Implementation Example:**

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
    AdjListNode* head;
} AdjList;

// Structure to represent the graph
typedef struct Graph {
    int numVertices;
    AdjList* array;
} Graph;

// Function to create a new adjacency list node
AdjListNode* newAdjListNode(int dest) {
    AdjListNode* newNode = malloc(sizeof(AdjListNode));
    newNode->dest = dest;
    newNode->next = NULL;
    return newNode;
}

// Initialize the graph
Graph* createGraph(int vertices) {
    Graph* graph = malloc(sizeof(Graph));
    graph->numVertices = vertices;

    // Create an array of adjacency lists
    graph->array = malloc(vertices * sizeof(AdjList));
    for(int i = 0; i < vertices; i++)
        graph->array[i].head = NULL;

    return graph;
}

// Add edge to the graph
void addEdge(Graph* graph, int src, int dest, int directed) {
    // Add an edge from src to dest
    AdjListNode* newNode = newAdjListNode(dest);
    newNode->next = graph->array[src].head;
    graph->array[src].head = newNode;

    // If undirected, add an edge from dest to src
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
Vertex 0: -> 4 -> 1
Vertex 1: -> 3 -> 2 -> 4 -> 0
Vertex 2: -> 3 -> 1
Vertex 3: -> 4 -> 2 -> 1
Vertex 4: -> 3 -> 1 -> 0
```



#### **Implementation Example #2**:

`hello.h`

```C
```

`functions.c`

```C
```

`main.c`

```C
```

**Output**:

```shell
```

