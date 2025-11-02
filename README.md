# teori graf week 2  

# 🧩 Hierholzer’s Algorithm in Python

This project implements **Hierholzer’s Algorithm**, which is used to find an **Eulerian Path** or **Eulerian Circuit** in a graph.  
It works for **undirected graphs**, but can easily be adapted for **directed graphs** as well.

---

## 📘 Description

Hierholzer’s Algorithm is designed to find a path or circuit that visits **every edge exactly once**.

- **Eulerian Path** → a path that visits every edge exactly once (does **not** have to return to the starting vertex).  
- **Eulerian Circuit** → a path that visits every edge exactly once and **returns** to the starting vertex.

The algorithm only works if the graph satisfies **Euler’s conditions**:
- All vertices have **even degree** → Eulerian Circuit exists.  
- Exactly **two vertices have odd degree** → Eulerian Path exists.  
- Otherwise, no Eulerian Path or Circuit exists.

---

## ⚙️ How It Works

1. Choose a starting vertex:
   - If there are exactly two vertices with odd degree → start at one of them.
   - If all vertices have even degree → start at any vertex.
2. Use a **stack** to explore edges from the current vertex.
3. Remove edges as they are traversed.
4. When you reach a dead end (no more edges), backtrack by popping vertices from the stack.
5. Reverse the recorded path to get the correct Eulerian traversal.

---

## 🧠 Example Code

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


# ======== Example Usage ========

g = Graph(5)
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)
g.add_edge(3, 4)
g.add_edge(4, 2)

print("Graph 1:")
g.find_euler()
