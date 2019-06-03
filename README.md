# LAB-4
LAB4 361
struct Edge
{
    int source, dest, weight;
};

// Recurive Function to print path of given vertex v from source vertex
void printPath(vector<int> const &parent, int v)
{
    if (v < 0)
        return;

    printPath(parent, parent[v]);
    cout << v << " ";
}

// Function to run Bellman-Ford algorithm from given source
void BellmanFord(vector<Edge> const &edges, int source, int N)
{
    // count number of edges present in the graph
    int E = edges.size();

    // distance[] and parent[] stores shortest-path (least cost/path)
    // information. Initally all vertices except source vertex have
    // a weight of infinity and a no parent

    vector<int> distance (N, INT_MAX);
    distance[source] = 0;

    vector<int> parent (N, -1);

    int u, v, w, k = N;

    // Relaxation step (run V-1 times)
    while (--k)
    {
        for (int j = 0; j < E; j++)
        {
            // edge from u to v having weight w
            u = edges[j].source, v = edges[j].dest;
            w = edges[j].weight;

            // if the distance to the destination v can be
            // shortened by taking the edge u-> v
            if (distance[u] != INT_MAX && distance[u] + w < distance[v])
            {
                // update distance to the new lower value
                distance[v] = distance[u] + w;

                // set v's parent as u
                parent[v] = u;
            }
        }
    }

    // Run Relaxation step once more for Nth time to
    // check for negative-weight cycles
    for (int i = 0; i < E; i++)
    {
        // edge from u to v having weight w
        u = edges[i].source, v = edges[i].dest;
        w = edges[i].weight;

        // if the distance to the destination u can be
        // shortened by taking the edge u-> v
        if (distance[u] != INT_MAX && distance[u] + w < distance[v])
        {
            cout << "Negative Weight Cycle Found!!";
            return;
        }
    }

    for (int i = 0; i < N; i++)
    {
        cout << "Distance of vertex " << i << " from the source is "
             << setw(2) << distance[i] << ". It's path is [ ";
        printPath(parent, i); cout << "]" << '\n';
    }
}

// main function
int main()
{
    // vector of graph edges as per above diagram
    vector<Edge> edges =
    {
        // (x, y, w) -> edge from x to y having weight w
        { 0, 1, -2 }, { 0, 3, 3 }, { 1, 2, 1 }, { 2, 13, -3 },
        { 2, 12, 3 }, { 3, 4, 2 }, { 3, 5, 6 }, { 3, 6, -1 },
        { 3, 13, -1 }, { 4, 5, 3 }, { 5, 7, -2 }, { 6, 7, 1 },
        { 7, 8, -4 }, { 7, 10, -1 }, { 8, 9, 2 }, { 9, 10, 3 },
        { 6, 9, 3 }, { 10,11, 2 }, { 11, 12, -4 }, { 9, 13, 5 },
        { 12, 13, 8 }


    };

    // Set maximum number of nodes in the graph
    int N = 14;

    // let source be vertex 0
    int source = 0;
    cout << " I used Numbers instead of the alphabit it goes like this : "<<endl;
    cout << " a = 0, b = 1, c = 2, d = 3, e = 4, f = 5, g = 6, h = 7, i = 8, j = 9, k = 10, l = 11, m = 12, n = 13. "<<endl;
    cout << endl << endl;
    // run Bellman-Ford algorithm from given source
    BellmanFord(edges, source, N);

    return 0;
}
