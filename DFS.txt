#include <iostream>
#include <vector>
#include <stack>
#include <omp.h>

using namespace std;

void dfs_parallel(vector<vector<int>>& graph, int start, vector<bool>& visited) {
    stack<int> stack;
    stack.push(start);

    while (!stack.empty()) {
        int node = stack.top();
        stack.pop();

        if (!visited[node]) {
            visited[node] = true;

            #pragma omp parallel for
            for (int i = 0; i < graph[node].size(); i++) {
                int neighbor = graph[node][i];
                if (!visited[neighbor]) {
                    stack.push(neighbor);
                }
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

    // Perform parallel DFS from node 0
    dfs_parallel(graph, 0, visited);

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
0= 1 2
1= 0 3 4
2 =0 5 6
3
4
5
6*/
