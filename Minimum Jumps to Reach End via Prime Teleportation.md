# 🚀 3629. Minimum Jumps to Reach End via Prime Teleportation

<div align="center">

# ⚡ CodeWithHSquare DSA Visualized

### LeetCode Medium Problem Solution

<img src="https://img.shields.io/badge/CodeWithHSquare-DSA_Visualizer-blueviolet?style=for-the-badge"/>
<img src="https://img.shields.io/badge/BFS-Shortest_Path-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Graphs-Unweighted-success?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Sieve-Prime_Factors-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/C%2B%2B-Optimized-red?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Visualization-Friendly-purple?style=for-the-badge"/>

---

## 🌟 Part of the CodeWithHSquare DSA Learning & Visualization Repository

✨ Learn DSA with:

📌 Visual Explanations
📌 Step-by-Step Algorithms
📌 Dry Runs
📌 Optimized Solutions
📌 Interview-Level Thinking
📌 Beginner Friendly Explanations

</div>

---

# 📘 Problem Statement

You are given an integer array `nums` of length `n`.

You start at index `0`, and your goal is to reach index `n - 1`.

From any index `i`, you may perform one of the following operations:

---

# ✅ Adjacent Step

Jump to:

```cpp
i + 1
```

or

```cpp
i - 1
```

if the index is within bounds.

---

# ✅ Prime Teleportation

If:

```cpp
nums[i]
```

is a prime number `p`, then you may instantly jump to any index `j != i` such that:

```cpp
nums[j] % p == 0
```

---

# 🎯 Goal

Return the minimum number of jumps required to reach:

```cpp
n - 1
```

---
<div align="center">

## 🌐 Live Visualization

### 🔗 [Open Interactive Visualizer](https://minimum-jumps-via-prime-teleportati.vercel.app/)

</div>

# 🧠 Intuition

This problem looks difficult at first because:

* We can move normally
* We can teleport
* Teleportation depends on prime numbers

But actually:

# 🚀 This is a Shortest Path Problem

---

# 🔥 Think Like a Graph

Every index behaves like a:

# ➜ Graph Node

Every possible move behaves like a:

# ➜ Graph Edge

| Operation      | Edge          |
| -------------- | ------------- |
| `i → i+1`      | Adjacent edge |
| `i → i-1`      | Adjacent edge |
| Prime teleport | Special edge  |

---

# ✅ Which Algorithm Finds Minimum Steps?

For:

# Unweighted Graph Shortest Path

We always use:

# 🚀 BFS (Breadth First Search)

Because BFS explores:

```text
Level by level
```

which guarantees:

# ✅ Minimum Jumps

---

# ❓ What Information Do We Need at Each Step?

| Information   | Why Needed            |
| ------------- | --------------------- |
| Current index | To process moves      |
| Visited array | To avoid cycles       |
| Prime factors | To know teleportation |
| Prime buckets | To jump quickly       |
| BFS level     | To count jumps        |

---

# 🔥 Main Optimization

We create:

```cpp
edges[p]
```

which stores:

```text
All indices divisible by prime p
```

Example:

```cpp
nums = [1,2,4,6]
```

Then:

```cpp
edges[2] = [1,2,3]
edges[3] = [3]
```

Now teleportation becomes:

# ⚡ O(1) Access

instead of searching the whole array.

---

# ❌ Common Mistake

Many people write:

```cpp
factors[x].size() == 1
```

for prime checking.

This is WRONG.

---

# ⚠️ Why Wrong?

Example:

| Number | Prime Factors | Size | Prime? |
| ------ | ------------- | ---- | ------ |
| 2      | {2}           | 1    | ✅      |
| 8      | {2}           | 1    | ❌      |
| 9      | {3}           | 1    | ❌      |
| 27     | {3}           | 1    | ❌      |

These numbers have:

```text
Only ONE DISTINCT prime factor
```

but they are NOT prime.

---

# ✅ Correct Prime Check

```cpp
factors[x].size() == 1 && factors[x][0] == x
```

Because:

```cpp
factors[prime] = {prime}
```

---

# 🪜 Step-by-Step Algorithm

# ✅ Step 1 — Precompute Prime Factors

Use sieve technique to store:

```cpp
factors[number]
```

---

# ✅ Step 2 — Build Prime Buckets

For every index:

```cpp
edges[p].push_back(index)
```

