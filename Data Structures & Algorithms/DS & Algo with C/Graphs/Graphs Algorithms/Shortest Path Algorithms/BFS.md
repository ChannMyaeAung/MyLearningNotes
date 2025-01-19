# BFS to find the shortest path

- Breadth-First Search (BFS) can be used to find the shortest paths from a source vertex to all other vertices in an unweighted graph. 
- The key idea is that BFS explores vertices in layers, ensuring that the first time we reach a vertex, it's through the shortest possible route.

## C implementation

#### Program Code

`hello.h`

```c
#define MAX_V 100
#define INF INT_MAX

// struct for an adjacency list node
typedef struct Node
{
    int vertex;
    struct Node *next;
} Node;

// struct for graph representation
typedef struct
{
    int numVertices; // total num of vertices in the graph
    Node *adjList[MAX_V]; // Array of pointers to adjacency lists for each vertex
} Graph;

// Queue implementation for BFS
typedef struct
{
    int items[MAX_V];    // Array to store queue elements
    int front;           // Index of the front element in the queue
    int rear;            // Index of the rear element in the queue
} Queue;

Graph *createGraph(int v);
Node *createNode(int v);
void addEdge(Graph *g, int src, int dest);
void initQueue(Queue *q);
int isEmpty(Queue *q);
void enqueue(Queue *q, int value);
int dequeue(Queue *q);
void bfsShortestPath(Graph *g, int startVertex);

void freeGraph(Graph *g);
```

`hello.c`

```c
Graph *createGraph(int v)
{
    Graph *g = malloc(sizeof(Graph));
    assert(g);
    g->numVertices = v;
    for (int i = 0; i < v; i++)
    {
        g->adjList[i] = NULL;
    }
    return g; // Return the pointer to the newly created graph
}
Node *createNode(int v)
{
    Node *newNode = malloc(sizeof(Node));
    assert(newNode);
    newNode->vertex = v;
    newNode->next = NULL;
    return newNode;
}
void addEdge(Graph *g, int src, int dest)
{
    Node *newNode = createNode(dest);

    newNode->next = g->adjList[src];  // Point newNode's next to the current head of src's list
    g->adjList[src] = newNode;        // Update the head of src's list to newNode (newNode becomes the src(head))
}


void initQueue(Queue *q)
{
    q->front = 0; // Front index starts at 0
    q->rear = -1; // Rear index starts at -1 indicating the queue is empty
}
int isEmpty(Queue *q)
{
    return q->rear < q->front;
}
void enqueue(Queue *q, int value)
{
    // ensure there is space in the queue
    if (q->rear < MAX_V - 1)
    {
        // increment rear and add the value
        q->items[++q->rear] = value;
    }else
    {
        printf("Queue Overflow\n"); // Handle queue overflow if necessary
    }
}
int dequeue(Queue *q)
{
    if (!isEmpty(q))
    {
        // return the front item and increment front
        return q->items[q->front++];
    }
    return -1; // err condition
}

// BFS to find the shortest path from startVertex
void bfsShortestPath(Graph *g, int startVertex)
{
    int v = g->numVertices;

    // initialize distance array with infinity and predecessor aray with -1
    int distance[MAX_V];
    int predecessor[MAX_V];
    for (int i = 0; i < v; i++)
    {
        distance[i] = INF;
        predecessor[i] = -1; // Set predecessors to -1 indicating no path
    }

    Queue q;
    initQueue(&q);

    // start from the src vertex
    distance[startVertex] = 0; // Distance to source is 0
    enqueue(&q, startVertex); // Enqueue the source vertex
	
    
    // perform BFS
    while (!isEmpty(&q))
    {
        int current = dequeue(&q); // Dequeue a vertex from the queue
        Node *temp = g->adjList[current]; // Get the adjacency list for the current vertex

        // Iterate through all adjacent vertices
        while (temp != NULL)
        {
            int adjVertex = temp->vertex;
            // if vertex is not visited yet
            if (distance[adjVertex] == INF)
            {
                distance[adjVertex] = distance[current] + 1; // Update distance
                predecessor[adjVertex] = current; // Update predecessor
                enqueue(&q, adjVertex); // Enqueue the adjacent vertex
            }
            temp = temp->next;
        }
    }

    // print the shortest path from the startVertex
    printf("Vertex\tDistance from %d\tPath\n", startVertex);
    for (int i = 0; i < v; i++)
    {
        printf("%d\t", i);
        if (distance[i] == INF)
        {
            printf("Infinity\tNo path\n");
        }
        else
        {
            printf("%d\t\t", distance[i]);

            // print the path from startVertex to i
            int path[MAX_V];
            int pathIdx = 0;
            int crawl = i;
            while (crawl != -1)
            {
                path[pathIdx++] = crawl;
                crawl = predecessor[crawl];
            }

            for (int j = pathIdx - 1; j >= 0; j--)
            {
                printf("%d", path[j]);
                if (j > 0)
                {
                    printf(" -> ");
                }
            }
            printf("\n");
        }
    }
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
        Node *current = g->adjList[i];
        while (current != NULL)
        {
            Node *temp = current;
            current = current->next;
            free(temp);
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

    Graph *g = createGraph(v);

    printf("Enter the number of edges: ");
    scanf("%d", &e);

    printf("Enter each edge in the format 'src dest' (0-indexed vertices):\n");
    for (int i = 0; i < e; i++)
    {
        int src, dest;
        scanf("%d %d", &src, &dest);

        addEdge(g, src, dest);
        addEdge(g, dest, src);
    }

    int startVertex;
    printf("Enter the start vertex for BFS: ");
    scanf("%d", &startVertex);

    bfsShortestPath(g, startVertex);

    freeGraph(g);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==22816== Memcheck, a memory error detector
==22816== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==22816== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==22816== Command: ./final
==22816== 
Enter the number of vertices: 6
Enter the number of edges: 7
Enter each edge in the format 'src dest' (0-indexed vertices):
0 1
0 2
1 3
2 3
2 4
3 5
4 5
Enter the start vertex for BFS: 0
Vertex	Distance from 0	Path
0	0		0
1	1		0 -> 1
2	1		0 -> 2
3	2		0 -> 2 -> 3
4	2		0 -> 2 -> 4
5	3		0 -> 2 -> 4 -> 5
==22816== 
==22816== HEAP SUMMARY:
==22816==     in use at exit: 0 bytes in 0 blocks
==22816==   total heap usage: 17 allocs, 17 frees, 3,080 bytes allocated
==22816== 
==22816== All heap blocks were freed -- no leaks are possible
==22816== 
==22816== For lists of detected and suppressed errors, rerun with: -s
==22816== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```



### Complexity Analysis:

- **Time Complexity:**
  - Building the adjacency list with `addEdge` takes O(E) for E edges.
  - BFS traversal visits each vertex once and examines each edge once. This results in O(V+E) time complexity, where V is the number of vertices and E is the number of edges.
  - Reconstructing and printing paths for each vertex takes O(V) for each vertex in the worst-case scenario. However, this is typically dominated by the BFS traversal.
  - **Overall Time Complexity:** O(V+E) (assuming printing paths is not the dominant factor).
- **Space Complexity:**
  - The adjacency list requires O(V+E)space.
  - The distance, predecessor arrays, and queue require O(V) space.
  - **Overall Space Complexity:** O(V+E).

