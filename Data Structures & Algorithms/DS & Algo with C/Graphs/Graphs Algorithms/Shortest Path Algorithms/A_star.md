# A* (A Star) Algorithm

- The **A\*** algorithm is a popular pathfinding and graph traversal algorithm known for efficiently finding the shortest path between a start and a goal node using heuristics. 

- It combines features of **Dijkstra's algorithm** (which finds the shortest path based purely on cost so far) and **Greedy Best-First Search** (which uses a heuristic to guide the search). The key idea of A* is to prioritize paths that appear to lead most quickly to the goal.

---

## Key Concepts of A*:

1. **g(n):** The cost (distance) from the start node to the current node `n`.

2. **h(n):** A heuristic function that estimates the cost from node `n` to the goal. This must be:

   - **Admissible:** It never overestimates the true cost to reach the goal.
   - **Consistent (Monotonic):** For every node `n` and every successor `n′` of `n`, the estimated cost from `n` to the goal is no greater than the cost of getting from `n` to `n′` plus the estimated cost from `n′` to the goal.

3. **f(n):** The total estimated cost of a cheapest solution through `n`, calculated as:

   `f(n)=g(n)+h(n)`

   A* expands the node with the lowest f(n) value next.

**Common heuristics:**

- **Manhattan Distance:** Suitable for grid-based maps with only horizontal and vertical movements.
- **Euclidean Distance:** Suitable for maps allowing diagonal movements.
- **Diagonal Distance:** Combines horizontal, vertical, and diagonal movements.

**Open and Closed Lists**

- **Open List:** A priority queue containing nodes that have been discovered but not yet evaluated. Nodes are prioritized based on their `f(n)` value.
- **Closed List:** A list containing nodes that have already been evaluated.

---

## Usage of A*:

The A* (A-star) algorithm remains widely used today across various domains. Here are a few key points about its prevalence and why it’s favored:

**1. Ubiquitous in Pathfinding and Navigation:**

- **Gaming:** A* is one of the most popular algorithms for character movement in video games. Its ability to efficiently calculate shortest paths on grids or graphs makes it ideal for NPC navigation, enemy AI, and route planning.
- **Robotics:** Robots use A* or its variants to plan paths in physical spaces while avoiding obstacles. It is a foundational tool in autonomous navigation systems.
- **Mapping and GPS Services:** While commercial applications (e.g., Google Maps) use more complex systems that may incorporate additional real-time data and optimizations, the underlying principles of A* for route calculation and graph traversal are often a part of these systems.

---

## Why A*?

- **Efficiency:** Uses heuristics to guide the search, reducing the number of nodes it needs to explore.
- **Optimality:** Guarantees the shortest path if the heuristic is admissible.
- **Flexibility:** Can be adapted to various types of graphs and heuristics.

---

## Step-by-Step Algorithm Explanation

Here's how the A* algorithm operates:

1. **Initialization:**
   - Create and initialize the open list (priority queue).
   - Create the start node with g(n)=0 and calculate its h(n).
   - Insert the start node into the open list.
   - Initialize the closed list as empty.
2. **Main Loop:**
   - While the open list is not empty:
     - Remove the node `n` with the lowest `f(n)` from the open list.
     - If `n` is the goal node, reconstruct and return the path.
     - Add `n` to the closed list.
     - For each neighbor `m` of `n`:
       - If `m` is in the closed list, skip it.
       - Calculate the tentative `g(m)=g(n)+cost(n,m)`.
       - If `m` is not in the open list or tentative `g(m)` is lower than the previously recorded `g(m)`:
         - Update m's g, h, and f.
         - Set n as m's parent.
         - If m is not in the open list, add it.
3. **Termination:**
   - If the open list is empty and the goal hasn't been reached, no path exists.
4. **Path Reconstruction:**
   - Starting from the goal node, follow parent pointers back to the start node to reconstruct the path.

---

## C implementation

Below is a complete C implementation of the A* algorithm using a grid-based map. The implementation includes all necessary components: node structure, priority queue, heuristic function, A* algorithm, path reconstruction, and a sample grid.

#### Program Code

`hello.h`

