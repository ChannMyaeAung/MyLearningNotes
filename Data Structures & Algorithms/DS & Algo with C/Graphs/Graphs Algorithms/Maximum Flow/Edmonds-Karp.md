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
```

`hello.c`

```c
```

`main.c`

```c
```



#### Program Output

```shell
```

