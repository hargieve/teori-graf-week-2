# teori-graf-week-2

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100

int graph[MAX][MAX];
int V, E;
char nodes[MAX];
int degree[MAX];

int getIndex(char c) {
    for (int i = 0; i < V; i++) {
        if (nodes[i] == c) return i;
    }
    return -1;
}

void DFSCount(int v, int visited[]) {
    visited[v] = 1;
    for (int i = 0; i < V; i++) {
        if (graph[v][i] && !visited[i]) {
            DFSCount(i, visited);
        }
    }
}

int isValidNextEdge(int u, int v) {
    int count = 0;
    for (int i = 0; i < V; i++)
        if (graph[u][i]) count++;

    if (count == 1) return 1;

    int visited[MAX];
    memset(visited, 0, sizeof(visited));
    int count1 = 0;
    DFSCount(u, visited);
    for (int i = 0; i < V; i++) if (visited[i]) count1++;

    graph[u][v] = graph[v][u] = 0;
    memset(visited, 0, sizeof(visited));
    int count2 = 0;
    DFSCount(u, visited);
    for (int i = 0; i < V; i++) if (visited[i]) count2++;
    graph[u][v] = graph[v][u] = 1;

    return (count1 <= count2);
}

void fleuryUtil(int u) {
    for (int v = 0; v < V; v++) {
        if (graph[u][v] && isValidNextEdge(u, v)) {
            printf("%c -> %c\n", nodes[u], nodes[v]);
            graph[u][v] = graph[v][u] = 0;
            fleuryUtil(v);
        }
    }
}

void fleury() {
    int odd = 0, start = 0;
    for (int i = 0; i < V; i++) {
        if (degree[i] % 2 != 0) {
            odd++;
            start = i;
        }
    }
    if (!(odd == 0 || odd == 2)) {
        printf("Euler path not found\n");
        return;
    }
    printf("Fleury's Algorithm Path:\n");
    fleuryUtil(start);
}

int main() {
    printf("Enter number of vertices: ");
    scanf("%d", &V);
    printf("Enter vertex labels (e.g. A B C D...): ");
    for (int i = 0; i < V; i++) scanf(" %c", &nodes[i]);

    printf("Enter number of edges: ");
    scanf("%d", &E);
    memset(graph, 0, sizeof(graph));
    memset(degree, 0, sizeof(degree));

    printf("Enter edges (format: U V)\n");
    for (int i = 0; i < E; i++) {
        char u, v;
        scanf(" %c %c", &u, &v);
        int ui = getIndex(u);
        int vi = getIndex(v);
        graph[ui][vi]++;
        graph[vi][ui]++;
        degree[ui]++;
        degree[vi]++;
    }

    fleury();

    return 0;
}