```c
#define WIDTH 10
#define HEIGHT 10
#define INF 99999

extern int grid[HEIGHT][WIDTH]; // 0 for free cell, 1 for obstacle

typedef struct Node
{

    int x, y;            // Coordinates on the grid
    int g;               // Cost from start node
    int h;               // Heuristic cost to goal
    int f;               // Total cost (g + h)
    struct Node *parent; // Pointer to the parent node for path reconstruction
} Node;

typedef struct PriorityQueue
{
    Node **elements; // Array of node pointers
    int size;        // Current number of elements
    int capacity;    // Maximum capacity
} PriorityQueue;

int heuristic(int x1, int y1, int x2, int y2);

PriorityQueue *createPriorityQueue(int capacity);

void swapNodes(Node **a, Node **b);

void enqueue(PriorityQueue *pq, Node *node);

Node *dequeue(PriorityQueue *pq);

int isEmpty(PriorityQueue *pq);

void freePriorityQueue(PriorityQueue *pq);

void aStar(int startX, int startY, int goalX, int goalY);
```

`hello.c`

```c
int grid[HEIGHT][WIDTH];

int heuristic(int x1, int y1, int x2, int y2)
{
    return abs(x1 - x2) + abs(y1 - y2);
}

PriorityQueue *createPriorityQueue(int capacity)
{
    PriorityQueue *pq = (PriorityQueue *)malloc(sizeof(PriorityQueue));
    assert(pq != NULL);
    pq->elements = (Node **)malloc(capacity * sizeof(Node *));
    assert(pq->elements != NULL);
    pq->size = 0;
    pq->capacity = capacity;
    return pq;
}

void swapNodes(Node **a, Node **b)
{
    Node *temp = *a;
    *a = *b;
    *b = temp;
}

void enqueue(PriorityQueue *pq, Node *node)
{
    if (pq->size == pq->capacity)
    {
        printf("Priority Queue Overflow\n");
        return;
    }
    // Insert at the end
    int i = pq->size;
    pq->elements[i] = node;
    pq->size++;

    // Bubble up
    while (i != 0)
    {
        int parent = (i - 1) / 2;
        if (pq->elements[parent]->f <= pq->elements[i]->f)
            break;
        swapNodes(&pq->elements[parent], &pq->elements[i]);
        i = parent;
    }
}

Node *dequeue(PriorityQueue *pq)
{
    if (pq->size <= 0)
        return NULL;
    if (pq->size == 1)
    {
        pq->size--;
        return pq->elements[0];
    }

    // Store the root
    Node *root = pq->elements[0];

    // Move the last element to root
    pq->elements[0] = pq->elements[pq->size - 1];
    pq->size--;

    // Heapify down
    int i = 0;
    while (1)
    {
        int smallest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < pq->size && pq->elements[left]->f < pq->elements[smallest]->f)
            smallest = left;
        if (right < pq->size && pq->elements[right]->f < pq->elements[smallest]->f)
            smallest = right;
        if (smallest != i)
        {
            swapNodes(&pq->elements[i], &pq->elements[smallest]);
            i = smallest;
        }
        else
        {
            break;
        }
    }

    return root;
}

int isEmpty(PriorityQueue *pq)
{
    return pq->size == 0;
}

void freePriorityQueue(PriorityQueue *pq)
{
    if (pq == NULL)
        return;

    // Do not free individual nodes here
    free(pq->elements);
    free(pq);
}

void aStar(int startX, int startY, int goalX, int goalY)
{
    // Create and initialize the open list (priority queue)
    PriorityQueue *openList = createPriorityQueue(WIDTH * HEIGHT);

    // Create and initialize the closed list
    int closedList[HEIGHT][WIDTH] = {0};

    // Create the start node
    Node *startNode = (Node *)malloc(sizeof(Node));
    assert(startNode != NULL);
    startNode->x = startX;
    startNode->y = startY;
    startNode->g = 0;
    startNode->h = heuristic(startX, startY, goalX, goalY);
    startNode->f = startNode->g + startNode->h;
    startNode->parent = NULL;

    // Enqueue the start node
    enqueue(openList, startNode);

    // Create a grid to store nodes for path reconstruction
    Node *nodes[HEIGHT][WIDTH] = {0};
    nodes[startY][startX] = startNode;

    // Directions for movement: Up, Down, Left, Right
    int directions[4][2] = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};

    // Main A* loop
    while (!isEmpty(openList))
    {
        // Dequeue the node with the lowest f value
        Node *current = dequeue(openList);
        assert(current != NULL);

        // If the goal is reached, reconstruct and print the path
        if (current->x == goalX && current->y == goalY)
        {
            printf("Path found!\n");
            printf("Path: ");

            // Reconstruct the path
            Node *pathNode = current;
            while (pathNode != NULL)
            {
                printf("(%d,%d) ", pathNode->x, pathNode->y);
                pathNode = pathNode->parent;
            }
            printf("\nTotal Cost: %d\n", current->g);

            // Free all nodes after path reconstruction
            for (int y = 0; y < HEIGHT; y++)
            {
                for (int x = 0; x < WIDTH; x++)
                {
                    if (nodes[y][x] != NULL)
                    {
                        free(nodes[y][x]);
                        nodes[y][x] = NULL;
                    }
                }
            }

            // Free the open list
            freePriorityQueue(openList);
            return;
        }

        // Mark the current node as closed
        closedList[current->y][current->x] = 1;

        // Explore neighbors
        for (int i = 0; i < 4; i++)
        {
            int newX = current->x + directions[i][0];
            int newY = current->y + directions[i][1];

            // Check if the new position is within the grid bounds
            if (newX < 0 || newX >= WIDTH || newY < 0 || newY >= HEIGHT)
                continue;

            // Check if the cell is blocked
            if (grid[newY][newX] == 1)
                continue;

            // Check if the node is already in the closed list
            if (closedList[newY][newX] == 1)
                continue;

            // Calculate the g, h, and f scores
            int tentative_g = current->g + 1; // Assuming uniform cost for movement

            // If the node is not yet created or a better path is found
            if (nodes[newY][newX] == NULL || tentative_g < nodes[newY][newX]->g)
            {
                // Create a new node or update the existing one
                Node *neighbor;
                if (nodes[newY][newX] == NULL)
                {
                    neighbor = (Node *)malloc(sizeof(Node));
                    assert(neighbor != NULL);
                    neighbor->x = newX;
                    neighbor->y = newY;
                    neighbor->parent = current;
                    nodes[newY][newX] = neighbor;
                }
                else
                {
                    neighbor = nodes[newY][newX];
                    neighbor->parent = current;
                }

                neighbor->g = tentative_g;
                neighbor->h = heuristic(newX, newY, goalX, goalY);
                neighbor->f = neighbor->g + neighbor->h;

                // Enqueue the neighbor node
                enqueue(openList, neighbor);
            }
        }

        // **Removed:** Free the current node after processing its neighbors
        // free(current);
        // Explanation: Nodes are freed after path reconstruction to avoid use-after-free
    }

    // If the open list is empty and goal was not reached
    printf("No path exists from (%d,%d) to (%d,%d)\n", startX, startY, goalX, goalY);

    // Free all nodes
    for (int y = 0; y < HEIGHT; y++)
    {
        for (int x = 0; x < WIDTH; x++)
        {
            if (nodes[y][x] != NULL)
            {
                free(nodes[y][x]);
                nodes[y][x] = NULL;
            }
        }
    }

    // Free the open list
    freePriorityQueue(openList);
}
```

