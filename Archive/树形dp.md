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





---