where:

```cpp
nums[index] % p == 0
```

---

# ✅ Step 3 — Start BFS

Start from:

```cpp
index 0
```

---

# ✅ Step 4 — Process Adjacent Moves

Visit:

```cpp
i-1
i+1
```

---

# ✅ Step 5 — Process Prime Teleportation

If current value is prime:

```cpp
nums[i] = p
```

then visit:

```cpp
all indices in edges[p]
```

---

# ✅ Step 6 — Clear Bucket

```cpp
edges[p].clear();
```

This avoids:

# ❌ TLE (Time Limit Exceeded)

because same teleportation bucket won't be processed again.

---

# 💻 Fully Optimized C++ Solution

```cpp
const int MX = 1000001;

vector<int> factors[MX];

bool initialized = []() {
    for (int i = 2; i < MX; ++i) {
        if (factors[i].empty()) {
            for (int j = i; j < MX; j += i) {
                factors[j].push_back(i);
            }
        }
    }
    return true;
}();

class Solution {
public:
    int minJumps(vector<int>& nums) {

        int n = nums.size();

        unordered_map<int, vector<int>> edges;

        // Build prime buckets
        for (int i = 0; i < n; ++i) {
            for (int p : factors[nums[i]]) {
                edges[p].push_back(i);
            }
        }

        int res = 0;

        vector<bool> seen(n, false);

        seen[0] = true;

        vector<int> q = {0};

        while (!q.empty()) {

            vector<int> q2;

            for (int i : q) {

                // Reached end
                if (i == n - 1)
                    return res;

                // Left move
                if (i > 0 && !seen[i - 1]) {
                    seen[i - 1] = true;
                    q2.push_back(i - 1);
                }

                // Right move
                if (i < n - 1 && !seen[i + 1]) {
                    seen[i + 1] = true;
                    q2.push_back(i + 1);
                }

                // Prime teleportation
                if (factors[nums[i]].size() == 1 &&
                    factors[nums[i]][0] == nums[i]) {

                    int p = nums[i];

                    for (int j : edges[p]) {

                        if (!seen[j]) {
                            seen[j] = true;
                            q2.push_back(j);
                        }
                    }

                    // Important optimization
                    edges[p].clear();
                }
            }

            q = move(q2);

            res++;
        }

        return -1;
    }
};
```

---

# 🧪 Dry Run

# ✅ Example

```cpp
nums = [1,2,4,6]
```

---

# 🔹 Step 1

Start:

```text
Index = 0
```

Possible moves:

```text
0 → 1
```

---

# 🔹 Step 2

Now:

```cpp
nums[1] = 2
```

`2` is prime.

Teleport using:

```cpp
edges[2] = [1,2,3]
```

Jump directly:

```text
1 → 3
```

Reached end.

---

# ✅ Minimum Jumps

```text
2
```

---

# 📊 Complexity Analysis

# ✅ Sieve Preprocessing

```text
O(MAX log log MAX)
```

---

# ✅ BFS Traversal

Each:

* index visited once
* prime bucket cleared once

So:

```text
O(n log MAX)
```

---

# ✅ Space Complexity

```text
O(n + MAX)
```

---

# 🚨 Why Clearing Buckets Is Important

Without:

```cpp
edges[p].clear();
```

same teleportation bucket gets revisited repeatedly.

That causes:

# ❌ Huge Repeated Work

and eventually:

# ❌ TLE

---

# 🎯 Interview Concepts Used

| Concept               | Used |
| --------------------- | ---- |
| BFS                   | ✅    |
| Graph Theory          | ✅    |
| Sieve of Eratosthenes | ✅    |
| Prime Factorization   | ✅    |
| Hash Maps             | ✅    |
| Optimization          | ✅    |
| Shortest Path         | ✅    |

---

# 🌟 Final Takeaway

This problem is a beautiful combination of:

* Graph traversal
* BFS shortest path
* Prime preprocessing
* Smart optimization

The MOST IMPORTANT trick is:

# 🚀 Process each prime bucket only once

which makes the solution efficient enough for:

```text
n = 1e5
```

---

<div align="center">

# 💜 CodeWithHSquare

### Learn • Visualize • Master DSA

⭐ Beginner Friendly
⭐ Visualization Focused
⭐ Interview Oriented
⭐ Competitive Programming Ready

</div>
