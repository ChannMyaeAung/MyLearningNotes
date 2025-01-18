# Edmonds-Karp Algorithm

- The **Edmonds-Karp algorithm** is a specific implementation of the Ford-Fulkerson method for computing the maximum flow in a flow network. 
- It improves upon the basic Ford-Fulkerson approach by specifying how to find augmenting paths: it uses Breadth-First Search (BFS) to always choose the shortest path (in terms of number of edges) from the source to the sink in the residual network. 
- This choice guarantees polynomial time complexity.

### **Key Concepts**

1. **Residual Network**:
   - Like in Ford-Fulkerson, we maintain a residual graph that indicates remaining capacity on each edge.
   - For an edge (u,v) with capacity c(u,v) and current flow f(u,v), the residual capacity is c<sub>f</sub>(u,v)=c(u,v)−f(u,v).
   - Also include a reverse edge (v,u) in the residual network with capacity f(u,v)to allow flow cancellation.
2. **Augmenting Path with BFS**:
   - Use BFS to find the shortest path from the source `s` to the sink `t` in the residual graph.
   - The path found by BFS minimizes the number of edges, which limits how much "detour" flow can take. This ensures that the algorithm converges in polynomial time.
3. **Algorithm Steps**:
   - Initialize flow f=0.
   - While an augmenting path from  `s` to `t` exists in the residual graph:
     1. Use BFS to find the shortest augmenting path.
     2. Determine the path flow, which is the minimum residual capacity along this path.
     3. For each edge (u,v) on the path:
        - Increase the flow f(u,v)) by the path flow.
        - Decrease the residual capacity c<sub>f</sub>(u,v) accordingly.
        - Increase the reverse edge capacity c<sub>f</sub>(v,u) by the path flow.
     4. Add the path flow to the overall maximum flow.
   - When no augmenting path exists, the current flow is maximum.

---

### **Time Complexity**

- The Edmonds-Karp algorithm runs in O(V×E<sup>2</sup>) time.
- Each BFS takes O(E)time, and the number of augmenting paths found is O(VE), leading to overall complexity.

---

## Implementation in C

Below is a C implementation of the Edmonds-Karp algorithm using an adjacency matrix for graph capacities. This implementation uses BFS to find augmenting paths.



#### Program Code

`hello.h`

```c
#define V 6

// A BFS based function to find the shortest augmenting path.
// Returns true if there is a path from source 's' to sink 't' in residualGraph.
// Also fills parent[] to store the path.
bool bfs(int rGraph[V][V], int s, int t, int parent[]);

// Edmonds-Karp algorithm for maximum flow
int edmondsKarp(int graph[V][V], int s, int t);
```

`hello.c`

```c
bool bfs(int rGraph[V][V], int s, int t, int parent[])
{
    bool visited[V];
    memset(visited, 0, sizeof(visited));

    // Create a queue for BFS
    int queue[V], front = 0, rear = 0;

    // Start from the src vertex
    visited[s] = true;
    parent[s] = -1;
    queue[rear++] = s;

    while (front < rear)
    {
        int u = queue[front++];

        for (int v = 0; v < V; v++)
        {

            // if v is not visited and there is available capacity
            if (!visited[v] && rGraph[u][v] > 0)
            {
                queue[rear++] = v;
                parent[v] = u;
                visited[v] = true;

                // Stop BFS upon reaching the sink
                if (v == t)
                {
                    return true;
                }
            }
        }
    }
    return false;
}

// Edmonds-Karp algorithm for maximum flow
int edmondsKarp(int graph[V][V], int s, int t)
{
    int u, v;

    // Create a residual graph and fill with initial capacities
    int rGraph[V][V];
    for (u = 0; u < V; u++)
    {
        for (v = 0; v < V; v++)
        {
            rGraph[u][v] = graph[u][v];
        }
    }

    int parent[V]; // array to store path found by BFS
    int maxFlow = 0;

    // Augment the flow while there is a path from src to sink
    while (bfs(rGraph, s, t, parent))
    {
        // Find the min residual capacity along the path filled by BFS
        int pathFlow = INT_MAX;
        for (v = t; v != s; v = parent[v])
        {
            u = parent[v];
            
            // updates the pathFlow to the smallest residual capacity found along the current augmenting path
            if (rGraph[u][v] < pathFlow)
            {
                pathFlow = rGraph[u][v];
            }
        }

        // Update residual capacities along the path
        for (v = t; v != s; v = parent[v])
        {
            u = parent[v];
            rGraph[u][v] -= pathFlow;
            rGraph[v][u] += pathFlow;
        }

        // Add path flow to overall flow
        maxFlow += pathFlow;
    }
    return maxFlow;
}
```

`main.c`

```c
int main()
{
    int graph[V][V] = {
        {0, 16, 13, 0, 0, 0},
        {0, 0, 10, 12, 0, 0},
        {0, 4, 0, 0, 14, 0},
        {0, 0, 9, 0, 0, 20},
        {0, 0, 0, 7, 0, 4},
        {0, 0, 0, 0, 0, 0}};

    int s = 0;
    int t = 5;

    printf("The maximum possible flow is %d\n", edmondsKarp(graph, s, t));
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
The maximum possible flow is 23
```

