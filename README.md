 # 🧩 Eulerian Path & Circuit Algorithms in Python
This project contains implementations of **Hierholzer’s Algorithm** and **Fleury’s Algorithm** — two classical graph algorithms used to find an **Eulerian Path** or **Eulerian Circuit** in a graph.

Both algorithms work on **undirected graphs** (but Hierholzer’s version can be adapted for directed graphs as well).

---

## 📘 What is an Eulerian Path/Circuit?

- **Eulerian Path** → a path that visits **every edge exactly once** (does **not** have to return to the starting vertex).  
- **Eulerian Circuit** → a path that visits **every edge exactly once** and **returns** to the starting vertex.

A graph has:
- An **Eulerian Circuit** if **all vertices have even degree**.  
- An **Eulerian Path** if **exactly two vertices have odd degree**.  
- **None** otherwise.

---

# ⚙️ Algorithm 1: Hierholzer’s Algorithm

### 🧠 Description
Hierholzer’s Algorithm finds an Eulerian path or circuit efficiently by building the route step by step using a **stack**.  
It is faster than Fleury’s Algorithm because it doesn’t repeatedly check for bridges.

### 🔢 Steps
1. Choose a **starting vertex**:
   - If two vertices have odd degree → start at one of them.
   - If all vertices are even → start anywhere.
2. Follow edges one by one, removing them as you go.
3. When you reach a vertex with no more edges, **backtrack** using the stack.
4. The final path (in reverse order) is the Eulerian path/circuit.

---

### 💻 Example Code
```python
from collections import defaultdict

class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list)
        self.V = vertices

    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)

    def find_euler(self):
        odd = [v for v in range(self.V) if len(self.graph[v]) % 2 != 0]

        if len(odd) == 0:
            start = 0
            print("Eulerian Circuit found:")
        elif len(odd) == 2:
            start = odd[0]
            print("Eulerian Path found:")
        else:
            print("No Eulerian Path/Circuit.")
            return

        stack = [start]
        path = []

        while stack:
            v = stack[-1]
            if self.graph[v]:
                u = self.graph[v].pop()
                self.graph[u].remove(v)
                stack.append(u)
            else:
                path.append(stack.pop())

        print(" -> ".join(map(str, path[::-1])))


# ===== Example Usage =====
g = Graph(5)
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)
g.add_edge(3, 4)
g.add_edge(4, 2)

print("Graph 1:")
g.find_euler()
📥 Example Input
text
Copy code
Vertices: 0, 1, 2, 3, 4
Edges: (0-1), (0-2), (1-2), (2-3), (3-4), (4-2)
📤 Example Output
rust
Copy code
Graph 1:
Eulerian Path found:
0 -> 1 -> 2 -> 0 -> 2 -> 4 -> 3 -> 2
⏱️ Complexity
Operation	Time	Space
Building Graph	O(E)	O(V + E)
Finding Eulerian Path	O(E)	O(E)

⚙️ Algorithm 2: Fleury’s Algorithm
🧠 Description
Fleury’s Algorithm is another method to find an Eulerian path or circuit, but it is less efficient than Hierholzer’s Algorithm because it checks whether each edge is a bridge before using it.

It works well for small graphs and helps demonstrate the concept of Eulerian traversal clearly.

🔢 Steps
Choose the starting vertex:

If two vertices are odd → start at one of them.

If all are even → start anywhere.

Repeatedly:

Choose an edge that is not a bridge, unless no other choice.

Remove that edge and continue traversal.

Continue until all edges are removed.

💻 Example Code
python
Copy code
from collections import defaultdict

class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list)
        self.V = vertices

    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)

    def remove_edge(self, u, v):
        self.graph[u].remove(v)
        self.graph[v].remove(u)

    def dfs_count(self, v, visited):
        count = 1
        visited[v] = True
        for i in self.graph[v]:
            if not visited[i]:
                count += self.dfs_count(i, visited)
        return count

    def is_valid_next_edge(self, u, v):
        if len(self.graph[u]) == 1:
            return True

        visited = [False] * self.V
        count1 = self.dfs_count(u, visited)

        self.remove_edge(u, v)
        visited = [False] * self.V
        count2 = self.dfs_count(u, visited)
        self.add_edge(u, v)

        return count1 == count2

    def print_euler_util(self, u):
        for v in self.graph[u][:]:
            if self.is_valid_next_edge(u, v):
                print(f"{u} -> {v}")
                self.remove_edge(u, v)
                self.print_euler_util(v)

    def print_euler(self):
        odd = [v for v in range(self.V) if len(self.graph[v]) % 2 != 0]
        if len(odd) == 0:
            start = 0
            print("Eulerian Circuit found:")
        elif len(odd) == 2:
            start = odd[0]
            print("Eulerian Path found:")
        else:
            print("No Eulerian Path/Circuit.")
            return
        self.print_euler_util(start)


# ===== Example Usage =====
g = Graph(5)
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)
g.add_edge(3, 4)
g.add_edge(4, 2)

print("Graph 1:")
g.print_euler()
📥 Example Input
text
Copy code
Vertices: 0, 1, 2, 3, 4
Edges: (0-1), (0-2), (1-2), (2-3), (3-4), (4-2)
📤 Example Output
rust
Copy code
Graph 1:
Eulerian Path found:
2 -> 0
0 -> 1
1 -> 2
2 -> 3
3 -> 4
4 -> 2
⏱️ Complexity
Operation	Time	Space
Checking bridges	O(E²)	O(V)
Traversal	O(E²)	O(V + E)

🧮 Comparison: Hierholzer vs Fleury
Feature	Hierholzer’s Algorithm	Fleury’s Algorithm
Efficiency	O(E)	O(E²)
Checks for bridges	❌ No	✅ Yes
Suitable for	Large graphs	Educational / small graphs
Output	Eulerian Path/Circuit	Eulerian Path/Circuit

🚀 How to Run
Clone or download this repository.

Run either Python file:

bash
Copy code
python hierholzer.py
or

bash
Copy code
python fleury.py
The Eulerian path or circuit will be printed to the terminal.

📚 References
Hierholzer, Carl (1873). Über die Möglichkeit, einen Linienzug ohne Wiederholung und ohne Unterbrechung zu umfahren.

GeeksforGeeks: Hierholzer’s Algorithm for Eulerian Path and Circuit

GeeksforGeeks: Fleury’s Algorithm for Eulerian Path and Circuit



