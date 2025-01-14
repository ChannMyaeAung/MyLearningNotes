# Prim's Algorithm

- Prim's algorithm is a greedy algorithm used to find a Minimum Spanning Tree (MST) for a weighted, undirected graph. 
- Starting from an arbitrary vertex, the algorithm grows the MST one edge at a time by always adding the least expensive edge that connects a vertex in the tree to a vertex outside the tree.

## **Steps of Prim's Algorithm:**

1. **Initialization:**
   - Start with any arbitrary vertex. Mark it as part of the MST.
   - Initialize a set or structure to keep track of vertices included in the MST.
   - Initialize an array (or similar structure) to store the minimum edge weight that connects each vertex not yet in the MST to the tree.
2. **Growing the MST:**
   - At each step, select the vertex not yet in the MST that has the smallest edge connecting it to a vertex already in the MST.
   - Include that vertex and the corresponding edge in the MST.
   - Update the minimum edge weights for the vertices not yet in the MST, if a cheaper connection via the newly added vertex is found.
3. **Repeat:**
   - Continue selecting the smallest edge connecting the MST to a new vertex until all vertices are included in the MST.

## **Applications**

- Network design (e.g., laying cables, roads, or pipelines).
- Approximating solutions for certain NP-hard problems.
- Cluster analysis in machine learning.

**Time Complexity:**

- With an adjacency matrix and a simple search for the minimum edge in each iteration, the time complexity is O(V<sup>2</sup>) for a graph with V vertices.
- Using a more efficient data structure such as a priority queue (min-heap) with an adjacency list representation, the complexity can be reduced to O(E log V) where E is the number of edges.

## Pseudocode

```css
1. Initialize:
   - MST = {}
   - Select an arbitrary starting vertex (u).
   - Mark u as included in the MST.
   - Initialize all other vertices as "not included" in MST.
   
2. While there are vertices not in MST:
   - Find the smallest edge (u, v) where u is in MST, and v is not.
   - Add this edge (u, v) to the MST.
   - Mark v as included in MST.

3. Repeat until all vertices are included.
```

### **Example**

**Graph:**

Vertices: {A, B, C, D, E}
Edges with weights:

- A-B: 1, A-C: 4
- B-C: 2, B-D: 6
- C-E: 3, D-E: 5

```css
      1         6
[A]------[B]--------[D]
 |        |          |
 |4       |2         |
 |        |          |
[C]------←↑          |5
 |                   |
 |3                  |
 |                   |
[E]------------------|
 
```



**Steps:**

1. Start at **A**. Add edge **A-B (weight 1)** to the MST.
2. From {A, B}, add edge **B-C (weight 2)** (smallest weight edge).
3. From {A, B, C}, add edge **C-E (weight 3)**.
4. From {A, B, C, E}, add edge **D-E (weight 5)**.

```css
      1         
[A]------[B]        [D]
          |          |
          |2         |
          |          |
[C]------←↑          |5
 |                   |
 |3                  |
 |                   |
[E]------------------|
```



**Resulting MST:**

- Edges: {A-B, B-C, C-E, D-E}
- Total Weight: 11

## **C Implementation Using Adjacency Matrix (O(V²) approach):**

Below is a simple implementation of Prim's algorithm using an adjacency matrix. This approach is best suited for dense graphs.

#### Program Code

`hello.h`

```C
#define V 5

int minKey(int key[], bool mstSet[]);
void printMST(int parent[], int graph[V][V]);
void primMST(int graph[V][V]);
```

`hello.c`

