# Understanding the Floyd-Warshall Algorithm

- The **Floyd-Warshall Algorithm** is a classic algorithm in graph theory used to find the **shortest paths** between all pairs of vertices in a **weighted** graph. 

- Unlike single-source algorithms like Dijkstra's or Bellman-Ford, Floyd-Warshall efficiently computes the shortest paths for every possible pair of vertices. 
- This makes it particularly useful in applications requiring comprehensive distance information, such as **network routing protocols, urban traffic planning, and more**.
- The **Floyd-Warshall Algorithm** is a dynamic programming approach that computes the shortest paths between all pairs of vertices in a weighted graph. 
- It systematically considers each vertex as an intermediate point in potential paths and updates the shortest path distances accordingly.

---

## **Key Characteristics**

- **All-Pairs Shortest Paths:** Computes shortest paths between every pair of vertices.
- **Dynamic Programming:** Builds up solutions by considering one intermediate vertex at a time.
- **Handles Negative Weights:** Can process graphs with negative edge weights but **cannot** handle graphs with **negative cycles**.
- **Time Complexity:** O(V³), where V is the number of vertices.
- **Space Complexity:** O(V²), primarily due to the distance matrix.

---

## **Mathematical Foundation**

The core idea is to iteratively update the distance matrix `dist[][]` where `dist[i][j]` represents the shortest distance from vertex `i` to vertex `j`. For each intermediate vertex `k`, the algorithm checks if the path from `i` to `j` via `k` is shorter than the current known path.

---

## When to Use Floyd-Warshall

- **All-Pairs Shortest Paths Needed:** When we need to know the shortest paths between every pair of vertices.
- **Small to Medium-Sized Graphs:** Given its O(V³) time complexity, it's more suitable for graphs where V is not excessively large (e.g., V ≤ 500).
- **Presence of Negative Edge Weights:** When the graph contains negative edge weights but no negative cycles.
- **Path Reconstruction:** It can be extended to reconstruct the actual paths, not just the distances.

**Note:** For single-source shortest paths in larger graphs without negative weights, algorithms like Dijkstra's are more efficient.

---

## Algorithm Steps

The Floyd-Warshall algorithm operates in three main phases:

1. **Initialization:**
   - Create a distance matrix `dist[][]` where `dist[i][j]` is the weight of the edge from `i` to `j` if it exists, or infinity (`INF`) otherwise.
   - Set `dist[i][i] = 0` for all vertices `i`.
2. **Dynamic Programming Iteration:**
   - For each intermediate vertex  `k` from  `0` to  `V-1`:
     - For each source vertex `i` from  `0` to  `V-1`:
       - For each destination vertex `j`from  `0` to  `V-1`:
         - If `dist[i][k] + dist[k][j] < dist[i][j]`, then update `dist[i][j] = dist[i][k] + dist[k][j]`.
3. **Negative Cycle Detection:**
   - After completing the iterations, check for negative cycles by verifying if any `dist[i][i] < 0` for any vertex `i`.
   - If such a condition exists, the graph contains a negative weight cycle, and the shortest paths are undefined.



------

## Visual Explanation

Let's say we have the following graph:

```css
      w=4        w = -2
	 |-------[1]---------|
	 ↓					 ↓
	[2]	→----(w= 3)----→[3]
	 |                   |
	 |------→[4]←--------|
	  w=-1           w=2		 


```

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      |      |      |
| 2    |      | 0    |      |      |
| 3    |      |      | 0    |      |
| 4    |      |      |      | 0    |

- First we initialize each node paths to itself to zero.
- The next step is to loop through the edges of our graph and fill in the table with corresponding weights.
- We will start with the edges [1, 3], which has -2.
- We loop through the remaining edges and added their weights.

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      | -2   |      |
| 2    | 4    | 0    | 3    |      |
| 3    |      |      | 0    | 2    |
| 4    |      | -1   |      | 0    |

- Now we will go for the nested loops `i, j, and k` using the adjacency matrix above.

```c
for k from 1 to V:
	for i from 1 to V:
		for j from 1 to V:
			if dist[i][j] > dist[i][k] + dist[k][j]
                dist[i][j] = dist[i][k] + dist[k][j]
```

- Let's start with k = 1, i = 1, j = 1:

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      | -2   |      |
| 2    | 4    | 0    | 3    |      |
| 3    |      |      | 0    | 2    |
| 4    |      | -1   |      | 0    |

