# 🧩 Eulerian Path & Circuit Finder (Hierholzer’s and Fleury’s Algorithms)

This project implements **Hierholzer’s Algorithm** and **Fleury’s Algorithm** to find an **Eulerian Path** or **Eulerian Circuit** in an undirected graph.  
Both programs allow the user to input graph data through the console and display the resulting path or circuit as output.

---

## 📘 What is an Eulerian Path or Circuit?

- **Eulerian Path** → visits every edge exactly once (does not have to end where it started).  
- **Eulerian Circuit** → visits every edge exactly once **and** ends at the same starting vertex.

A graph has:
- **Eulerian Circuit** → if all vertices have even degree.  
- **Eulerian Path** → if exactly two vertices have odd degree.  
- **No Eulerian Path/Circuit** → if more than two vertices have odd degree.

---

## ⚙️ 1. Hierholzer’s Algorithm

### 🧠 Description
Hierholzer’s Algorithm is an efficient method to find an Eulerian path or circuit by building a closed trail and combining smaller cycles.

### 🖥️ Input Format
1. `V` — number of vertices  
2. `E` — number of edges  
3. Next `E` lines contain two integers `u v`, representing an undirected edge between vertex `u` and vertex `v`.

### 🧾 Output Format
- Prints whether the graph contains:
  - an **Eulerian Circuit**, or  
  - an **Eulerian Path**, or  
  - **no Eulerian path/circuit**.
- Then prints the traversal order of vertices in the Eulerian path or circuit.

### 💡 Example
#### Input:
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (u v):
0 1
0 2
1 2
2 3
3 4
4 2
#### Output:
Result:
Eulerian Path found:
0 -> 1 -> 2 -> 0 -> 2 -> 4 -> 3 -> 2

---

## ⚙️ 2. Fleury’s Algorithm

### 🧠 Description
Fleury’s Algorithm also finds an Eulerian path or circuit but is less efficient because it checks whether each edge is a bridge before using it.  
It is simpler to understand but slower than Hierholzer’s.

### 🖥️ Input Format
1. `V` — number of vertices  
2. `E` — number of edges  
3. Next `E` lines contain two integers `u v`, representing undirected edges.

### 🧾 Output Format
- Prints whether the graph contains:
  - an **Eulerian Circuit**, or  
  - an **Eulerian Path**, or  
  - **no Eulerian path/circuit**.
- Then lists each step of the traversal showing which edge was taken next.

### 💡 Example
#### Input:
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (u v):
0 1
0 2
1 2
2 3
3 4
4 2
#### Output:
Result:
Eulerian Path found:
2 -> 0
0 -> 1
1 -> 2
2 -> 3
3 -> 4
4 -> 2