# 问题 I：Sequence

## 题目描述

给定两个长度为 \(n\) 的序列 \(a\) 和 \(b\)，求：

$$
\max_{1\le l\le r\le n}\left(\min_{i=l}^{r}a_i\right)\left(\sum_{i=l}^{r}b_i\right)
$$

其中 `min` 表示区间最小值，`sum` 表示区间元素之和。

## 输入

第一行包含一个整数 `n`。

第二行包含 `n` 个整数，表示序列 `a`。

第三行包含 `n` 个整数，表示序列 `b`。

## 输出

输出一个整数，表示答案。

## 样例输入

```text
3
1 -1 1
1 2 3
```

## 样例输出

```text
3
```

## 提示或数据范围

对于所有测试数据：

`1 ≤ n ≤ 3 × 10^6`，`-10^6 ≤ ai, bi ≤ 10^6`。

## 题解

对数组 `a` 建立小根笛卡尔树，满足：

- 中序遍历顺序为原数组下标顺序；
- 父节点的 `a` 值不大于子节点；
- 每个节点的子树对应原数组中的一段连续区间；
- 节点 `u` 是其子树对应区间的最小值。

笛卡尔树可以用单调栈在 `O(n)` 时间内建立。

对于节点 `u`，其子树对应的序列可以写成：

```text
左子树区间 + b[u] + 右子树区间
```

一个包含位置 `u` 的连续区间，一定由以下三部分组成：

```text
左子树的一个后缀 + b[u] + 右子树的一个前缀
```

因此，对每个节点维护：

- `sum[u]`：子树区间元素和；
- `max_prefix[u]`、`min_prefix[u]`：最大、最小前缀和；
- `max_suffix[u]`、`min_suffix[u]`：最大、最小后缀和。

前缀和后缀允许为空，空区间的和为 `0`。

包含 `u` 的最大、最小区间和分别为：

$$
maxSum_u=maxSuffix_{left}+b_u+maxPrefix_{right}
$$

$$
minSum_u=minSuffix_{left}+b_u+minPrefix_{right}
$$

当 `a[u] ≥ 0` 时，应选择最大的区间和；当 `a[u] < 0` 时，应选择最小的区间和。

转移公式为：

```cpp
max_prefix[u] = max(
    max_prefix[left],
    sum[left] + b[u] + max_prefix[right]
);

min_prefix[u] = min(
    min_prefix[left],
    sum[left] + b[u] + min_prefix[right]
);

max_suffix[u] = max(
    max_suffix[right],
    max_suffix[left] + b[u] + sum[right]
);

min_suffix[u] = min(
    min_suffix[right],
    min_suffix[left] + b[u] + sum[right]
);
```

由于 `n` 最大为 `3 × 10^6`，递归遍历可能爆栈，因此使用手动栈完成后序遍历。

## 复杂度分析

时间复杂度：`O(n)`。

空间复杂度：`O(n)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;

    vector<long long> a(n + 1);

    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }

    vector<int> left_child(n + 1);
    vector<int> right_child(n + 1);

    // 计算前存放 b[i]，计算后存放子树区间和
    vector<long long> sum(n + 1);

    for (int i = 1; i <= n; i++) {
        cin >> sum[i];
    }

    // 单调栈建立小根笛卡尔树
    vector<int> st;
    st.reserve(n);

    for (int i = 1; i <= n; i++) {
        int last = 0;

        while (!st.empty() && a[st.back()] > a[i]) {
            last = st.back();
            st.pop_back();
        }

        if (!st.empty()) {
            right_child[st.back()] = i;
        }

        left_child[i] = last;
        st.push_back(i);
    }

    int root = st.front();

    vector<long long> max_prefix(n + 1);
    vector<long long> min_prefix(n + 1);
    vector<long long> max_suffix(n + 1);
    vector<long long> min_suffix(n + 1);

    long long ans = LLONG_MIN;

    // 非递归后序遍历
    st.clear();

    int cur = root;
    int last_vis = 0;

    while (cur != 0 || !st.empty()) {
        if (cur != 0) {
            st.push_back(cur);
            cur = left_child[cur];
        } else {
            int u = st.back();
            int right = right_child[u];

            if (right != 0 && last_vis != right) {
                cur = right;
            } else {
                int left = left_child[u];
                long long value = sum[u];

                long long max_sum =
                    max_suffix[left] +
                    value +
                    max_prefix[right];

                long long min_sum =
                    min_suffix[left] +
                    value +
                    min_prefix[right];

                if (a[u] >= 0) {
                    ans = max(ans, a[u] * max_sum);
                } else {
                    ans = max(ans, a[u] * min_sum);
                }

                long long left_sum = sum[left];
                long long right_sum = sum[right];

                sum[u] = left_sum + value + right_sum;

                max_prefix[u] = max(
                    max_prefix[left],
                    left_sum + value + max_prefix[right]
                );

                min_prefix[u] = min(
                    min_prefix[left],
                    left_sum + value + min_prefix[right]
                );

                max_suffix[u] = max(
                    max_suffix[right],
                    max_suffix[left] + value + right_sum
                );

                min_suffix[u] = min(
                    min_suffix[right],
                    min_suffix[left] + value + right_sum
                );

                last_vis = u;
                st.pop_back();
            }
        }
    }

    cout << ans << '\n';

    return 0;
}
```
