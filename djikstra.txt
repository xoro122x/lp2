#include <iostream>
#include <limits.h>
using namespace std;

#define V 5

int minDistance(int dist[], bool visited[])
{

    int min = INT_MAX, min_index;

    for (int i = 0; i < V; i++)
        if (visited[i] == false && dist[i] <= min)
            min = dist[i], min_index = i;

    return min_index;
}

void printSolution(int dist[])
{
    cout << "Vertex \t Distance from Source" << endl;
    for (int i = 0; i < V; i++)
        cout << i << " \t\t\t\t" << dist[i] << endl;
}

void dijkstra(int graph[V][V], int src)
{
    int dist[V];

    bool visited[V];
    for (int i = 0; i < V; i++)
        dist[i] = INT_MAX, visited[i] = false;

    dist[src] = 0;

    for (int count = 0; count < V - 1; count++)
    {

        int u = minDistance(dist, visited);

        visited[u] = true;

        for (int i = 0; i < V; i++)
            if (!visited[i] && graph[u][i] && dist[u] != INT_MAX && dist[u] + graph[u][i] < dist[i])
                dist[i] = dist[u] + graph[u][i];
    }

    printSolution(dist);
}

int main()
{

    int graph[V][V] = {
        {0,1,7,0,0},
        {1,0,5,3,8},
        {7,5,0,2,0},
        {0,3,2,0,2},
        {0,8,0,2,0}
    };

    // Function call
    dijkstra(graph, 0);

    return 0;
}
