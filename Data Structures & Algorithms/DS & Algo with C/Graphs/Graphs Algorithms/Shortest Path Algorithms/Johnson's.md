# Johnson's Algorithm

- **Johnson's Algorithm** is an efficient method to find the shortest paths between all pairs of vertices in a sparse, weighted, directed graph. 
- It accommodates negative edge weights but requires that the graph does not contain any negative weight cycles.

---

## Why Use Johnson's Algorithm?

- **Efficiency on Sparse Graphs:** Johnson's Algorithm runs in `O(V * E+V<sup>2</sup> * log⁡ V) time when implemented with a Fibonacci heap, making it more efficient than Floyd-Warshall's O(V<sup>3</sup>) for sparse graphs.
- **Handling Negative Weights:** Unlike Dijkstra's Algorithm, which cannot handle negative edge weights, Johnson's Algorithm can process graphs with negative weights by reweighting the edges to eliminate negative weights.

---

## Applications

- **Network Routing:** Determining optimal paths in communication networks.
- **Urban Planning:** Designing efficient transportation or utility distribution systems.
- **Bioinformatics:** Comparing genetic sequences.
- **Social Network Analysis:** Measuring closeness or influence between individuals.

---

## Algorithm Overview

Johnson's Algorithm combines the strengths of both the Bellman-Ford and Dijkstra's algorithms to compute all-pairs shortest paths efficiently. The algorithm proceeds in the following steps:

1. **Add a New Vertex:** Introduce a new vertex `q` and connect it to every other vertex in the graph with an edge of weight 0.

2. **Run Bellman-Ford Algorithm:** Execute the Bellman-Ford algorithm from the new vertex `q` to compute a potential function `h(v)` for each vertex `v`. This potential is used to reweight the original graph to eliminate negative edge weights.

3. **Reweight the Graph:** Adjust the weights of the original graph's edges using the potential `h` such that all edge weights become non-negative. The new weight `w′(u,v)` is calculated as:

   `w′(u,v)=w(u,v)+h(u)−h(v)`

4. **Remove the Added Vertex:** Delete the newly added vertex `q` and its associated edges.

5. **Run Dijkstra's Algorithm:** For each vertex `u` in the original graph, perform Dijkstra's algorithm to find the shortest paths from `u` to all other vertices using the reweighted graph.

6. **Adjust the Shortest Paths:** Convert the reweighted shortest path distances back to the original graph's weights using the potential `h`:

   `d(u,v)=d′(u,v)−h(u)+h(v)`

7. **Detect Negative Weight Cycles:** If the Bellman-Ford algorithm detects a negative weight cycle, terminate as shortest paths are undefined.

---

### Why It Works

- The key idea is to reweight the edges to eliminate negative weights without altering the relative shortest path relationships. 
- By computing the potential `h` using Bellman-Ford, the algorithm ensures that all reweighted edge weights are non-negative, making Dijkstra's Algorithm applicable.

---

