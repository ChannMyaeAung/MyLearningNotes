# All-Pairs Shortest Paths Problem

**Definition:**

Given a weighted graph G=(V,E), the All-Pairs Shortest Paths problem involves finding the shortest paths between every pair of vertices in V.

**Types of Graphs:**

- **Directed vs. Undirected:** The edges can be directed or undirected.
- **Weighted vs. Unweighted:** Each edge has an associated weight (cost), which can be positive, negative, or zero.
- **Presence of Negative Cycles:** A negative cycle is a cycle whose total sum of edge weights is negative. APSP algorithms must handle or detect negative cycles appropriately.

**Applications:**

- **Network Routing:** Determining optimal routes in computer or transportation networks.
- **Urban Planning:** Designing efficient transportation or utility distribution systems.
- **Social Network Analysis:** Measuring closeness or influence between individuals.
- **Geographical Mapping:** Finding shortest driving or walking routes.

---

## Dynamic Programming Approach: Floyd-Warshall Algorithm

- The **Floyd-Warshall algorithm** is a dynamic programming technique that computes the shortest paths between all pairs of vertices in a graph. 
- It's particularly effective for dense graphs and can handle negative edge weights, provided there are no negative cycles.