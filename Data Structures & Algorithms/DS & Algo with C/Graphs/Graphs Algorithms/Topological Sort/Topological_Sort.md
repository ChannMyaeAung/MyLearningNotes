# Topological Sort

- Topological Sort is an algorithm used to **linearly order the vertices of a Directed Acyclic Graph (DAG)** such that for every directed edge u→v, vertex u appears before v in the ordering. 
- It is widely used in scenarios like task scheduling, where certain tasks must be completed before others.

---

## **Key Concepts**

1. **Directed Acyclic Graph (DAG)**:
   Topological sort works only on DAGs because cycles would make it impossible to order the vertices linearly.
2. **Linear Ordering**:
   The algorithm ensures that dependencies (represented as directed edges) are respected in the ordering.

---

## **Applications**

1. **Task Scheduling**:
   Order tasks where certain tasks depend on others.
2. **Build Systems**:
   Determine the order to compile files with dependencies.
3. **Dependency Resolution**:
   In package managers like npm or pip.
4. **Course Scheduling**:
   Determine the order to take courses with prerequisites.

---

## Big O of Topological Sort

- **Kahn's Algorithm**: O(V+E), where V is the number of vertices and E is the number of edges.

- **DFS-Based**: O(V+E).

---

## **Steps of Topological Sort**

### **Approach 1: Kahn's Algorithm (Indegree-Based)**

1. **Calculate Indegrees**:
   Compute the indegree (number of incoming edges) for each vertex in the graph.
2. **Start with Zero-Indegree Vertices**:
   Add all vertices with an indegree of 0 to a queue (or similar structure).
3. **Process the Vertices**:
   - Remove a vertex from the queue.
   - Add it to the topological order.
   - For each outgoing edge from this vertex, decrease the indegree of the destination vertex.
   - If a destination vertex's indegree becomes 0, add it to the queue.
4. **Repeat Until All Vertices Are Processed**:
   The algorithm ends when the queue is empty. If the graph has all vertices processed, the result is a valid topological sort.

### **Approach 2: Depth-First Search (DFS-Based)**

1. **Perform DFS**:
   - Visit each vertex and recursively explore its neighbors.
   - When a vertex has no unvisited neighbors, add it to a stack or result list.
2. **Reverse the Result**:
   - Since vertices are added to the stack after all their dependencies are visited, reversing the stack gives the correct topological order.
3. **Cycle Detection**:
   If a back edge (an edge pointing to an ancestor in the recursion stack) is found, the graph is not a DAG, and topological sorting is not possible.

---

## **Pseudocode** (Kahn's Algorithm)

```css
1. Compute indegree for all vertices.
2. Initialize a queue with all vertices having indegree 0.
3. While the queue is not empty:
   - Remove a vertex u from the queue.
   - Add u to the topological order.
   - For each neighbor v of u:
       - Decrease indegree of v by 1.
       - If indegree of v becomes 0, add v to the queue.
4. If the topological order contains all vertices, return it.
   Otherwise, the graph has a cycle.
```

### Example

#### Graph:

Vertices: {A, B, C, D, E, F}
Edges:

- A → B, A → C
- B → D
- C → D, C → E
- E → F

#### Using Kahn’s Algorithm:

1. **Indegrees**:
   - A: 0, B: 1, C: 1, D: 2, E: 1, F: 1.
2. **Start with A** (indegree 0).
   - Add A to the topological order, reduce indegrees of B and C.
   - Updated indegrees: B: 0, C: 0, D: 2, E: 1, F: 1.
3. **Next, B and C** (indegree 0).
   - Add B, then C, reduce indegrees of their neighbors.
   - Updated indegrees: D: 0, E: 0, F: 1.
4. **Next, D and E** (indegree 0).
   - Add D, then E, reduce indegrees of their neighbors.
   - Updated indegrees: F: 0.
5. **Finally, F**.

**Topological Order**: A → B → C → D → E → F.

### C implementation of Kahn's Algorithm

#### Program Code

`hello.h`

```C
#define MAX_V 6

// struct to represent an adjacency list node
typedef struct Node
{
    int vertex;
    struct Node *next;
} Node;

// Struct to represent a graph using an adjacency list
typedef struct
{
    int numVertices;
    Node *adjList[MAX_V];
    int inDegree[MAX_V];
} Graph;

// Queue struct for vertices
typedef struct
{
    int items[MAX_V];
    int front;
    int rear;
} Queue;

Graph *createGraph(int V);

Node *createNode(int v);
void addEdge(Graph *g, int src, int dest);

void initQueue(Queue *q);

void enqueue(Queue *q, int value);
int dequeue(Queue *q);
int isEmpty(Queue *q);
void kahnTopologicalSort(Graph *g, char vertexNames[]);

void freeGraph(Graph *g);
```

`hello.c`