```c
// Utility func to find the vertex with min key value,
// from the set of vertices not yet included in MST.
int minKey(int key[], bool mstSet[])
{
    int min = INT_MAX, min_idx;
    for (int v = 0; v < V; v++)
    {
        if (!mstSet[v] && key[v] < min)
        {
            min = key[v];
            min_idx = v;
        }
    }
    return min_idx;
}

// Func to print the constructed MST stored in parent[]
void printMST(int parent[], int graph[V][V])
{
    printf("Edge \tWeight\n");
    for (int i = 1; i < V; i++)
    {
        printf("%d - %d \t%d \n", parent[i], i, graph[i][parent[i]]);
    }
}

// func to construct and print MST for a graph represented using adjacency matrix
void primMST(int graph[V][V])
{
    int parent[V];  // Array to store constructed MST
    int key[V];     // key values used to pick min weight edge in cut
    bool mstSet[V]; // to represent set of vertices included in MST

    // initialize all keys as INFINITE and mstSet[] as false
    for (int i = 0; i < V; i++)
    {
        key[i] = INT_MAX;
        mstSet[i] = false;
    }

    // Always include first vertex in MST
    key[0] = 0;     // make key 0 so that this vertex is picked first
    parent[0] = -1; // first node is always root of MST

    // The MST will have V vertices
    for (int count = 0; count < V - 1; count++)
    {
        // Pick the min key vertex from the set of vertices not yet included in MST
        int u = minKey(key, mstSet);

        // Add the picked vertex to the MST set
        mstSet[u] = true;

        // Update key value and parent index of adjacent vertices of the picked vertex.
        for (int v = 0; v < V; v++)
        {
            // graph[u][v] is non zero only for adjacent vertices of u
            // mstSet[v] is false for vertices not yet included in MST
            // update the key only if graph[u][v] is smaller than key[v]
            // if there is an edge between u and v &&  v is not yet included in the MST && updates only if the current edge weight is smaller than the existing key for vertex v
            if (graph[u][v] && !mstSet[v] && graph[u][v] < key[v])
            {
                
                // sets the parent of vertex v to vertex u, indicating that the edge u-v is included in the MST
                // if parent[2] = 1, it means the edge connecting vertex 1 to vertex 2 is part of the MST.
                parent[v] = u;
                
                // updates the key value of vertex v to the weight of the edge u-v, representing the min weight edge connecting v to the MST.
                // if graph[1][2] = 3 and key[2] = 5, then key[2] is updated to 3 because 3 < 5, indicating a better connection to the MST via vertex 1.
                key[v] = graph[u][v];
            }
        }
    }
    printMST(parent, graph);
}
```

`main.c`

```c
int main()
{
    // example graph represented as an adjacency matrix.
    int graph[V][V] = {
        {0, 2, 0, 6, 0},
        {2, 0, 3, 8, 5},
        {0, 3, 0, 0, 7},
        {6, 8, 0, 0, 9},
        {0, 5, 7, 9, 0},
    };
    primMST(graph);
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
Edge 	Weight
0 - 1 	2 
1 - 2 	3 
0 - 3 	6 
1 - 4 	5 
Total weight of MST: 16
```

##### Explanation

```css
       (weight)
     0    1    2    3    4
   -------------------------
0 |  0    2    0    6    0
1 |  2    0    3    8    5
2 |  0    3    0    0    7
3 |  6    8    0    0    9
4 |  0    5    7    9    0

```

- Vertices: {0, 1, 2, 3, 4}
- Edges with weights (non-zero entries):
  - 0-1 (2), 0-3 (6)
  - 1-2 (3), 1-3 (8), 1-4 (5)
  - 2-4 (7)
  - 3-4 (9)

**Initial Graph**:

```css
    (2)       (5)
  0 ------ 1------ 4---------
   |      / |      |         |
(6)|  (8)/  |(3)   | (7)     |
   |    /   |      |         |
   3---↙    2-------         | (9)
   |                         |
   |-------------------------|
```

###### **Step-by-Step Execution of Prim's Algorithm**

Prim's algorithm builds the MST by **greedily selecting** the smallest weight edge that connects a vertex in the MST to a vertex outside the MST. Here's a detailed walkthrough based on our program.

**Initialization:**

- **Starting Vertex:** 0
- **MST Set (`mstSet`):** {0}
- **Key Values (`key`):** `[0, INF, INF, INF, INF]` where `INF = ∞`
- **Parent Array (`parent`):** `[-1, -1, -1, -1, -1]` (to store MST connections)

**Iteration 1:**

1. **Select Vertex with Minimum Key Not in MST:**
   - Vertex 0 is already in MST.
   - **Minimum Key Vertex:** 0 (key = 0)
