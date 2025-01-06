# Understanding the Floyd-Warshall Algorithm

- The **Floyd-Warshall Algorithm** is a classic algorithm in graph theory used to find the **shortest paths** between all pairs of vertices in a **weighted** graph. 

- Unlike single-source algorithms like Dijkstra's or Bellman-Ford, Floyd-Warshall efficiently computes the shortest paths for every possible pair of vertices. 
- This makes it particularly useful in applications requiring comprehensive distance information, such as **network routing protocols, urban traffic planning, and more**.
- The **Floyd-Warshall Algorithm** is a dynamic programming approach that computes the shortest paths between all pairs of vertices in a weighted graph. 
- It systematically considers each vertex as an intermediate point in potential paths and updates the shortest path distances accordingly.

## **Key Characteristics**

- **All-Pairs Shortest Paths:** Computes shortest paths between every pair of vertices.
- **Dynamic Programming:** Builds up solutions by considering one intermediate vertex at a time.
- **Handles Negative Weights:** Can process graphs with negative edge weights but **cannot** handle graphs with **negative cycles**.
- **Time Complexity:** O(V³), where V is the number of vertices.
- **Space Complexity:** O(V²), primarily due to the distance matrix.

## **Mathematical Foundation**

The core idea is to iteratively update the distance matrix `dist[][]` where `dist[i][j]` represents the shortest distance from vertex `i` to vertex `j`. For each intermediate vertex `k`, the algorithm checks if the path from `i` to `j` via `k` is shorter than the current known path.

## When to Use Floyd-Warshall

- **All-Pairs Shortest Paths Needed:** When we need to know the shortest paths between every pair of vertices.
- **Small to Medium-Sized Graphs:** Given its O(V³) time complexity, it's more suitable for graphs where V is not excessively large (e.g., V ≤ 500).
- **Presence of Negative Edge Weights:** When the graph contains negative edge weights but no negative cycles.
- **Path Reconstruction:** It can be extended to reconstruct the actual paths, not just the distances.

**Note:** For single-source shortest paths in larger graphs without negative weights, algorithms like Dijkstra's are more efficient.