```C
// create a new graph with a given num of vertices
Graph *createGraph(int V)
{
    Graph *g = malloc(sizeof(Graph));
    assert(g);
    g->numVertices = V;
    for (int i = 0; i < V; i++)
    {
        g->adjList[i] = NULL;
        g->inDegree[i] = 0;
    }
    return g;
}

// create a new adjacency list node
Node *createNode(int v)
{
    Node *newNode = malloc(sizeof(Node));
    assert(newNode);
    newNode->vertex = v;
    newNode->next = NULL;
    return newNode;
}

// add directed edge from src to dest
void addEdge(Graph *g, int src, int dest)
{
    Node *newNode = createNode(dest);
    newNode->next = g->adjList[src];
    g->adjList[src] = newNode;

    // increase in-degree of destination
    g->inDegree[dest]++;
}

void initQueue(Queue *q)
{
    q->front = 0;
    q->rear = -1;
}

void enqueue(Queue *q, int value)
{
    if (q->rear < MAX_V - 1)
    {
        q->items[++(q->rear)] = value;
    }
}
int dequeue(Queue *q)
{
    if (!isEmpty(q))
    {
        return q->items[(q->front)++];
    }
    return -1;
}
int isEmpty(Queue *q)
{
    return q->rear < q->front;
}
void kahnTopologicalSort(Graph *g, char vertexNames[])
{
    int v = g->numVertices;
    int count = 0;
    int topologicalOrder[MAX_V];
    Queue q;
    initQueue(&q);

    // Enqueue all vertices with in-degree 0
    for (int i = 0; i < v; i++)
    {
        if (g->inDegree[i] == 0)
        {
            enqueue(&q, i);
        }
    }

    while (!isEmpty(&q))
    {
        int u = dequeue(&q);
        topologicalOrder[count++] = u;

        Node *temp = g->adjList[u];
        while (temp)
        {
            int v = temp->vertex;
            g->inDegree[v]--;
            if (g->inDegree[v] == 0)
            {
                enqueue(&q, v);
            }
            temp = temp->next;
        }
    }

    // if topological sort includes fewer vertices than total, there's a cycle.
    if (count != v)
    {
        printf("The graph has a cycle. Topological sort not possible.\n");
        return;
    }

    // Print topological order with original vertex names
    for (int i = 0; i < count; i++)
    {
        printf("%c ", vertexNames[topologicalOrder[i]]);
    }
    printf("\n");
}

void freeGraph(Graph *g)
{
    if (g == NULL)
    {
        return;
    }

    for (int i = 0; i < g->numVertices; i++)
    {
        Node *temp = g->adjList[i];
        while (temp)
        {
            Node *toFree = temp;
            temp = temp->next;
            free(toFree);
        }
    }
    free(g);
}
```

`main.c`

```C
int main()
{
    // Map character vertices to indices: A->0, B->1, C->2, D->3, E->4, F->5
    char vertexNames[MAX_V] = {'A', 'B', 'C', 'D', 'E', 'F'};

    Graph *g = createGraph(MAX_V);

    // Add edges based on the provided graph:
    // A → B, A → C, B → D, C → D, C → E, E → F
    addEdge(g, 0, 1); // A → B
    addEdge(g, 0, 2); // A → C
    addEdge(g, 1, 3); // B → D
    addEdge(g, 2, 3); // C → D
    addEdge(g, 2, 4); // C → E
    addEdge(g, 4, 5); // E → F

    printf("Topological Sort (Kahn's Algorithm):\n");
    kahnTopologicalSort(g, vertexNames);

    freeGraph(g);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==39494== Memcheck, a memory error detector
==39494== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==39494== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==39494== Command: ./final
==39494== 
Topological Sort (Kahn's Algorithm):
A C B E D F 
==39494== 
==39494== HEAP SUMMARY:
==39494==     in use at exit: 0 bytes in 0 blocks
==39494==   total heap usage: 8 allocs, 8 frees, 1,200 bytes allocated
==39494== 
==39494== All heap blocks were freed -- no leaks are possible
==39494== 
==39494== For lists of detected and suppressed errors, rerun with: -s
==39494== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

- **A → B** (0 → 1)
- **A → C** (0 → 2)
- **B → D** (1 → 3)
- **C → D** (2 → 3)
- **C → E** (2 → 4)
- **E → F** (4 → 5)

**Graph Visualization**:

```css
A (0) →  B (1) → D (3)
|        ^
|        |
↓        |
C (2) → E (4) → F (5)
```

**Validating the Topological Sort:**

A valid **topological sort** ensures that for every directed edge **U → V**, vertex **U** comes before vertex **V** in the ordering. Let's verify the output **"A C B E D F"** against the graph's dependencies.

1. **A before B:**
   - **Edge:** A → B
   - **Output:** A comes before B ✔️
2. **A before C:**
   - **Edge:** A → C
   - **Output:** A comes before C ✔️
3. **B before D:**
   - **Edge:** B → D
   - **Output:** B comes before D ✔️
4. **C before D:**
   - **Edge:** C → D
   - **Output:** C comes before D ✔️
5. **C before E:**
   - **Edge:** C → E
   - **Output:** C comes before E ✔️
6. **E before F:**
   - **Edge:** E → F
   - **Output:** E comes before F ✔️

**Additional Validation:**

- Order Between B and C:
  - **No direct dependency:** B and C can appear in any order relative to each other.
  - **Output:** C appears before B, which is acceptable ✔️

---



## Pseudocode (DFS-Based)

```css
1. Initialize an empty stack and a visited set.
2. For each vertex v in the graph:
   - If v is not visited, call DFS(v).
