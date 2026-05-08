# 3660. Jump Game IX

<div align="center">

# 🚀 Jump Game IX

### LeetCode Medium

<img src="https://img.shields.io/badge/Array-Problem-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Greedy-Approach-green?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Graph-Thinking-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/C++-Solutions-red?style=for-the-badge"/>

</div>

---

# 📌 Problem Statement

You are given an integer array `nums`.

From any index `i`, you can jump to another index `j` under the following rules:

* Jump to index `j` where `j > i` is allowed only if:

```math
nums[j] < nums[i]
```

* Jump to index `j` where `j < i` is allowed only if:

```math
nums[j] > nums[i]
```

For each index `i`, find the maximum value reachable by following any sequence of valid jumps.

Return an array `ans` where:

```text
ans[i] = maximum reachable value starting from i
```

---

# 🧠 Understanding the Problem

The jumps depend on:

* direction
* value comparison
* chain connections

Even if two indices are not directly connected, they may become connected through intermediate jumps.

---

# 🔍 Example Visualization

## Input

```text
nums = [2,3,1]
```

---

## Valid Jumps

```text
Index:   0   1   2
Value:   2   3   1

0 -----> 2     because 1 < 2
2 -----> 1     because 3 > 1
```

So:

```text
0 ↔ 2 ↔ 1
```

All indices become connected.

Maximum reachable value:

```text
3
```

Final Answer:

```text
[3,3,3]
```

---

# 🚀 Approaches

1. Brute Force DFS
2. Better Approach (DSU / Union Find)
3. Optimal Greedy Approach

---

# 🥉 Approach 1 — Brute Force DFS

---

# 💡 Idea

For every index:

* try all possible jumps
* recursively explore reachable indices
* store maximum reachable value

This is simple graph traversal.

---

# 📚 Algorithm

## For every index i:

1. Start DFS from i
2. Visit all reachable indices
3. Track maximum value
4. Store answer

---

# 🔄 DFS Visualization

## nums = [2,3,1]

Starting from index 0:

```text
0 -> 2 -> 1
```

Reachable values:

```text
2,1,3
```

Maximum:

```text
3
```

---

# 💻 Brute Force C++ Code

```cpp
class Solution {
public:

    void dfs(int node, vector<int>& nums,
             vector<bool>& vis, int& maxi) {

        vis[node] = true;

        maxi = max(maxi, nums[node]);

        int n = nums.size();


        // Forward jumps
        for (int j = node + 1; j < n; j++) {

            if (nums[j] < nums[node] && !vis[j]) {
                dfs(j, nums, vis, maxi);
            }
        }


        // Backward jumps
        for (int j = node - 1; j >= 0; j--) {

            if (nums[j] > nums[node] && !vis[j]) {
                dfs(j, nums, vis, maxi);
            }
        }
    }


    vector<int> maxValue(vector<int>& nums) {

        int n = nums.size();

        vector<int> ans(n);


        for (int i = 0; i < n; i++) {

            vector<bool> vis(n, false);

            int maxi = nums[i];

            dfs(i, nums, vis, maxi);

            ans[i] = maxi;
        }

        return ans;
    }
};
```

---

# ⏱️ Time Complexity

For every index we may traverse entire array.

```text
O(n²)
```

---

# 📦 Space Complexity

```text
O(n)
```

for recursion + visited array.

---

# ❌ Why Brute Force is Bad?

* repeated traversal
* repeated computations
* inefficient for large constraints

Constraint:

```text
n ≤ 100000
```

So O(n²) can cause TLE.

---

# 🥈 Approach 2 — Better Approach (DSU / Union Find)

---

# 💡 Core Observation

If indices become connected through valid jumps,
then every index in that connected component shares the same maximum reachable value.

So the problem becomes:

```text
Find connected components
```

---

# 🔍 DSU Visualization

## nums = [2,3,1]

Connections:

```text
0 -> 2
2 -> 1
```

Connected Component:

```text
[0,1,2]
```

Maximum inside component:

```text
3
```

Answer:

```text
[3,3,3]
```

---

# 📚 DSU Algorithm

1. Sort indices by value
2. Activate indices one by one
3. Union adjacent active indices
4. Store maximum value in component
5. Final answer = component maximum

---

# 💻 DSU C++ Code

```cpp
class Solution {
public:

    vector<int> parent, sz, mx;


    int find(int x) {

        if (parent[x] == x)
            return x;

        return parent[x] = find(parent[x]);
    }


    void unite(int a, int b) {

        a = find(a);
        b = find(b);

        if (a == b)
            return;


        if (sz[a] < sz[b])
            swap(a, b);


        parent[b] = a;

        sz[a] += sz[b];

        mx[a] = max(mx[a], mx[b]);
    }


    vector<int> maxValue(vector<int>& nums) {

        int n = nums.size();


        parent.resize(n);
        sz.assign(n, 1);
        mx.resize(n);


        for (int i = 0; i < n; i++) {

            parent[i] = i;
            mx[i] = nums[i];
        }


        vector<pair<int,int>> arr;


        for (int i = 0; i < n; i++) {
            arr.push_back({nums[i], i});
        }


        sort(arr.begin(), arr.end());


        vector<bool> active(n, false);


        for (auto &[val, idx] : arr) {

            active[idx] = true;


            if (idx > 0 && active[idx - 1]) {
                unite(idx, idx - 1);
            }


            if (idx + 1 < n && active[idx + 1]) {
                unite(idx, idx + 1);
            }
        }


        vector<int> ans(n);


        for (int i = 0; i < n; i++) {
            ans[i] = mx[find(i)];
        }

        return ans;
    }
};
```