`main.c`

```c
int main()
{
    // Initialize the grid (0: free, 1: obstacle)
    for (int y = 0; y < HEIGHT; y++)
    {
        for (int x = 0; x < WIDTH; x++)
        {
            grid[y][x] = 0; // Initialize all cells as free
        }
    }

    // Example obstacles
    // Let's add some obstacles to the grid
    grid[3][3] = 1;
    grid[3][4] = 1;
    grid[3][5] = 1;
    grid[4][5] = 1;
    grid[5][5] = 1;

    // Define start and goal positions
    int startX = 0, startY = 0;
    int goalX = 7, goalY = 7;

    printf("Starting A* Algorithm from (%d,%d) to (%d,%d)\n", startX, startY, goalX, goalY);

    // Run A* algorithm
    aStar(startX, startY, goalX, goalY);

    return 0;
}

```

#### Program Output

```sh
chan@CMA:~/C_Programming/test$ make valgrind
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./final
==17659== Memcheck, a memory error detector
==17659== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==17659== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==17659== Command: ./final
==17659== 
Starting A* Algorithm from (0,0) to (7,7)
Path found!
Path: (7,7) (6,7) (5,7) (5,6) (4,6) (4,5) (4,4) (3,4) (2,4) (2,3) (2,2) (2,1) (1,1) (0,1) (0,0) 
Total Cost: 14
==17659== 
==17659== HEAP SUMMARY:
==17659==     in use at exit: 0 bytes in 0 blocks
==17659==   total heap usage: 67 allocs, 67 frees, 3,888 bytes allocated
==17659== 
==17659== All heap blocks were freed -- no leaks are possible
==17659== 
==17659== For lists of detected and suppressed errors, rerun with: -s
==17659== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

