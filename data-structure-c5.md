# #1. 二叉树的建立与基本操作

```
#include <cstdio>
#include <iostream>
#include <queue>
#include <cstring>
#include <cstdlib>
#include <algorithm>

using namespace std;

// Binary tree storage representation
struct BiTNode {
    char data;
    struct BiTNode* lchild;
    struct BiTNode* rchild;
};

typedef BiTNode* BiTree;

queue<BiTree> q;
int counts = 0;

// Function prototypes
void createBiTree();
void visit(BiTree R);
void print(BiTree R, int n);
void pre_sequence(BiTree R);
void in_sequence(BiTree R);
void post_sequence(BiTree R);

// Functions for swapping
void swap_print(BiTree R, int n);
void swap_pre_sequence(BiTree R);
void swap_in_sequence(BiTree R);
void swap_post_sequence(BiTree R);

int main() {
    BiTree bit;
    bit = (BiTree)malloc(sizeof(BiTNode));
    q.push(bit);

    // Create binary tree
    createBiTree();

    // Print binary tree
    printf("BiTree\n");
    print(bit, 0);

    // Pre-order traversal
    printf("pre_sequence  : ");
    pre_sequence(bit);
    printf("\n");

    // In-order traversal
    printf("in_sequence   : ");
    in_sequence(bit);
    printf("\n");

    // Post-order traversal
    printf("post_sequence : ");
    post_sequence(bit);
    printf("\n");

    // Count number of leaf nodes
    printf("Number of leaf: %d\n", counts);

    // Swap and print the binary tree
    printf("BiTree swapped\n");
    swap_print(bit, 0);

    printf("pre_sequence  : ");
    swap_pre_sequence(bit);
    printf("\n");

    printf("in_sequence   : ");
    swap_in_sequence(bit);
    printf("\n");

    printf("post_sequence : ");
    swap_post_sequence(bit);
    printf("\n");

    return 0;
}

void createBiTree() {
    char c;
    BiTree T, N;

    while (!q.empty()) {
        scanf("%c", &c);

        if (c == '\n') {
            break;
        }

        T = q.front();
        q.pop();
        T->data = c;

        if (c != '#') {
            N = (BiTree)malloc(sizeof(BiTNode));
            T->lchild = N;
            q.push(N);
            createBiTree();

            N = (BiTree)malloc(sizeof(BiTNode));
            T->rchild = N;
            q.push(N);
            createBiTree();
        }
    }
}

void visit(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        cout << R->data;
    }
}

void print(BiTree R, int n) {
    if (R->data != '#' && R->data != '\0') {
        int i = 1;

        while (i <= n) {
            cout << "    ";
            i++;
        }

        visit(R);
        cout << endl;
        n++;
        print(R->lchild, n);
        print(R->rchild, n);
    }
}

void pre_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        visit(R);
        pre_sequence(R->lchild);
        pre_sequence(R->rchild);
    }
}

void in_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        in_sequence(R->lchild);

        if ((R->lchild->data == '#' || R->lchild->data == '\0') && (R->rchild->data == '#' || R->rchild->data == '\0')) {
            counts++;
        }

        visit(R);
        in_sequence(R->rchild);
    }
}

void post_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        post_sequence(R->lchild);
        post_sequence(R->rchild);
        visit(R);
    }
}

void swap_print(BiTree R, int n) {
    if (R->data != '#' && R->data != '\0') {
        int i = 1;

        while (i <= n) {
            cout << "    ";
            i++;
        }

        visit(R);
        cout << endl;
        n++;
        swap_print(R->rchild, n);
        swap_print(R->lchild, n);
    }
}

void swap_pre_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        visit(R);
        swap_pre_sequence(R->rchild);
        swap_pre_sequence(R->lchild);
    }
}

void swap_in_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        swap_in_sequence(R->rchild);
        visit(R);
        swap_in_sequence(R->lchild);
    }
}

void swap_post_sequence(BiTree R) {
    if (R->data != '#' && R->data != '\0') {
        swap_post_sequence(R->rchild);
        swap_post_sequence(R->lchild);
        visit(R);
    }
}
```

# 2.二叉树遍历序列还原

```
#include <bits/stdc++.h>
using namespace std;

struct Node {
    char data;
    Node* left;
    Node* right;
};

Node* NewNode(char data) {
    Node* node = new Node;
    node->data = data;
    node->left = NULL;
    node->right = NULL;
    return node;
}

int Search(string in, int start, int end, char value) {
    for (int i = start; i <= end; i++) {
        if (in[i] == value) {
            return i;
        }
    }
    return -1;
}

Node* BuildTree(string in, string post, int inStart, int inEnd) {
    static int postIndex = inEnd;
    if (inStart > inEnd) {
        return NULL;
    }

    Node* node = NewNode(post[postIndex--]);

    if (inStart == inEnd) {
        return node;
    }

    int inIndex = Search(in, inStart, inEnd, node->data);

    node->right = BuildTree(in, post, inIndex + 1, inEnd);
    node->left = BuildTree(in, post, inStart, inIndex - 1);

    return node;
}

void PrintLevelOrder(Node* root) {
    if (root == NULL)
        return;

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        int nodeCount = q.size();

        while (nodeCount > 0) {
            Node* node = q.front();
            cout << node->data;
            q.pop();

            if (node->left != NULL)
                q.push(node->left);

            if (node->right != NULL)
                q.push(node->right);

            nodeCount--;
        }
    }
    cout << endl;
}

int main() {
    string in, post;
    cin >> in >> post;

    int len = in.size();
    Node* root = BuildTree(in, post, 0, len - 1);

    PrintLevelOrder(root);

    return 0;
}
```

