---
aliases: []
area: 算法
project: 
date: 2023-08-28 19:51
tags: []
---
---
# 没有上司的舞会
> [!faq] 题目描述
> 某大学有 $n$ 个职员，编号为 $1\ldots n$。他们之间有从属关系，也就是说他们的关系就像一棵以校长为根的树，父结点就是子结点的直接上司。现在有个周年庆宴会，宴会每邀请来一个职员都会增加一定的快乐指数 $r_i$，但是呢，如果某个职员的直接上司来参加舞会了，那么这个职员就无论如何也不肯来参加舞会了。所以，请你编程计算，邀请哪些职员可以使快乐指数最大，求最大的快乐指数。

```cpp
#include <bits/stdc++.h>
#include <vector>
using namespace std;

// class TreeNode {
// public:
//     TreeNode(int x) {
//         val = x;
//     }

// public:
//     int val;
//     vector<TreeNode*> children;
// };

class Solution {
public:
    void dfs(int x, vector<vector<int>>& dp, vector<int>& happy, vector<vector<int>>& nodes) {
        dp[x][0] = 0;
        dp[x][1] = happy[x];
        for (auto v : nodes[x]) {
            dfs(v, dp, happy, nodes);
            dp[x][0] += max(dp[v][0], dp[v][1]);
            dp[x][1] += dp[v][0];
        }
    }

    void dance() {
        int n;
        cin >> n;
        vector<vector<int>> dp(n, vector<int>(2));
        vector<vector<int>> nodes(n);
        vector<int> happy(n);
        vector<int> is_leaf(n, 0);
        
        for (int i = 0; i < n; ++i) {
            cin >> happy[i];
        }

        int i, j;
        while (cin >> i >> j) {
            is_leaf[i - 1] = 1;
            nodes[j - 1].push_back(i - 1);
        }

        int root;
        for (int i = 0; i < n; ++i) {
            if (is_leaf[i] == 0) {
                root = i;
                break;
            }
        }
        dfs(root, dp, happy, nodes);
        cout << max(dp[root][0], dp[root][1]);

    }

    int max(int a, int b) {
        return a > b ? a : b;
    }
};

int main() {
    auto so = Solution();
    so.dance();

    return 0;
}
```

# 选课
> [!faq] 问题描述
> 在大学里每个学生，为了达到一定的学分，必须从很多课程里选择一些课程来学习，在课程里有些课程必须在某些课程之前学习，如高等数学总是在其它课程之前学习。现在有 $N$ 门功课，每门课有个学分，每门课有一门或没有直接先修课（若课程 a 是课程 b 的先修课即只有学完了课程 a，才能学习课程 b）。一个学生要从这些课程里选择 $M$ 门课程学习，问他能获得的最大学分是多少？
## 输入格式

第一行有两个整数 $N$ , $M$ 用空格隔开。( $1 \leq N \leq 300$ , $1 \leq M \leq 300$ )

接下来的 $N$ 行, 第 $I+1$ 行包含两个整数 $k_i $和 $s_i$, $k_i$ 表示第 I 门课的直接先修课，$s_i$ 表示第 I 门课的学分。若 $k_i=0$ 表示没有直接先修课（$1 \leq {k_i} \leq N$ , $1 \leq {s_i} \leq 20$）。

## 解
由于一门课最多只有一门先选课，所以每门先选课都能构成一个<u>子树</u>，现在设 0 号课程为所有子树根节点的先选课，将所有课构成一棵树。那么我们的目标就是计算得到根节点选 m 门课的最大分值。定义 $dp[i][j]$ 为第 $i$ 门课程所在节点选 $j$ 门课程的最大分值。子问题就是计算每个子树选取 $j(0 \le j \le m)$ 的分值。可以将子树计算分值看作是背包问题，因此需要扩充状态数组，将子树的子树加入状态列表，推出计算 $dp[i][k][j]$ 的转移方程为 $dp[i][k][j] = \max (dp[i][k - 1][j - c] + dp[child[k]][childsize[k]][c]) (0 \le c \le m)$，可以将第二维利用滚动数组省略，因为 k 是通过 k - 1 推导来的，因此需要反向遍历。

```cpp
#include <bits/stdc++.h>
#include <vector>
using namespace std;

class Solution {
public:
    void dfs(int x, int m, vector<vector<vector<int>>>& dp, vector<int>& score, vector<vector<int>>& classes) {
        dp[x][0][0] = 0;
        int index = 1;
        for (auto v  : classes[x]) {
            dfs(v, m, dp, score, classes);
            for (int i = 0; i <= m; ++i) {
                for (int j = 0; j <= i; ++j) {
                    dp[x][index][i] = max(dp[x][index][i], dp[x][index - 1][i - j] + dp[v][classes[v].size()][j]);
                }
            }
            ++index;
        }
        if (x != 0) {
            for (int i = m; i > 0; --i) {
                dp[x][index - 1][i] = dp[x][index - 1][i - 1] + score[x];
            }
        }
    }

    void dance() {
        int n, m;
        cin >> n >> m;
        vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(n, vector<int>(m + 1, 0)));
        vector<vector<int>> classes(n + 1);
        vector<int> score(n + 1);

        int k, s;
        for (int i = 0; i < n; ++i) {
            cin >> k >> s;
            score[i + 1] = s;
            classes[k].push_back(i + 1);
        }

        dfs(0, m, dp, score, classes);
        cout << dp[0][classes[0].size()][m] << endl;
    }

    int max(int a, int b) {
        return a > b ? a : b;
    }
};

int main() {
    auto so = Solution();
    so.dance();

    return 0;
}
```


---
