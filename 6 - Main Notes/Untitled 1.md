Consider how to solve the following problem:

You are given a directed graph in which **every edge has a weight 0 or 1**. Find the shortest distance from the Vertex `S` to each vertex on this graph.

# Solution
This problem can be solved with algorithms of solving shortest path problems like Dijkstra's algorithm. But 01-BFS is more efficient.

1. Prepare an empty deque.
2. Define provisional shortest distances `D[vertex index] = { provisional distance} as follows,
    * `D[S] = 0`
    * `D[x] = infinity` for every vertex `x` other than `S`.
3. Add vertex `S` as the only element of the deque.
4. Repeat the following until deque becomes empty,
    * Pop on element from the head of the deque. Let `V` be the popped vertex.
    * If `V` has not been visited yet, do the following operation and mark `V` as visited,
        * For each vertex `W` adjacent to `V`, do the following,
            * If the length of the edge connecting `V` and `W` is `0` and `D[W] > D[V]`, then push `W` to the head of the deque and let `D[W] = D[V]`.
            * If the length of the edge connecting `V` and `W` is 1 and `D[W] > D[V] + 1`, then push `W` to the tail of the deque and let `D[W] = D[V] + 1`

While the outline of the algorithm is the same as Dijkstra's algorithm, it is distinctive in the usage of the deque. In this algorithm, the deque plays the role of priority queue in Dijkstra's algorithm.