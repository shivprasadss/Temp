#include <iostream>
#include <vector>
#include <queue>
#include <omp.h>

using namespace std;

void bfs_parallel(vector<vector<int>>& graph, int start, vector<bool>& visited) {
    queue<int> q;
    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        #pragma omp parallel for
        for (int i = 0; i < graph[node].size(); i++) {
            int neighbor = graph[node][i];
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}

int main() {
    int numNodes = 7;  // Number of nodes in the graph

    // Create an adjacency list representation of the graph
    vector<vector<int>> graph(numNodes);
    graph[0] = {1, 2};
    graph[1] = {0, 3, 4};
    graph[2] = {0, 5, 6};
    graph[3] = {1};
    graph[4] = {1};
    graph[5] = {2};
    graph[6] = {2};

    // Create a visited array to keep track of visited nodes
    vector<bool> visited(numNodes, false);

    // Perform parallel BFS from node 0
    bfs_parallel(graph, 0, visited);

    // Print the visited nodes
    for (int i = 0; i < numNodes; i++) {
        if (visited[i]) {
            cout << "Node " << i << " is visited." << endl;
        }
    }

    return 0;
}


//another test case
/*numnode = 7
0 = 1 2
1 = 0 3 4
2 = 0 5 6
3
4
5
6*/