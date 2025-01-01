### Dijkstra’s Shortest Path Algorithm

Dijkstra's algorithm finds the shortest path from a single source vertex to all other vertices in a weighted graph with non-negative weights.

**Note:** Implementing Dijkstra's algorithm in C requires handling priority queues (often implemented with min-heaps) for efficiency. 

1. **Initialize:**
   - Set the distance to the source vertex as 0 and all others as infinity.
   - Create a priority queue and insert all vertices with their current distance.
2. **Process:**
   - While the queue is not empty:
     - Extract the vertex with the minimum distance.
     - For each neighbor of this vertex, calculate the tentative distance through the current vertex.
     - If this distance is less than the known distance, update it and adjust its position in the priority queue.
3. **Result:**
   - After processing, the distance array contains the shortest distances from the source to all vertices.

Implementing Dijkstra's algorithm efficiently in C involves creating a priority queue with operations like `insert` and `extract-min`, which can be done using a binary heap.



#### Program Code

`practice.h`

```C
```

`functions.c`

```C
```

`practice.c`

```C
```

#### Program Output

```shell
```