3. DFS(v):
   - Mark v as visited.
   - For each neighbor u of v:
       - If u is not visited, call DFS(u).
   - Add v to the stack.
4. Reverse the stack to get the topological order.
```

### C Implementation of Topological Sort(DFS)

#### Program Code

`hello.h`

```C
#define MAX_V 100

// Graph representation using adjacency list
typedef struct Node
{
    int vertex;
    struct Node *next;
} Node;

typedef struct
{
    Node *head[MAX_V]; // Array of pointers to the head nodes of adjacency lists
    int numVertices;
} Graph;

// Creates a new graph with a specified number of vertices
Graph *createGraph(int vertices);

// Creates a new adjacency list node for a given vertex
Node *createNode(int v);

// Adds a directed edge from 'src' to 'dest' in the graph
void addEdge(Graph *g, int src, int dest);

// Recursive utility function for topological sort using DFS
void topologicalSortUtil(Graph *g, int v);

// Performs topological sort on the graph and prints the sorted order
void topologicalSort(Graph *g);

// Frees all dynamically allocated memory associated with the graph
void freeGraph(Graph *g);
```

`hello.c`

```c
int visited[MAX_V]; 
int stack[MAX_V];
int top = -1;

// Create a new graph with a given num of vertices
Graph *createGraph(int vertices)
{
    Graph *g = malloc(sizeof(Graph));
    assert(g);
    g->numVertices = vertices;
    for (int i = 0; i < vertices; i++)
    {
        g->head[i] = NULL;
        visited[i] = 0;
    }
    return g; // Return the pointer to the newly created graph
}

// Create a new adjacency list node
Node *createNode(int v)
{
    Node *newNode = malloc(sizeof(Node));
    assert(newNode);
    newNode->vertex = v;
    newNode->next = NULL;
    return newNode; // Return the pointer to the newly created node
}

// Add directed edge from src to dest
void addEdge(Graph *g, int src, int dest)
{
    Node *newNode = createNode(dest);

    // Insert the new node at the beginning of the adjacency list for 'src'
    newNode->next = g->head[src];
    // Update the head to point to the new node
    g->head[src] = newNode;
}

// Recursive util func for topological sort
void topologicalSortUtil(Graph *g, int v)
{
    visited[v] = 1; // Mark the current vertex as visited

    // Traverse all adjacent vertices
    Node *temp = g->head[v];
    while (temp != NULL)
    {
        int adjVertex = temp->vertex;
        if (!visited[adjVertex])
        {
            topologicalSortUtil(g, adjVertex);
        }
        temp = temp->next;
    }

    // push the current vertex onto the stack after exploring all adjacent vertices
    stack[++top] = v;
}

void topologicalSort(Graph *g)
{
    // call the recursive helper func to store topological sort starting from all vertices one by one
    for (int i = 0; i < g->numVertices; i++)
    {
        if (!visited[i])
        {
            topologicalSortUtil(g, i);
        }
    }

    // print the contents of the stack which represents the topological order
    while (top >= 0)
    {
        printf("%d ", stack[top--]);
    }
    printf("\n");
}

void freeGraph(Graph *g)
{
    if (g == NULL)
    {
        return;
    }

    // iterate thru each adjacency list
    for (int i = 0; i < g->numVertices; i++)
    {
        Node *current = g->head[i];
        while (current != NULL)
        {
            Node *temp = current;
            current = current->next;
            free(temp); // Free each node
        }
    }
    free(g);
}
```

`main.c`

```c
int main()
{
    int v, e;
    printf("Enter the number of vertices: ");
    scanf("%d", &v);
    printf("Enter the number of edges: ");
    scanf("%d", &e);

    Graph *g = createGraph(v);

    printf("Enter each edge in the format 'src dest':\n");
    for (int i = 0; i < e; i++)
    {
        int u, v;
        scanf("%d %d", &u, &v);
        addEdge(g, u, v);
    }

    printf("Topological Sort:\n");
    topologicalSort(g);
    freeGraph(g);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==19974== Memcheck, a memory error detector
==19974== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==19974== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==19974== Command: ./final
==19974== 
Enter the number of vertices: 6
Enter the number of edges: 6
Enter each edge in the format 'src dest':
5 2
5 0
4 0
4 1
2 3
3 1
Topological Sort:
5 4 2 3 1 0 
==19974== 
==19974== HEAP SUMMARY:
==19974==     in use at exit: 0 bytes in 0 blocks
==19974==   total heap usage: 9 allocs, 9 frees, 2,952 bytes allocated
==19974== 
==19974== All heap blocks were freed -- no leaks are possible
==19974== 
==19974== For lists of detected and suppressed errors, rerun with: -s
==19974== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

