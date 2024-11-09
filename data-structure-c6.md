# 1. 图的广度优先遍历

```
#include <iostream>
#include <vector>
#include <queue>
#include <cstdlib>

using namespace std;

const int MAXNUM = 1000;
int visited[MAXNUM];

struct Edge {
    int ver;
    Edge* next;
};

struct Vertex {
    char data;
    Edge* firstEdge;
};

struct Graph {
    Vertex adjList[MAXNUM];
    int verNum = 0, edgeNum = 0;
};

void CreateGraph(Graph &G) {
    Edge* e = nullptr;
    int i, j;
    char v;

    cin >> v;
    while (v != '*') {
        G.adjList[G.verNum].data = v;
        G.adjList[G.verNum].firstEdge = nullptr;
        cin >> v;
        G.verNum++;
    }

    scanf("%d,%d", &i, &j);
    while (i != -1 && j != -1) {
        e = new Edge;
        e->ver = j;
        e->next = G.adjList[i].firstEdge;
        G.adjList[i].firstEdge = e;

        e = new Edge;
        e->ver = i;
        e->next = G.adjList[j].firstEdge;
        G.adjList[j].firstEdge = e;

        scanf("%d,%d", &i, &j);
        G.edgeNum++;
    }
}

void BFS(Graph &G, int vt) {
    queue<int> Q;
    Edge* e;
    Q.push(vt);
    visited[vt] = 1;

    while (!Q.empty()) {
        vt = Q.front();
        cout << G.adjList[vt].data;
        Q.pop();
        e = G.adjList[vt].firstEdge;

        while (e != nullptr) {
            if (visited[e->ver] == 0) {
                visited[e->ver] = 1;
                Q.push(e->ver);
            }
            e = e->next;
        }
    }

    for (int i = 0; i < G.verNum; i++) {
        if (visited[i] == 0) {
            BFS(G, i);
        }
    }
}

int main() {
    Graph G;
    CreateGraph(G);

    cout << "the ALGraph is\n";
    for (int i = 0; i < G.verNum; i++) {
        if (G.adjList[i].firstEdge != nullptr)
            cout << G.adjList[i].data << " ";
        else
            cout << G.adjList[i].data;

        Edge* e = G.adjList[i].firstEdge;
        while (e != nullptr) {
            if (e->next != nullptr)
                cout << e->ver << " ";
            else
                cout << e->ver;
            e = e->next;
        }
        cout << "\n";
    }

    for (int i = 0; i < G.verNum; i++) {
        visited[i] = 0;
    }

    cout << "the Breadth-First-Seacrh list:";
    BFS(G, 0);
    cout << endl;

    return 0;
}
```

# 2. 网络楼楼通

```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <algorithm>

using namespace std;

struct node {
    int x;
    int y;
    int dis;
} road[5500];

bool operator<(const node &A, const node &B) {
    return A.dis < B.dis;
}

int n = 0, m = 0;
int tree[1010];

void init() {
    for (int i = 0; i <= n; i++)
        tree[i] = i;
}

int find(int n) {
    if (tree[n] == n)
        return n;
    else
        return tree[n] = find(tree[n]);
}

int judge(int x, int y) {
    int a = find(x);
    int b = find(y);
    if (a != b) {
        tree[a] = b;
        return 1;
    } else {
        return 0;
    }
}

void kruskal() {
    int side = 0;
    int sum = 0;
    init();
    sort(road, road + m);

    for (int i = 0; i < m; i++) {
        if (judge(road[i].x, road[i].y)) {
            side++;
            sum += road[i].dis;
        }
        if (side == n - 1)
            break;
    }

    if (side != n - 1)
        cout << "-1" << endl;
    else
        cout << sum << endl;
}

int main() {
    cin >> n >> m;

    for (int i = 0; i < m; i++)
        cin >> road[i].x >> road[i].y >> road[i].dis;

    kruskal();

    return 0;
}
```

# 3. 求单点的最短路径

```
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <algorithm>

using namespace std;

const int inf = 0x3f3f3f;
int road[1010][1010], d[1010];
bool vis[1010];
int n, m, num = 0;

void dijkstra(int s) {
    for (int i = 0; i < n; i++) {
        d[i] = road[s][i];
        vis[i] = false;
    }
    d[s] = 0;
    vis[s] = true;

    for (int i = 0; i < n; i++) {
        int temp = inf, v;
        for (int j = 0; j < n; j++) {
            if (!vis[j] && temp > d[j]) {
                temp = d[j];
                v = j;
            }
        }
        if (temp == inf)
            break;
        vis[v] = true;
        for (int j = 0; j < n; j++) {
            if (!vis[j] && d[v] + road[v][j] < d[j]) {
                d[j] = d[v] + road[v][j];
            }
        }
    }
}

int main() {
    int x, y, dis;
    char a, b, c;
    scanf("%d,%d,%c", &m, &n, &c);
    getchar(); // Read newline character

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            road[i][j] = (i == j) ? 0 : inf;
        }
    }

    for (int i = 0; i < n; i++) {
        scanf("<%c,%c,%d>", &a, &b, &dis);
        getchar();
        x = a - 'a';
        y = b - 'a';
        if (road[x][y] > dis)
            road[x][y] = dis;
    }

    num = c - 'a';
    dijkstra(num);

    for (int i = 0; i < m; i++) {
        printf("%c:%d\n", i + 'a', d[i]);
    }

    return 0;
}
```