2. **Add Vertex 0 to MST:**
   - **MST Set:** {0}
3. **Update Keys and Parents for Adjacent Vertices of Vertex 0:**
   - **Vertex 1:**
     - Edge 0-1: Weight = 2 < INF
     - **Update:** `key[1] = 2`, `parent[1] = 0`
   - **Vertex 3:**
     - Edge 0-3: Weight = 6 < INF
     - **Update:** `key[3] = 6`, `parent[3] = 0`
   - **Vertices 2 & 4:** No direct edge from 0
4. **Updated Arrays:**
   - **Key Values:** `[0, 2, INF, 6, INF]`
   - **Parent Array:** `[-1, 0, -1, 0, -1]`
   - **MST Set:** {0}

**Iteration 2:**

1. **Select Vertex with Minimum Key Not in MST:**
   - **Minimum Key Vertex:** 1 (key = 2)
2. **Add Vertex 1 to MST:**
   - **MST Set:** {0, 1}
3. **Update Keys and Parents for Adjacent Vertices of Vertex 1:**
   - **Vertex 2:**
     - Edge 1-2: Weight = 3 < INF
     - **Update:** `key[2] = 3`, `parent[2] = 1`
   - **Vertex 3:**
     - Edge 1-3: Weight = 8 > 6 (current key)
     - **No Update Needed**
   - **Vertex 4:**
     - Edge 1-4: Weight = 5 < INF
     - **Update:** `key[4] = 5`, `parent[4] = 1`
4. **Updated Arrays:**
   - **Key Values:** `[0, 2, 3, 6, 5]`
   - **Parent Array:** `[-1, 0, 1, 0, 1]`
   - **MST Set:** {0, 1}

**Iteration 3:**

1. **Select Vertex with Minimum Key Not in MST:**
   - **Minimum Key Vertex:** 2 (key = 3)
2. **Add Vertex 2 to MST:**
   - **MST Set:** {0, 1, 2}
3. **Update Keys and Parents for Adjacent Vertices of Vertex 2:**
   - **Vertex 4:**
     - Edge 2-4: Weight = 7 > 5 (current key)
     - **No Update Needed**
   - **Vertex 3:** No direct edge from 2
4. **Updated Arrays:**
   - **Key Values:** `[0, 2, 3, 6, 5]`
   - **Parent Array:** `[-1, 0, 1, 0, 1]`
   - **MST Set:** {0, 1, 2}

**Iteration 4:**

1. **Select Vertex with Minimum Key Not in MST:**
   - **Minimum Key Vertex:** 4 (key = 5)
2. **Add Vertex 4 to MST:**
   - **MST Set:** {0, 1, 2, 4}
3. **Update Keys and Parents for Adjacent Vertices of Vertex 4:**
   - Vertex 3:
     - Edge 4-3: Weight = 9 > 6 (current key)
     - **No Update Needed**
4. **Updated Arrays:**
   - **Key Values:** `[0, 2, 3, 6, 5]`
   - **Parent Array:** `[-1, 0, 1, 0, 1]`
   - **MST Set:** {0, 1, 2, 4}

**Iteration 5:**

1. **Select Vertex with Minimum Key Not in MST:**
   - **Minimum Key Vertex:** 3 (key = 6)
2. **Add Vertex 3 to MST:**
   - **MST Set:** {0, 1, 2, 3, 4}
3. **All Vertices Included in MST.**

**Output from Prim's Algorithm**

The algorithm produced the following edges for the MST:

```shell
Edge    Weight
0 - 1   2 
1 - 2   3 
0 - 3   6 
1 - 4   5 
```

**Drawing the Graph Based on MST Output**

Using the edges given:

- **0 — 1** with weight **2**
- **1 — 2** with weight **3**
- **0 — 3** with weight **6**
- **1 — 4** with weight **5**

The MST connects all vertices (0, 1, 2, 3, 4) without any cycles. Here's a simple sketch:

```css
      (2)
   0 ----- 1
   |       | \ 
(6)|    (3)|  \(5)
   |       |    \
   3       2     4
```

