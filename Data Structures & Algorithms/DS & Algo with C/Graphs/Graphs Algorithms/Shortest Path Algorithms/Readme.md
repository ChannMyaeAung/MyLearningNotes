# All Shortest Path Algorithms Overview



| Algorithm           | Negative Weights | All-Paris | Single Source | Specific Pair | Time Complexity                      |
| ------------------- | ---------------- | --------- | ------------- | ------------- | ------------------------------------ |
| Dijkstra            | No               | No        | Yes           | Yes           | O(V<sup>2</sup>) or O((V + E) log V) |
| Bellman-Ford        | Yes              | No        | Yes           | No            | O(V * E)                             |
| Floyd-Warshall      | Yes              | Yes       | No            | No            | O(V<sup>3</sup>)                     |
| A*                  | No               | No        | No            | Yes           | Depends on heuristic                 |
| Johnson's           | Yes              | Yes       | No            | No            | O(V<sup>2</sup> log V + V E)         |
| BFS                 | No weights       | No        | Yes           | Yes           | O(V + E)                             |
| SPFA                | Yes              | No        | Yes           | No            | Average O(E), Worst O(V E)           |
| Dynamic Programming | Yes              | No        | Yes (DAGs)    | No            | O(V + E)                             |

When finding the shortest path in graphs, there are several algorithms available, each suited for specific graph properties, such as edge weights, negative cycles, and graph size. Below are the common options:

------

### **1. Dijkstra's Algorithm**

- **Purpose**: Finds the shortest path from a single source to all other vertices.
- **Graph Type**: Works for weighted graphs with non-negative edge weights.
- **Time Complexity:**
  - O(V<sup>2</sup>) with an adjacency matrix.
  - O((V+E) log⁡ V) with a priority queue (min-heap) and adjacency list.
- **Best Use Case**: Efficient for dense graphs or graphs with non-negative weights.

------

### **2. Bellman-Ford Algorithm**

- **Purpose**: Finds the shortest path from a single source to all other vertices, even with negative edge weights.
- **Graph Type**: Weighted graphs with positive or negative edge weights (but no negative weight cycles).
- **Time Complexity**: O(V * E), where V is the number of vertices and E is the number of edges.
- **Best Use Case**: When negative weights are present.

------

### **3. Floyd-Warshall Algorithm**

- **Purpose**: Finds the shortest path between all pairs of vertices.
- **Graph Type**: Weighted graphs with positive or negative weights (but no negative weight cycles).
- **Time Complexity**: O(V<sup>3</sup>).
- **Space Complexity**: O(V<sup>2</sup>) (for the distance matrix).
- **Best Use Case**: When we need the shortest path between all pairs of vertices in small graphs.

------

### **4. A\* Search Algorithm**

- **Purpose**: Finds the shortest path from a source to a target vertex using a heuristic.
- **Graph Type**: Weighted graphs with non-negative weights.
- **Time Complexity**: Depends on the heuristic but typically O(E)or better with good heuristics.
- **Best Use Case**: When we need the shortest path between two specific nodes and can define an appropriate heuristic (e.g., Euclidean distance in a spatial graph).

------

### **5. Johnson's Algorithm**

- **Purpose**: Finds the shortest path between all pairs of vertices and works with negative edge weights.
- **Graph Type**: Weighted graphs with positive or negative weights (but no negative weight cycles).
- **Time Complexity**: O(V<sup>2</sup> log V + V E) using Dijkstra's algorithm with a priority queue.
- **Best Use Case**: When we need all-pairs shortest paths for large sparse graphs with negative weights.

------

### **6. Breadth-First Search (BFS)**

- **Purpose**: Finds the shortest path in an unweighted graph (or equivalently, a graph where all edge weights are 1).
- **Graph Type**: Unweighted graphs.
- **Time Complexity**: O(V+E).
- **Best Use Case**: When all edges have the same weight (e.g., 1).

------

### **7. Bidirectional Search**

- **Purpose**: Finds the shortest path between two specific vertices by searching simultaneously from the source and target.
- **Graph Type**: Weighted or unweighted graphs.
- **Time Complexity**: Roughly half the cost of a single-direction search (O(b<sup>d/2</sup>), where b is the branching factor and d is the depth of the solution).
- **Best Use Case**: When the source and destination are known, and the graph is large.

------

### **8. Yen's Algorithm**

- **Purpose**: Finds the k-shortest paths between a source and target.
- **Graph Type**: Weighted graphs.
- **Time Complexity**: O(K⋅(V<sup>3</sup>)), where K is the number of paths to find.
- **Best Use Case**: When we need multiple shortest paths between two specific vertices.

------

### **9. Shortest Path Faster Algorithm (SPFA)**

- **Purpose**: An optimized version of Bellman-Ford that uses a queue to reduce redundant computations.
- **Graph Type**: Weighted graphs with positive or negative edge weights (but no negative weight cycles).
- **Time Complexity**: Average O(E), but worst-case O(VE).
- **Best Use Case**: When negative weights are present and the graph is sparse.

------

### **10. Dynamic Programming (for Specific Graphs)**

- **Purpose**: Finds the shortest path in **DAGs** (Directed Acyclic Graphs).
- **Graph Type**: DAGs with positive or negative edge weights.
- **Time Complexity**: O(V+E).
- **Best Use Case**: When the graph is acyclic.

---

### Choosing the Right Algorithm

1. **Unweighted Graphs**: Use BFS.
2. **Non-Negative Weights**: Use Dijkstra or A* (for a specific target).
3. **Negative Weights**: Use Bellman-Ford, SPFA, or Johnson's Algorithm.
4. **All-Pairs Shortest Path**: Use Floyd-Warshall or Johnson's Algorithm.
5. **Acyclic Graphs**: Use Dynamic Programming or topological sorting.