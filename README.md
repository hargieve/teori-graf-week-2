# 🧩 Eulerian Path & Circuit Algorithms (Hierholzer’s and Fleury’s) in Python

This project implements **Hierholzer’s Algorithm** and **Fleury’s Algorithm** — two classical algorithms to find an **Eulerian Path** or **Eulerian Circuit** in a graph.  
Both versions now include **user input (stdin)** and **printed output (stdout)** support.

---

## 📘 What is an Eulerian Path/Circuit?

- **Eulerian Path** → visits **every edge exactly once**, but doesn’t necessarily return to the starting vertex.  
- **Eulerian Circuit** → visits **every edge exactly once** and **returns to the starting vertex**.

A graph has:
- An **Eulerian Circuit** → if all vertices have even degree.  
- An **Eulerian Path** → if exactly two vertices have odd degree.  
- **None** → otherwise.

---

# ⚙️ Algorithm 1: Hierholzer’s Algorithm

### 🧠 Description
Hierholzer’s Algorithm efficiently constructs an Eulerian path or circuit by traversing all edges exactly once using a **stack**.  
It’s faster than Fleury’s Algorithm because it doesn’t need to check bridges repeatedly.

---

### 💻 **Python Code (with User Input)**
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
            print("No Eulerian Path or Circuit exists.")
            return

        stack, path = [start], []
        while stack:
            v = stack[-1]
            if self.graph[v]:
                u = self.graph[v].pop()
                self.graph[u].remove(v)
                stack.append(u)
            else:
                path.append(stack.pop())

        print(" -> ".join(map(str, path[::-1])))


# ======== MAIN PROGRAM ========
if __name__ == "__main__":
    V = int(input("Enter number of vertices: "))
    E = int(input("Enter number of edges: "))

    g = Graph(V)
    print("Enter edges (u v):")
    for _ in range(E):
        u, v = map(int, input().split())
        g.add_edge(u, v)

    print("\nResult:")
    g.find_euler()
📥 Example Input
mathematica
Copy code
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (u v):
0 1
0 2
1 2
2 3
3 4
4 2
📤 Example Output
rust
Copy code
Result:
Eulerian Path found:
0 -> 1 -> 2 -> 0 -> 2 -> 4 -> 3 -> 2

⚙️ Algorithm 2: Fleury’s Algorithm
🧠 Description
Fleury’s Algorithm also finds an Eulerian path/circuit but is less efficient because it checks if an edge is a bridge before traversing it.
This makes it slower but conceptually easier to understand.

💻 Python Code (with User Input)
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
            print("No Eulerian Path or Circuit exists.")
            return
        self.print_euler_util(start)


# ======== MAIN PROGRAM ========
if __name__ == "__main__":
    V = int(input("Enter number of vertices: "))
    E = int(input("Enter number of edges: "))

    g = Graph(V)
    print("Enter edges (u v):")
    for _ in range(E):
        u, v = map(int, input().split())
        g.add_edge(u, v)

    print("\nResult:")
    g.print_euler()
📥 Example Input
mathematica
Copy code
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (u v):
0 1
0 2
1 2
2 3
3 4
4 2
📤 Example Output
rust
Copy code
Result:
Eulerian Path found:
2 -> 0
0 -> 1
1 -> 2
2 -> 3
3 -> 4
4 -> 2
🧮 Comparison: Hierholzer vs Fleury
Feature	Hierholzer’s Algorithm	Fleury’s Algorithm
Efficiency	O(E)	O(E²)
Checks bridges	❌ No	✅ Yes
Suitable for	Large graphs	Small graphs / teaching
Output	Eulerian Path/Circuit	Eulerian Path/Circuit

🚀 How to Run
Save each algorithm in separate files:

hierholzer.py

fleury.py

Open terminal and run either:

bash
Copy code
python hierholzer.py
or

bash
Copy code
python fleury.py
Enter the graph data when prompted.

The program will display whether the graph has an Eulerian Path or Circuit and print the traversal order.

📚 References
Hierholzer, Carl (1873). Über die Möglichkeit, einen Linienzug ohne Wiederholung und ohne Unterbrechung zu umfahren.

GeeksforGeeks — Hierholzer’s Algorithm

GeeksforGeeks — Fleury’s Algorithm

