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

**Time Complexity:**

- With an adjacency matrix and a simple search for the minimum edge in each iteration, the time complexity is O(V2)O(V^2)O(V2) for a graph with VVV vertices.
- Using a more efficient data structure such as a priority queue (min-heap) with an adjacency list representation, the complexity can be reduced to O(Elog⁡V)O(E \log V)O(ElogV) where EEE is the number of edges.

## **C Implementation Using Adjacency Matrix (O(V²) approach):**

Below is a simple implementation of Prim's algorithm using an adjacency matrix. This approach is best suited for dense graphs.

#### Program Code

`hello.h`

```C
```

`hello.c`

```c
```

`main.c`



#### Program Output

```shell
```

