# 🔄 1914. Cyclically Rotating a Grid

<div align="center">

# 🚀 Cyclically Rotating a Grid

### LeetCode Medium

<img src="https://img.shields.io/badge/Matrix-Problem-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Simulation-Approach-green?style=for-the-badge"/>
<img src="https://img.shields.io/badge/C%2B%2B-Optimized-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Contest-Weekly_247-purple?style=for-the-badge"/>

</div>

---

# 🌟 Problem Statement

You are given an `m × n` matrix where both `m` and `n` are even.

The matrix contains multiple **layers (rings)**.

For every rotation:

- Each layer rotates
- Rotation is performed in the **counter-clockwise** direction
- You must perform exactly `k` rotations

Return the final rotated matrix.

---

# 🧠 Key Observation

The most important realization:

> Every layer behaves like a circular array.

So instead of rotating the whole matrix directly:

✅ Extract layer into array  
✅ Rotate array  
✅ Put values back

This makes the problem simple and efficient.

---

# 🎯 Visualization

## Original Matrix

```text
1   2   3   4
5   6   7   8
9   10  11  12
13  14  15  16
```

---

## Outer Layer Extraction

```text
1 → 2 → 3 → 4
↑             ↓
5             8
↑             ↓
9             12
↑             ↓
13 ←14 ←15 ←16
```

Stored as:

```text
[1,2,3,4,8,12,16,15,14,13,9,5]
```

---

## After Rotation (`k = 2`)

```text
[3,4,8,12,16,15,14,13,9,5,1,2]
```

---

## Final Matrix

```text
3   4   8   12
2   11  10  16
1   7   6   15
5   9   13  14
```

---

# ⚡ Optimized Approach

For every layer:

1. Store coordinates
2. Store values
3. Rotate using modular arithmetic
4. Reassign rotated values

---

# 📌 What Information Do We Need At Each Step?

| Step | Required Information |
|---|---|
| Find layers | `min(m,n)/2` |
| Traverse layer | `top, bottom, left, right` |
| Rotate | `k % layer_size` |
| Refill matrix | stored coordinates |

---

# ✅ My Optimized C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) {

        int m = grid.size();
        int n = grid[0].size();

        int nlayer = min(m / 2, n / 2);

        for (int layer = 0; layer < nlayer; ++layer) {

            vector<int> r, c, val;

            // left side
            for (int i = layer; i < m - layer - 1; ++i) {
                r.push_back(i);
                c.push_back(layer);
                val.push_back(grid[i][layer]);
            }

            // bottom side
            for (int j = layer; j < n - layer - 1; ++j) {
                r.push_back(m - layer - 1);
                c.push_back(j);
                val.push_back(grid[m - layer - 1][j]);
            }

            // right side
            for (int i = m - layer - 1; i > layer; --i) {
                r.push_back(i);
                c.push_back(n - layer - 1);
                val.push_back(grid[i][n - layer - 1]);
            }

            // top side
            for (int j = n - layer - 1; j > layer; --j) {
                r.push_back(layer);
                c.push_back(j);
                val.push_back(grid[layer][j]);
            }

            int total = val.size();

            int kk = k % total;

            for (int i = 0; i < total; ++i) {

                int idx = (i + total - kk) % total;

                grid[r[i]][c[i]] = val[idx];
            }
        }

        return grid;
    }
};
```

---

# 🔍 Understanding the Rotation Formula

Core line:

```cpp
int idx = (i + total - kk) % total;
```

This means:

```text
current position ← shifted previous value
```

Equivalent to:

```cpp
new[i] = old[(i-k+size)%size]
```

which performs cyclic rotation.

---

# 🪄 Why `k % total` ?

If layer size is:

```text
12
```

and:

```text
k = 14
```

then:

```text
14 % 12 = 2
```

because after every complete cycle,
the layer returns to its original state.

---

# 🧪 Dry Run

## Input

```cpp
grid =
[
 [40,10],
 [30,20]
]

k = 1
```

---

## Extracted Layer

```text
[40,10,20,30]
```

---

## Rotated Layer

```text
[10,20,30,40]
```

---

## Final Matrix

```cpp
[
 [10,20],
 [40,30]
]
```

---

# 🚀 Alternative STL Approach

Using STL `rotate()` function.

```cpp
class Solution {
public:

    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) {

        int m = grid.size();
        int n = grid[0].size();

        int layers = min(m, n) / 2;

        for (int layer = 0; layer < layers; layer++) {

            vector<int> v;

            int top = layer;
            int left = layer;
            int bottom = m - layer - 1;
            int right = n - layer - 1;

            for (int j = left; j <= right; j++)
                v.push_back(grid[top][j]);

            for (int i = top + 1; i <= bottom - 1; i++)
                v.push_back(grid[i][right]);

            for (int j = right; j >= left; j--)
                v.push_back(grid[bottom][j]);

            for (int i = bottom - 1; i >= top + 1; i--)
                v.push_back(grid[i][left]);

            int rot = k % v.size();

            rotate(v.begin(), v.begin() + rot, v.end());

            int idx = 0;

            for (int j = left; j <= right; j++)
                grid[top][j] = v[idx++];

            for (int i = top + 1; i <= bottom - 1; i++)
                grid[i][right] = v[idx++];

            for (int j = right; j >= left; j--)
                grid[bottom][j] = v[idx++];

            for (int i = bottom - 1; i >= top + 1; i--)
                grid[i][left] = v[idx++];
        }

        return grid;
    }
};
```

---

# 📊 Complexity Analysis

## Time Complexity

```text
O(m × n)
```

Each cell is visited once.

---

## Space Complexity

```text
O(m + n)
```

Extra space used for storing one layer.

---

# 🎓 Important Learning

This problem teaches:

- Matrix layer traversal
- Simulation
- Circular array rotation
- Modular arithmetic
- Coordinate mapping

---

# 💡 Interview Tip

Whenever you see:

- Ring traversal
- Spiral movement
- Matrix boundary processing

Always think:

```text
"Can I convert this layer into a 1D circular array?"
```

That is usually the hidden trick.

---

# 🏆 CodeWithHSquare

<div align="center">

### 💻 Crafted with Logic & Visualization by CodeWithHSquare

🚀 DSA • Competitive Programming • Development

</div>

---

# ⭐ Support

If you found this helpful:

⭐ Star the repository  
🍴 Fork it  
🧠 Practice more DSA problems

---

# 📚 Topics Covered

```text
Matrix
Simulation
Array Rotation
Circular Traversal
Implementation
```

---

