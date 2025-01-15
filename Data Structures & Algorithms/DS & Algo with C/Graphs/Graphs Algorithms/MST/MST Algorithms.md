# The Minimum Spanning Tree

The Minimum Spanning Tree (MST) is the collection of edges required to connect all vertices in an undirected graph, with the minimum total edge weight.

- It's called a Minimum Spanning Tree, because it is a connected, acyclic, undirected graph, which is the definition of a tree data structure.
- In the real world, finding the Minimum Spanning Tree can help us find the most effective way to connect houses to the internet or to the electrical grid, or it can help us finding the fastest route to deliver packages.

## MST Algorithms

|                                                              | Prim's Algorithm                             | Kruskal's Algorithm                                          |
| ------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------ |
| Can it find MSTs (a Minimum Spanning Forest) in an unconnected graph? | No                                           | Yes                                                          |
| How does it start?                                           | The MST grows from a randomly chosen vertex. | The first edge in the MST is the edge with lowest edge weight. |
| What time complexity does it have?                           | O(V<sup>2</sup>), or O(E log V) (Optimized)  | O(E log E)                                                   |

