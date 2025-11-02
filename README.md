# teori graf week 2  

Hierholzer’s Algorithm — Description
Purpose:

Hierholzer’s Algorithm is used to find:

Eulerian Circuit → a path that visits every edge exactly once and returns to the starting vertex.

Eulerian Path → a path that visits every edge exactly once, but does not necessarily return to the starting vertex.

This algorithm works on graphs that contain an Eulerian path or circuit, meaning:

All vertices have even degree → Eulerian Circuit

Exactly two vertices have odd degree → Eulerian Path

Input and Output Description
Input

The number of vertices in the graph.

A list of edges that connect pairs of vertices.

Since this is an undirected graph, each edge connects two vertices in both directions.

Vertices can be represented by numbers (e.g., 0, 1, 2, 3, etc.).

Example Input (in code):

g = Graph(5)
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)
g.add_edge(3, 4)
g.add_edge(4, 2)


This means:

The graph has 5 vertices (0–4)
And the following edges:
0–1, 0–2, 1–2, 2–3, 3–4, 4–2

Output

The algorithm prints:

The type of Eulerian traversal:

“Eulerian Circuit found” → all vertices have even degree.

“Eulerian Path found” → exactly two vertices have odd degree.

“No Eulerian Path/Circuit” → the graph does not meet Euler’s conditions.

The order of vertices visited in the Eulerian path or circuit.

Example Output:

Graph 1:
Eulerian Path found:
0 -> 1 -> 2 -> 0 -> 2 -> 4 -> 3 -> 2


Explanation:

The graph has two vertices with odd degree, so an Eulerian Path exists.

The output shows the sequence of vertices that visits every edge exactly once.

How the Algorithm Works (Simplified)

Choose a starting vertex:

If there are two vertices with odd degree → start at one of them.

If all vertices have even degree → start at any vertex.

Use a stack to keep track of the traversal path.

While there are still edges:

Move along unused edges, removing them as you go.

When no more edges can be used from a vertex, backtrack using the stack.

Once all edges are visited, reverse the recorded path to get the correct Eulerian path/circuit order.