# 3. 二叉哥的二叉树

```
#include <math.h>
#include <stdio.h>
#include <iostream>
#include <cstdlib>
using namespace std;

int num = 0;

void count(int n, int m) {
    if (n < m + 1) {
        num += 0;
        return;
    } else if (n == m + 1) {
        num += 1;
        return;
    } else {
        n = n - m - 1;
        num += 1;
        int layer = 1;

        if (n == 0 || layer > m)
            return;

        int need;
        need = pow(2, layer) - 1;

        while (n - need > 0) {
            num += pow(2, layer - 1);
            layer++;
            n -= need;
            need = pow(2, layer) - 1;
            if (layer > m)
                return;
        }

        if (n - need == 0) {
            num += pow(2, layer - 1);
            return;
        } else
            count(n, layer - 1);
    }
}

int main() {
    int n, m, t;
    cin >> t;
    while (t--) {
        cin >> n >> m;
        num = 0;
        count(n, m);
        cout << num << endl;
    }

    return 0;
}
```

# 4. 树的建立与基本操作

```
#include <stdio.h>

int main() {
    char c, ab[100];
    int num = 0, level[100], degree[100] = {0}, p[100] = {0};
    int depth = -1, i, j, max = 0;

    while (1) {
        c = getchar();
        if (c == '\n')
            break;

        switch (c) {
            case '(':
                depth++;
                break;
            case ')':
                depth--;
                break;
            case ',':
                break;
            default:
                num++;
                ab[num] = c;
                level[num] = depth;
                break;
        }
    }

    for (i = 1; i <= num; i++) {
        for (j = 0; j < level[i]; j++)
            printf("    ");
        printf("%c\n", ab[i]);
    }

    for (i = 1; i <= num; i++) {
        for (j = i + 1; j <= num; j++) {
            if (level[j] == level[i])
                break;
            if (level[j] == level[i] + 1)
                degree[i]++;
        }
    }

    for (i = 1; i <= num; i++) {
        if (degree[i] > max)
            max = degree[i];
    }

    for (i = 1; i <= num; i++)
        p[degree[i]]++;

    printf("Degree of tree: %d\n", max);

    for (i = 0; i <= max; i++)
        printf("Number of nodes of degree %d: %d\n", i, p[i]);

    return 0;
}
```

# 5. 计算 WPL——New

```
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <fstream>
#include <queue>

using namespace std;

typedef struct {
    int weight;
    int parent, lchild, rchild;
    int depth;
} HTNode, *HuffmanTree;

void Select(HuffmanTree HT, int n, int *s1, int *s2) {
    int i;
    *s1 = n;
    for (i = 1; i < n; i++) {
        if (HT[i].parent) continue;
        if (HT[*s1].weight > HT[i].weight) *s1 = i;
    }
    for (i = 1; i <= n; i++) {
        if (HT[i].parent) continue;
        if (*s1 == i) continue;
        else {
            *s2 = i;
            break;
        }
    }
    for (i = 1; i <= n; i++) {
        if (HT[i].parent || i == *s1) continue;
        if (HT[*s2].weight > HT[i].weight) *s2 = i;
    }
}

void Setdepth(HuffmanTree HT, HTNode *ht) {
    if (!ht->lchild && !ht->rchild)
        ht->depth++;
    else {
        ht->depth++;
        HTNode *p;
        if (ht->lchild) {
            p = &HT[ht->lchild];
            Setdepth(HT, p);
        }
        if (ht->rchild) {
            p = &HT[ht->rchild];
            Setdepth(HT, p);
        }
    }
}

int TreeCreat(HuffmanTree &HT, int *w, int n) {
    if (n <= 1) return 0;
    int m = 2 * n - 1;
    HT = (HuffmanTree)malloc(sizeof(HTNode) * (m + 1));
    HuffmanTree p;
    int *q;
    int i;
    for (p = HT + 1, q = w + 1, i = 1; i <= n; i++, p++, q++) {
        *p = {*q, 0, 0, 0, 1};
    }
    int s1, s2;
    for (i = n + 1; i < m; i++, p++) {
        Select(HT, i - 1, &s1, &s2);
        HT[s1].parent = i;
        HT[s2].parent = i;
        HT[i].lchild = s1;
        HT[i].rchild = s2;
        HT[i].parent = 0;
        HT[i].depth = 1;
        HT[i].weight = HT[s1].weight + HT[s2].weight;
        Setdepth(HT, &HT[i]);
    }
    return 0;
}

void Treeprint(HuffmanTree HT, int n) {
    int i;
    for (i = 1; i <= n; i++) {
        cout << HT[i].weight << endl;
        cout << HT[i].depth << endl << endl;
    }
}

int WPLcalcu(HuffmanTree HT, int n) {
    int sum = 0;
    int i;
    for (i = 1; i <= n; i++) {
        sum += HT[i].weight * HT[i].depth;
    }
    return sum;
}

int main() {

    int *w;
    int n;
    int i;
    int value = 0;

    cin >> n;
    w = (int *)malloc(sizeof(int) * (n + 1));

    for (i = 1; i <= n; i++) {
        cin >> w[i];
    }

    HuffmanTree HT;

    TreeCreat(HT, w, n);

    value = WPLcalcu(HT, n);
    cout << "WPL=" << value << endl;
    return 0;
}
```

