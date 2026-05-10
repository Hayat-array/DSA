# 🚀 2770. Maximum Number of Jumps to Reach the Last Index

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:ff512f,100:dd2476&height=200&section=header&text=Maximum%20Number%20of%20Jumps&fontSize=35&fontColor=ffffff&animation=fadeIn&fontAlignY=38" width="100%"/>

<br>

![LeetCode](https://img.shields.io/badge/LeetCode-2770-orange?style=for-the-badge\&logo=leetcode)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-C%2B%2B-blue?style=for-the-badge\&logo=c%2B%2B)
![Topic](https://img.shields.io/badge/Topic-Dynamic_Programming-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Solved-success?style=for-the-badge)

<h3>💡 Dynamic Programming | DFS + Memoization | Graph Thinking</h3>

</div>

---

# 📚 Table of Contents

* ✨ Problem Statement
* 🧠 Intuition
* 📌 Key Observation
* 🪜 Dry Run
* 🎥 Visualization
* 💻 Approach 1: Bottom-Up DP
* 💻 Approach 2: Top-Down DP (Memoization)
* ⚖️ Comparison of Approaches
* 📊 Complexity Analysis
* ❌ Why Greedy Fails
* 🌟 Key Learnings
* 🏆 Tags
* 👨‍💻 Author

---

# ✨ Problem Statement

You are given:

* An integer array `nums`
* An integer `target`

Initially, you are standing at index `0`.

You can jump from index `i` to index `j` if:

```math
0 <= i < j < n
```

and

```math
-target <= nums[j] - nums[i] <= target
```

Return the **maximum number of jumps** required to reach the last index.

If it is impossible to reach the last index, return:

```cpp
-1
```

---

# 🧠 Intuition

We are NOT trying to minimize jumps.

Instead:

```text
We want the maximum number of jumps.
```

That changes the entire thinking process.

---

## ❌ Why Greedy Fails?

Greedy algorithms usually work when we need:

* Minimum jumps
* Shortest path
* Closest/farthest reach

But here:

```text
Smaller jumps may create MORE total jumps.
```

So we must explore:

✅ Every valid previous state

Which naturally leads to:

# 🚀 Dynamic Programming

---

# 📌 Key Observation

For every index `i`, we need:

```cpp
Maximum jumps required to reach index i
```

So define:

```cpp
dp[i]
```

---

## DP Meaning

| State             | Meaning                    |
| ----------------- | -------------------------- |
| `dp[i] = INT_MIN` | Index is unreachable       |
| `dp[i] >= 0`      | Maximum jumps to reach `i` |

---

# 🔄 State Transition

If we can jump from `j → i`:

```cpp
abs(nums[j] - nums[i]) <= target
```

Then:

```cpp
dp[i] = max(dp[i], dp[j] + 1)
```

---

# 🪜 Dry Run

## 📥 Input

```cpp
nums = [1,3,6,4,1,2]
target = 2
```

---

## Initial State

```cpp
dp = [0,-INF,-INF,-INF,-INF,-INF]
```

---

## 🔹 Reach Index 1

```cpp
3 - 1 = 2
```

Valid jump.

```cpp
dp[1] = dp[0] + 1 = 1
```

Updated DP:

```cpp
[0,1,-INF,-INF,-INF,-INF]
```

---

## 🔹 Reach Index 3

From index `1`:

```cpp
4 - 3 = 1
```

Valid.

```cpp
dp[3] = dp[1] + 1 = 2
```

---

## 🔹 Reach Index 5

From index `3`:

```cpp
2 - 4 = -2
```

Valid.

```cpp
dp[5] = dp[3] + 1 = 3
```

---

# ✅ Final Answer

```cpp
3
```

---

# 🎥 Visualization

<div align="center">

## 🌐 Interactive Visualization

### 🔗 [https://maximum-number-of-jumps-to-reach-th.vercel.app/](https://maximum-number-of-jumps-to-reach-th.vercel.app/)

</div>

---

# 💻 Approach 1: Bottom-Up Dynamic Programming

<div align="center">

## ⚡ Iterative DP Solution

</div>

---

# 📌 Algorithm

## Step 1

Create a DP array:

```cpp
vector<int> dp(n, INT_MIN);
```

---

## Step 2

Starting index requires `0` jumps:

```cpp
dp[0] = 0;
```

---

## Step 3

Try every previous index:

```cpp
for(int j = 0; j < i; j++)
```

---

## Step 4

Check if jump is valid:

```cpp
abs(nums[j] - nums[i]) <= target
```

---

## Step 5

Update maximum jumps:

```cpp
dp[i] = max(dp[i], dp[j] + 1);
```

---

# ✅ Bottom-Up DP Code

```cpp
class Solution {
public:
    int maximumJumps(vector<int>& nums, int target) {
        int n = nums.size();

        vector<int> dp(n, INT_MIN);

        dp[0] = 0;

        for(int i = 1; i < n; i++) {

            for(int j = 0; j < i; j++) {

                if(dp[j] != INT_MIN &&
                   abs(1LL * nums[j] - nums[i]) <= target) {

                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }

        return dp[n - 1] < 0 ? -1 : dp[n - 1];
    }
};
```

---

# 💻 Approach 2: Top-Down DP (Memoization + DFS)

<div align="center">

## 🔥 Recursive DP Solution

</div>

---

# 🧠 Intuition

Instead of building answers iteratively:

```text
We recursively try all possible valid jumps.
```

For every index `i`, we calculate:

```cpp
Maximum jumps from index i to reach the last index
```

To avoid recomputation:

```cpp
memo[i]
```

stores already computed answers.

---

# 📌 Algorithm

## Step 1

Create memo array:

```cpp
vector<int> memo(n, INT_MIN);
```

---

## Step 2

Define recursive DFS:

```cpp
dfs(i)
```

---

## Step 3

Base Case:

```cpp
if(i == n-1) return 0;
```

---

## Step 4

Try every next index:

```cpp
for(int j = i + 1; j < n; j++)
```

---

## Step 5

If jump is valid:

```cpp
abs(nums[i] - nums[j]) <= target
```

Update answer:

```cpp
res = max(res, dfs(j) + 1);
```

---

## Step 6

Store answer:

```cpp
memo[i] = res;
```

---

# ✅ Memoization Code

```cpp
class Solution {
public:
    int maximumJumps(vector<int>& nums, int target) {
        int n = nums.size();

        vector<int> memo(n, INT_MIN);

        function<int(int)> dfs = [&](int i) -> int {

            if(i == n - 1) {
                return 0;
            }

            if(memo[i] != INT_MIN) {
                return memo[i];
            }

            int res = INT_MIN;

            for(int j = i + 1; j < n; j++) {

                if(abs(1LL * nums[i] - nums[j]) <= target) {

                    res = max(res, dfs(j) + 1);
                }
            }

            return memo[i] = res;
        };

        int ans = dfs(0);

        return ans < 0 ? -1 : ans;
    }
};
```

---

# ⚖️ Bottom-Up vs Top-Down

| Feature          | Bottom-Up DP | Top-Down Memoization |
| ---------------- | ------------ | -------------------- |
| Style            | Iterative    | Recursive            |
| Uses Recursion   | ❌            | ✅                    |
| Memoization      | ❌            | ✅                    |
| Stack Space      | ❌            | ✅                    |
| Easy to Debug    | ✅            | ✅                    |
| Time Complexity  | `O(n²)`      | `O(n²)`              |
| Space Complexity | `O(n)`       | `O(n)`               |

---

# 🔍 Why Use `1LL`?

```cpp
abs(1LL * nums[j] - nums[i])
```

Because:

```cpp
nums[i] can be up to 1e9
```

Difference may overflow `int`.

Using `1LL` converts calculation into:

```cpp
long long
```

which safely handles large values.

---

# 📊 Complexity Analysis

<div align="center">

| Complexity       | Value   |
| ---------------- | ------- |
| Time Complexity  | `O(n²)` |
| Space Complexity | `O(n)`  |

</div>

---

# 🌟 Key Learnings

✅ Dynamic Programming on indices

✅ State transition using previous reachable states

✅ Memoization optimization

✅ Handling unreachable states safely

✅ Preventing integer overflow using `1LL`

✅ Difference between maximizing and minimizing problems

---

# 🏆 Tags

<div align="center">

`Dynamic Programming` • `DFS` • `Memoization` • `Array` • `Graph Thinking`

</div>

---

# 👨‍💻 Author

<div align="center">

# 🚀 CodeWithHSquare

### 💡 DSA • Development • Competitive Programming • Problem Solving

⭐ Building clean and beginner-friendly solutions for developers.

</div>

---

# ⭐ Support

If you found this repository helpful:

<div align="center">

⭐ Star this repository
🍴 Fork this repository
🧠 Share with friends
💻 Follow for more DSA solutions

</div>

---

<div align="center">

## 🌟 Happy Coding with CodeWithHSquare 🚀

</div>
