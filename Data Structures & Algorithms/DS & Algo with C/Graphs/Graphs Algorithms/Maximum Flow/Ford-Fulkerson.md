# Ford-Fulkerson Algorithm

- Ford-Fulkerson algorithm is a greedy approach *(an approach for solving a problem by selecting the best option available at the moment. It doesn't worry whether the current best result will bring the overall optimal result)* for calculating the maximum possible flow in a network or a graph.

- A term, **flow network**, is used to describe a network of vertices and edges with a source (S) and a sink (T). 
  - Each vertex, except **S** and **T**, can receive and send an equal amount of stuff through it. **S** can only send and **T** can only receive stuff.

---

## Terminologies Used

#### Augmenting Path

It is the path available in a flow network.

#### Residual Graph

It represents the flow network that has additional possible flow.

#### Residual Capacity

It is the capacity of the edge after subtracting the flow from the maximum capacity.

---

## How the algorithm works

The algorithm follows:

1. Initialize the flow in all the edges to 0.
2. While there is an augmenting path between the source and the sink, add this path to the flow.
3. Update the residual graph.

**Note: We can also consider reverse-path if required because if we do not consider them, we may never find a maximum flow.**

```css
              0/9
         A  ------>  B
	   ↗            ↗  ↘
 0/8 ↗            ↗      ↘ 0/2
   ↗            ↗          ↘
S          0/7↗              T
  ↘         ↗              ↗
    ↘     ↗              ↗ 0/5
 0/3  ↘ ↗              ↗
        D --------> C
             0/4
```

1. Select any arbitrary path from S to T. In this step, we have selected path `S-A-B-T`. 
2. The minimum capacity among the three edges is 2 (`B-T`). Based on this, update the `flow/capacity` for each path.

```css
             2/9
         A  ------>  B
	   ↗            ↗  ↘
 2/8 ↗            ↗      ↘ 2/2
   ↗            ↗          ↘
S          0/7↗              T
  ↘         ↗              ↗
    ↘     ↗              ↗ 0/5
 0/3  ↘ ↗              ↗
        D --------> C
             0/4
```

3. Select another path `S-D-C-T`. The minimum capacity among these edges is 3 (`S-D`). Update the capacities according to this.

```css
             2/9
         A  ------>  B
	   ↗            ↗  ↘
 2/8 ↗            ↗      ↘ 2/2
   ↗            ↗          ↘
S          0/7↗              T
  ↘         ↗              ↗
    ↘     ↗              ↗ 3/5
 3/3  ↘ ↗              ↗
        D --------> C
             3/4
```

4. Now, let's consider the reverse-path `B-D` as well. Selecting path `S-A-B-D-C-T`. The minimum residual capacity among the edges is 1 (`D-C`).

Updating the capacities.

```css
             3/9
         A  ------>  B
	   ↗            ↗  ↘
 3/8 ↗            ↗      ↘ 2/2
   ↗            ↗          ↘
S          1/7↗              T
  ↘         ↗              ↗
    ↘     ↗              ↗ 3/5
 3/3  ↘ ↗              ↗
        D --------> C
             4/4
```

The capacity for forward and reverse paths are considered separately.

5. Adding all the flows = 2 + 3 + 1 = 6, which is the maximum possible flow on the flow network.

**Note: if the capacity for any edge is full, then that path cannot be used.**

---

## C Implementation

#### Program Code

`hello.h`

```C
#define A 0
#define B 1
#define C 2
#define MAX_NODES 1000
#define O 1000000000

extern int n; // num of vertices
extern int e; // num of edges
extern int capacity[MAX_NODES][MAX_NODES];
extern int flow[MAX_NODES][MAX_NODES];
extern int color[MAX_NODES]; // color array of BFS
extern int pred[MAX_NODES]; // predcessor array to store paths

int min(int x, int y);
extern int head, tail;
extern int q[MAX_NODES + 2];

void enqueue(int x);
int dequeue();
int bfs(int start, int target);
int fordFulkerson(int source, int sink);
```

`hello.c`

```c
int n;
int e;
int capacity[MAX_NODES][MAX_NODES];
int flow[MAX_NODES][MAX_NODES];
int color[MAX_NODES];
int pred[MAX_NODES];

int head, tail;
int q[MAX_NODES + 2];


/**
 * @brief Returns the minimum of two integers.
 *
 * @param x First integer.
 * @param y Second integer.
 * @return Minimum of x and y.
 */
int min(int x, int y)
{
    return x < y ? x : y;
}


/**
 * @brief Adds an element to the queue.
 *
 * @param x The element to enqueue.
 */
void enqueue(int x)
{
    if (tail >= MAX_NODES + 2)
    {
        fprintf(stderr, "Queue overflow while enqueing %d\n", x);
        exit(1);
    }
    q[tail] = x; // Add the element to the queue
    tail++; // increment the tail index
    color[x] = B; // Mark the vertex as visited
}

/**
 * @brief Removes and returns the front element from the queue.
 *
 * @return The dequeued element.
 */
int dequeue()
{
    if (head >= tail)
    {
        fprintf(stderr, "Queue underflow while dequeing.\n");
        exit(1);
    }
    int x = q[head]; // retrieve the front element
    head++; // increment the head index
    color[x] = C; // mark the vertex as fully explored
    return x; // return the dequeued element
}

/**
 * @brief Performs Breadth-First Search to find an augmenting path.
 *
 * @param start The source vertex.
 * @param target The sink vertex.
 * @return 1 if a path exists, 0 otherwise.
 */
int bfs(int start, int target)
{
    int u, v;
    
    // Initialize all vertices as unvisited 
    for (u = 0; u < n; u++)
    {
        color[u] = A;
    }
    
    // Initialize the queue
    head = tail = 0;
    enqueue(start); // enqueue the src vertex
    pred[start] = -1; // src has no predecessor
    
    // Traverse the graph using BFS
    while (head != tail)
    {
        u = dequeue(); // dequeue the next vertex
        
        // Iterate thru all adjacent vertices
        for (v = 0; v < n; v++)
        {
            // if the vertex is unvisited and the residual capacity is positive
            if (color[v] == A && capacity[u][v] - flow[u][v] > 0)
            {
                // enqueue the vertex
                enqueue(v);
                
                // set predecessor for path reconstruction
                pred[v] = u;
            }
        }
    }
    
    // check if the sink was reached
    return color[target] == C;
}

/**
 * @brief Implements the Ford-Fulkerson algorithm to find the maximum flow.
 *
 * @param source The source vertex.
 * @param sink The sink vertex.
 * @return The maximum flow from source to sink.
 */
int fordFulkerson(int source, int sink)
{
    int i, j, u;
    int max_flow = 0; // Initialize maximum flow to 0
    
    // Initialize all flows to 0
    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
        {
            flow[i][j] = 0;
        }
    }

    // Continue finding augmenting paths while they exist
    // Updating the residual values of edges
    while (bfs(source, sink))
    {
        int increment = O;  // Initialize to a large number (infinity)
        
        // Determine the minimum residual capacity along the path found by BFS
        for (u = n - 1; pred[u] >= 0; u = pred[u])
        {
            // Update increment if a smaller residual is found
            increment = min(increment, capacity[pred[u]][u] - flow[pred[u]][u]);
        }
        
        // Update the flows along the path
        for (u = n - 1; pred[u] >= 0; u = pred[u])
        {
            flow[pred[u]][u] += increment; // Add flow in the forward direction
            flow[u][pred[u]] -= increment; // Subtract flow in the reverse direction
        }
        // Add the increment to the total maximum flow
        max_flow += increment;
    }

    return max_flow; // Return the computed maximum flow
}
```

`main.c`

```c
int main()
{
    // Initialize the number of vertices and edges
    n = 6; // Number of vertices
    e = 7; // Number of edges

    // Initialize the capacity matrix to 0 for all vertex pairs
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            capacity[i][j] = 0;
        }
    }

    // Define the capacities of the edges in the graph
    // Each capacity[i][j] represents the capacity from vertex i to vertex j
    capacity[0][1] = 8; // Edge from vertex 0 to vertex 1 with capacity 8
    capacity[0][4] = 3; // Edge from vertex 0 to vertex 4 with capacity 3
    capacity[1][2] = 9; // Edge from vertex 1 to vertex 2 with capacity 9
    capacity[2][4] = 7; // Edge from vertex 2 to vertex 4 with capacity 7
    capacity[2][5] = 2; // Edge from vertex 2 to vertex 5 with capacity 2
    capacity[3][5] = 5; // Edge from vertex 3 to vertex 5 with capacity 5
    capacity[4][2] = 7; // Edge from vertex 4 to vertex 2 with capacity 7
    capacity[4][3] = 4; // Edge from vertex 4 to vertex 3 with capacity 4

    int s = 0, t = 5; // Define source (vertex 0) and sink (vertex 5)

    // Call the fordFulkerson function to compute the maximum flow from source to sink
    printf("Max Flow: %d\n", fordFulkerson(s, t));
    
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Max Flow: 6
```

###### **Step-by-Step Flow Calculation**

**Initial Setup:**

- **Flow Matrix (`flow[][]`):** Initialized to **0** for all edges.
- **Max Flow (`max_flow`):** Starts at **0**.

**Augmenting Path 1:** `0 → 1 → 2 → 5`

- **Residual Capacities:**
  - `0 → 1`: 8
  - `1 → 2`: 9
  - `2 → 5`: 2
- **Bottleneck (Minimum Capacity):** **2**
- **Flow Update:**
  - `flow[0][1] += 2` → `flow[0][1] = 2`
  - `flow[1][2] += 2` → `flow[1][2] = 2`
  - `flow[2][5] += 2` → `flow[2][5] = 2`
- **Total Max Flow:** `0 + 2 = 2`

**Augmenting Path 2:** `0 → 4 → 3 → 5`

- **Residual Capacities:**
  - `0 → 4`: 3
  - `4 → 3`: 4
  - `3 → 5`: 5
- **Bottleneck (Minimum Capacity):** **3**
- **Flow Update:**
  - `flow[0][4] += 3` → `flow[0][4] = 3`
  - `flow[4][3] += 3` → `flow[4][3] = 3`
  - `flow[3][5] += 3` → `flow[3][5] = 3`
- **Total Max Flow:** `2 + 3 = 5`

**Augmenting Path 3:** `0 → 1 → 2 → 4 → 3 → 5`

- **Residual Capacities:**
  - `0 → 1`: 8 - 2 = 6
  - `1 → 2`: 9 - 2 = 7
  - `2 → 4`: 7
  - `4 → 3`: 4 - 3 = 1
  - `3 → 5`: 5 - 3 = 2
- **Bottleneck (Minimum Capacity):** **1**
- **Flow Update:**
  - `flow[0][1] += 1` → `flow[0][1] = 3`
  - `flow[1][2] += 1` → `flow[1][2] = 3`
  - `flow[2][4] += 1` → `flow[2][4] = 1`
  - `flow[4][3] += 1` → `flow[4][3] = 4`
  - `flow[3][5] += 1` → `flow[3][5] = 4`
- **Total Max Flow:** `5 + 1 = 6`

**Final Check:**

After these iterations, attempting to find another augmenting path:

- **No Additional Paths Available:**
  - Residual Capacities:
    - `0 → 1`: 6 - 3 = 3
    - `0 → 4`: 3 - 3 = 0
    - `1 → 2`: 7 - 3 = 4
    - `2 → 5`: 2 - 2 = 0
    - `4 → 3`: 4 - 4 = 0
    - `3 → 5`: 5 - 4 = 1
- **Result:**
  No further augmenting paths can increase the flow without exceeding capacities.
- **Final Max Flow:** **6**