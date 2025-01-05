## Overview of the Bellman-Ford Algorithm

- **Bellman-Ford** is an algorithm for finding the shortest paths from a single source vertex to all other vertices in a weighted graph. 

- It is capable of handling graphs with negative edge weights and can detect negative weight cycles, which makes it more versatile than Dijkstra's algorithm in certain scenarios.

**Key Characteristics:**

- **Graph Type:** Directed or undirected (treated as directed with edges in both directions), with possible negative edge weights.
- **Time Complexity:** O(V * E), where V is the number of vertices and E is the number of edges.
- **Space Complexity:** O(V + E) with an adjacency list representation.
- **Applications:** Routing protocols (e.g., Bellman-Ford is used in the RIP protocol), detecting arbitrage opportunities in currency exchange, etc.

---

## When to Use Bellman-Ford

- **Negative Edge Weights:** When our graph contains edges with negative weights, and we need to find shortest paths.
- **Negative Cycles Detection:** When we need to check for the existence of negative weight cycles, which indicate that no shortest path exists.
- **Simple Implementation:** Bellman-Ford is straightforward to implement, especially when using an edge list representation.

**Note:** If the graph doesn't have negative edge weights, Dijkstra's algorithm is generally more efficient.

---

## Algorithm Steps

The Bellman-Ford algorithm operates in two main phases:

1. **Relaxation Phase:**
   - Iterate |V| - 1 times over all edges.
   - For each edge (u, v) with weight w, if the current distance to v can be shortened by going through u, update the distance to v.
2. **Negative Cycle Detection Phase:**
   - Iterate once more over all edges.
   - If any distance can still be shortened, a negative weight cycle exists in the graph.

**Why |V| - 1 Iterations?**

In the worst case, the shortest path can have |V| - 1 edges. Therefore, after |V| - 1 iterations, all shortest paths should be found if no negative cycles exist.

---

## C Implementation

We'll implement the Bellman-Ford algorithm in C using an **Edge List** representation for simplicity. This approach is efficient for Bellman-Ford since the algorithm primarily iterates over edges.

`practice.h`

```C
```

`functions.c`

```C
```

`practice.c`

```C
```