---

# ⏱️ Time Complexity

| Operation      | Complexity |
| -------------- | ---------- |
| Sorting        | O(n log n) |
| DSU Operations | O(n α(n))  |

Overall:

```text
O(n log n)
```

---

# 📦 Space Complexity

```text
O(n)
```

---

# 🥇 Approach 3 — Optimal Greedy Approach

---

# 💡 Key Observation

If:

```text
prefix maximum > suffix minimum
```

then:

* larger value exists on left
* smaller value exists on right
* therefore jumps connect both regions

Hence both regions share same maximum reachable value.

---

# 📚 Greedy Algorithm

## Step 1 → Build Prefix Maximum

```text
pre[i] = max(nums[0...i])
```

---

## Step 2 → Build Suffix Minimum

```text
suf[i] = min(nums[i...n-1])
```

---

## Step 3 → Merge Components

If:

```text
pre[i] > suf[i+1]
```

then:

```text
res[i] = res[i+1]
```

Else:

```text
res[i] = pre[i]
```

---

# 🔄 Full Visualization

## nums = [2,3,1]

---

# Prefix Maximum

| Index | nums[i] | pre[i] |
| ----- | ------- | ------ |
| 0     | 2       | 2      |
| 1     | 3       | 3      |
| 2     | 1       | 3      |

```text
pre = [2,3,3]
```

---

# Suffix Minimum

| Index | nums[i] | suf[i] |
| ----- | ------- | ------ |
| 2     | 1       | 1      |
| 1     | 3       | 1      |
| 0     | 2       | 1      |

```text
suf = [1,1,1]
```

---

# Build Result

## Initial

```text
res[2] = 3
```

---

## i = 1

```text
pre[1] = 3
suf[2] = 1
```

Condition:

```text
3 > 1
```

Therefore:

```text
res[1] = 3
```

---

## i = 0

```text
pre[0] = 2
suf[1] = 1
```

Condition:

```text
2 > 1
```

Therefore:

```text
res[0] = 3
```

---

# 💻 Optimal C++ Code

```cpp
class Solution {
public:

    vector<int> maxValue(vector<int>& nums) {

        int n = nums.size();

        vector<int> pre(n), suf(n), res(n);


        // Build prefix maximum
        pre[0] = nums[0];

        for (int i = 1; i < n; i++) {
            pre[i] = max(pre[i - 1], nums[i]);
        }


        // Build suffix minimum
        suf[n - 1] = nums[n - 1];

        for (int i = n - 2; i >= 0; i--) {
            suf[i] = min(suf[i + 1], nums[i]);
        }


        // Last answer
        res[n - 1] = pre[n - 1];


        // Traverse backwards
        for (int i = n - 2; i >= 0; i--) {

            // Connected segments
            if (pre[i] > suf[i + 1]) {

                res[i] = res[i + 1];
            }

            // New segment
            else {
                res[i] = pre[i];
            }
        }

        return res;
    }
};
```

---

# ⚡ Step-by-Step Algorithm Code

```text
Algorithm JumpGameIX(nums):

1. Let n = size of nums

2. Create arrays:

      pre[n]
      suf[n]
      res[n]

3. Build prefix maximum:

      pre[0] = nums[0]

      for i from 1 to n-1:
          pre[i] = max(pre[i-1], nums[i])

4. Build suffix minimum:

      suf[n-1] = nums[n-1]

      for i from n-2 downto 0:
          suf[i] = min(suf[i+1], nums[i])

5. Initialize:

      res[n-1] = pre[n-1]

6. Traverse backwards:

      for i from n-2 downto 0:

          if pre[i] > suf[i+1]:
              res[i] = res[i+1]

          else:
              res[i] = pre[i]

7. Return res
```

---

# 📊 Complexity Comparison

| Approach         | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Brute Force DFS  | O(n²)           | O(n)             |
| DSU / Union Find | O(n log n)      | O(n)             |
| Optimal Greedy   | O(n)            | O(n)             |

---

# 🎯 What Information Do We Need at Each Step?

| Step         | Needed Information      |
| ------------ | ----------------------- |
| DFS          | Reachable indices       |
| DSU          | Connected components    |
| Greedy       | Prefix max & suffix min |
| Final Answer | Component maximum       |

---

# 🏆 Why Greedy is Optimal?

Because:

```text
pre[i] > suf[i+1]
```

already guarantees:

```text
A valid cross-boundary jump exists
```

So we do not need:

❌ DFS

❌ BFS

❌ Graph Construction

❌ DP

❌ DSU

---

# 🔥 Final Summary

This problem initially looks like:

* Graph Problem
* Reachability Problem
* DFS/BFS Problem

But the optimal solution is:

# 🚀 Prefix Maximum + Suffix Minimum + Greedy

which solves the problem in:

# ⚡ O(n)

---

<div align="center">

# ⭐ If this helped you, give the repository a star ⭐

</div>
