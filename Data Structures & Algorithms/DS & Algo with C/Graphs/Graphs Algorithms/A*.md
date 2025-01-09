# A* (A Star) Algorithm

- The **A\*** algorithm is a popular pathfinding and graph traversal algorithm known for efficiently finding the shortest path between a start and a goal node using heuristics. 

- It combines features of **Dijkstra's algorithm** (which finds the shortest path based purely on cost so far) and **Greedy Best-First Search** (which uses a heuristic to guide the search). The key idea of A* is to prioritize paths that appear to lead most quickly to the goal.

### Key Concepts of A*:

1. **g(n):** The cost from the start node to the current node `n`.

2. **h(n):** A heuristic function that estimates the cost from node `n` to the goal. This must be:

   - **Admissible:** It never overestimates the true cost to reach the goal.
   - **Consistent (Monotonic):** For every node `n` and every successor n′ of `n`, `h(n)≤c(n,n′)+h(n′)`, where c(n,n′) is the cost to move from `n` to n′.

3. **f(n):** The total estimated cost of a cheapest solution through `n`, calculated as:

   `f(n)=g(n)+h(n)`

   A* expands the node with the lowest f(n) value next.