# 7. 回溯：最优装载问题

```
#include <bits/stdc++.h>
using namespace std;

const int maxw = 1e5 + 10;
const int maxn = 105;

int n, c, w[maxn], ans, used[maxn], ans_used[maxn];

void Dfs(int x, int sum) {
    if (sum > c) return;
    if (x == n + 1) {
        if (sum > ans) {
            for (int i = 1; i <= n; ++i) {
                ans_used[i] = used[i];
            }
            ans = sum;
        }
        return;
    }

    // First, search for using item x (1), then without using it (0).
    used[x] = 1;
    Dfs(x + 1, sum + w[x]);
    used[x] = 0;
    Dfs(x + 1, sum);
}

int main() {
    scanf("%d %d", &c, &n);

    for (int i = 1; i <= n; ++i) {
        scanf("%d", w + i);
    }

    Dfs(1, 0);

    // Print the result
    printf("%d\n", ans);

    for (int i = 1; i <= n; ++i) {
        if (ans_used[i]) {
            printf("%d ", i);
        }
    }

    printf("\n");

    return 0;
}
```

# 8. 皇后的控制力

```
#include <cstdbool>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <math.h>
#include <queue>

using namespace std;

int m = 0;
int n = 0;
int tot = 0;
int* row = NULL;
int* column = NULL;
int* slash = NULL;
int* reslash = NULL;

int judge() {
    int i, j;
    for(i = 1; i <= n; i++) {
        if(row[i] == 999) {
            for(j = 1; j <= n; j++) {
                if(column[j] == 999) {
                    if(!(reslash[i + j] == 1 || slash[n + i - j + 1] == 1)) {
                        return 0;
                    }
                }
            }
        }
    }
    return 1;
}

void traceback(bool flag, int level, int num) {

    if(level > n) {
        if(num != m)
            return;
        if(judge())
            tot++;
        return;
    }

    if(n - level + num < m)
        return;
    if(!flag) {
        traceback(false, level + 1, num);
        traceback(true, level + 1, num + 1);
    }
    else {
        for(register int i = 1; i <= n; i++) {
            if(column[i] == 999 && reslash[level + i] == 999 && slash[n + level - i + 1] == 999) {
                row[level] = level;
                column[i] = i;
                reslash[level + i] = 1;
                slash[n + level - i + 1] = 1;
                traceback(false, level + 1, num);
                traceback(true, level + 1, num + 1);
                row[level] = 999;
                column[i] = 999;
                reslash[level + i] = 999;
                slash[n + level - i + 1] = 999;
            }
        }
    }
}

int main() {
    cin >> n;
    cin >> m;

    column = new int[n + 1];
    row = new int[n + 1];
    slash = new int[2 * n + 1];
    reslash = new int[2 * n + 1];

    for(register int i = 0; i <= n; i++) {
        column[i] = 999;
        row[i] = 999;
    }
    for(register int i = 0; i < 2 * n + 1; i++) {
        slash[i] = 999;
        reslash[i] = 999;
    }

    traceback(false, 1, 0);
    traceback(true, 1, 1);
    cout << tot << endl;

    return 0;
}
```

# H4. 回溯：木板墙（选做）

```
#include <iostream>
#include <stack>
#include <algorithm>
using namespace std;

struct Node
{
    int hight;
    int pre;
    int pos;
};

int main()
{
    int board_num;
    stack<Node> St;

    while (~scanf("%d", &board_num) && board_num)
    {
        long long sum_area = 0;

        for (int i = 1; i <= board_num; i++)
        {
            int temp_hight;
            int pre = 0;

            scanf("%d", &temp_hight);

            while (!St.empty() && St.top().hight > temp_hight)
            {
                Node temp = St.top();
                St.pop();
                pre += temp.pre + 1;
                sum_area = max(sum_area, (long long)temp.hight * (i - temp.pos + temp.pre));
            }

            Node push_node{temp_hight, pre, i};
            St.push(push_node);
        }

        while (!St.empty())
        {
            Node temp = St.top();
            St.pop();
            sum_area = max(sum_area, (long long)temp.hight * (board_num + 1 - temp.pos + temp.pre));
        }

        printf("%lld\n", sum_area);
    }

    return 0;
}
```