```c
dist[i][j] > dist[i][k] + dist[k][j]:
dist[1][1] > dist[1][1] + dist[1][1]:
0 > 0 + 0: (false)

// the if condition isn't met, so we don't update our table.

//k = 1, i = 1, j = 2
dist[i][j] > dist[i][k] + dist[k][j]:
dist[1][2] > dist[1][1] + dist[1][2]:
INF > 0 + INF:
INF > INF: (false)
// Likewise for k = 1, i = 1, j = 3, we get 
dist[i][j] > dist[i][k] + dist[k][j]:
dist[1][3] > dist[1][1] + dist[1][3]:
-2 > 0 + -2:
-2 > -2: (false)

// let's get to a more interesting iteration
// k = 1, i = 2, j = 3
dist[i][j] > dist[i][k] + dist[k][j]:
dist[2][3] > dist[2][1] + dist[1][3]:
3 > 4 + (-2):
3 > 2: (true)
// so this time our if condition is met, therefore we update the paths dist[2][3] with the smaller weight which is the result of dist[2][1] + dist[1][3] which is 2.
// so dist[2][3] = 2
```

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      | -2   |      |
| 2    | 4    | 0    | 2    |      |
| 3    |      |      | 0    | 2    |
| 4    |      | -1   |      | 0    |

- Again, let's skip ahead to a more interesting iteration.
- This time with k = 2, i = 4, j = 1

```c
dist[i][j] > dist[i][k] + dist[k][j]:
dist[4][1] > dist[4][2] + dist[2][1]:
INF > -1 + 4:
INF > 3: (true)
```

- So the same steps will be applied. We update our table with the smaller value.
- So the `dist[4][1]` = `dist[4][2] + dist[2][1]`:
- `dist[4][1]` = 3.

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      | -2   |      |
| 2    | 4    | 0    | 2    |      |
| 3    |      |      | 0    | 2    |
| 4    | 3    | -1   |      | 0    |

- now we go through the same steps for the remaining iterations and update the table if the `if` condition is met.

|      | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 0    | -1   | -2   | 0    |
| 2    | 4    | 0    | 2    | 4    |
| 3    | 5    | 1    | 0    | 2    |
| 4    | 3    | -1   | 1    | 0    |

- Here is our result. A table of shortest paths between all pairs of vertices.
- **Time Complexity**: O(V<sup>3</sup>) since we have triple nested loops where V is the number of vertices.

---

## 4. C Implementation

We'll implement the Floyd-Warshall algorithm in C using an **Adjacency Matrix** representation, which is ideal for this algorithm due to its all-pairs nature.

Since Floyd-Warshall relies on an adjacency matrix, we'll use a 2D array to represent the graph. We'll also define constants for clarity.

#### Program Code

`practice.h`

```C
// define the num of vertices
#define V 4

// Define a value for infinity
#define INF 99999

void printSolution(int dist[][V]);

void floydWarshall(int graph[][V], int dist[][V]);
```

`functions.c`

```C
// Function to print the solution
void printSolution(int dist[][V])
{
    printf("Following matrix shows the shortest distances between every pair of vertices:\n");
    for (int i = 0; i < V; i++)
    {
        for (int j = 0; j < V; j++)
        {
            if (dist[i][j] == INF)
                printf("%7s", "INF");
            else
                printf("%7d", dist[i][j]);
        }
        printf("\n");
    }
}

// Function that implements Floyd-Warshall algorithm
void floydWarshall(int graph[][V], int dist[][V])
{
    // Initialize the solution matrix same as input graph matrix
    for (int i = 0; i < V; i++)
        for (int j = 0; j < V; j++)
            dist[i][j] = graph[i][j];

    // Add all vertices one by one to the set of intermediate vertices.
    // ---> Before start of an iteration, we have shortest distances between all pairs
    // of vertices such that the shortest distances consider only the vertices in
    // the set {0, 1, 2, .. k-1} as intermediate vertices.
    // ----> After the end of an iteration, vertex no. k is added to the set of intermediate
    // vertices and the set becomes {0, 1, 2, .. k}
    for (int k = 0; k < V; k++)
    {
        // Pick all vertices as source one by one
        for (int i = 0; i < V; i++)
        {
            // Pick all vertices as destination for the above picked source
            for (int j = 0; j < V; j++)
            {
                // If vertex k is on the shortest path from i to j, then update the value of dist[i][j]
                if (dist[i][k] != INF && dist[k][j] != INF && dist[i][k] + dist[k][j] < dist[i][j])
                {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }

    // Print the shortest distance matrix
    printSolution(dist);
}

```



`practice.c`

```C

int main()
{
    /* Let us create the following weighted graph
        0 --> 1 (5)
        0 --> 3 (10)
        1 --> 2 (3)
        2 --> 3 (1)
        3 --> 2 (-2)
    */

    int graph[V][V] = {
        {0, 5, INF, 10},
        {INF, 0, 3, INF},
        {INF, INF, 0, 1},
        {INF, INF, -2, 0}};

    // Initialize distance matrix
    int dist[V][V];

    // Run Floyd-Warshall algorithm
    floydWarshall(graph, dist);

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Following matrix shows the shortest distances between every pair of vertices:
      0      5      7      8
    INF      0      2      3
    INF    INF     -1      0
    INF    INF     -3     -2

```

