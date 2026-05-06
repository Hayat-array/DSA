# 🔄 Rotating the Box (LeetCode 1861)

<div align="center">

### 👨‍💻 by **codewithhsquare (Hayat Ali)**

🚀 *Striver DSA Sheet Journey*

</div>

---

# 🧾 Problem Statement

You are given an `m x n` matrix of characters `boxGrid` representing a side-view of a box.

Each cell contains:

* `'#'` → Stone
* `'*'` → Obstacle
* `'.'` → Empty space

The box is rotated **90 degrees clockwise**, causing stones to fall due to gravity.

A stone falls until:

* It hits another stone
* It hits an obstacle
* It reaches the bottom

Return the final rotated matrix.

---

# 📌 Example

## Input

```text
boxGrid =
[
 ["#", ".", "#" ]
]
```

## Output

```text
[
 ["."],
 ["#"],
 ["#"]
]
```

---

# 🧠 Intuition

This problem has 2 main steps:

## ✅ Step 1: Apply Gravity

Before rotation:

* Stones fall toward the **right side**
* Obstacles remain fixed

We process each row from **right → left**.

---

## ✅ Step 2: Rotate Matrix

Rotate the matrix by **90° clockwise**.

Rotation Formula:

```text
rotated[j][m - 1 - i] = boxGrid[i][j]
```

---

# 🎯 Approach

## 🔹 Gravity Simulation

Maintain a pointer:

```cpp
emptyPos
```

which stores the next valid position where a stone can fall.

### Cases

| Cell  | Action                   |
| ----- | ------------------------ |
| `'*'` | Reset `emptyPos`         |
| `'#'` | Move stone to `emptyPos` |
| `'.'` | Ignore                   |

---

## 🔹 Matrix Rotation

If original matrix is:

```text
m x n
```

then rotated matrix becomes:

```text
n x m
```

---

# 🧩 Different Approaches

---

# ✅ Approach 1: Brute Force Simulation

## 💡 Idea

* Rotate the matrix first
* Then simulate gravity column-wise
* Stones fall downward after rotation

---

## 🧠 Algorithm

### Step 1

Rotate matrix by 90° clockwise.

### Step 2

For every column:

* Traverse bottom → top
* Maintain `emptyRow`
* Drop stones downward
* Reset when obstacle appears

---

## ⏱ Complexity

| Complexity | Value      |
| ---------- | ---------- |
| Time       | `O(m × n)` |
| Space      | `O(m × n)` |

---

# ✅ Approach 2: Gravity Before Rotation (Optimal)

## 💡 Idea

Instead of rotating first:

* Apply gravity row-wise
* Stones move right
* Then rotate matrix

This is cleaner and easier to implement.

---

## 🧠 Algorithm

### 🔹 Step 1: Apply Gravity

For every row:

* Start from rightmost side
* Maintain `emptyPos`
* If obstacle appears → reset `emptyPos`
* If stone appears → move it to `emptyPos`

---

### 🔹 Step 2: Rotate Matrix

Use mapping:

```text
(i,j) → (j,m-1-i)
```

---

## ⏱ Complexity

| Complexity | Value      |
| ---------- | ---------- |
| Time       | `O(m × n)` |
| Space      | `O(m × n)` |

---

# ✅ Approach 3: Queue-Based Simulation

## 💡 Idea

Use a queue to track empty spaces.

Whenever:

* `'.'` → push index into queue
* `'#'` → move stone to earliest empty space
* `'*'` → clear queue

This works but is less efficient logically.

---

## 🧠 Algorithm

### Step 1

Traverse every row.

### Step 2

Maintain queue of empty positions.

### Step 3

Move stones whenever queue has valid empty positions.

### Step 4

Rotate matrix.

---

## ⏱ Complexity

| Complexity | Value      |
| ---------- | ---------- |
| Time       | `O(m × n)` |
| Space      | `O(n)`     |

---

# 💻 Approach 1: Brute Force Code

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {

        int m = boxGrid.size();
        int n = boxGrid[0].size();

        // Step 1: Rotate matrix first
        vector<vector<char>> rotated(n, vector<char>(m));

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                rotated[j][m - 1 - i] = boxGrid[i][j];
            }
        }

        // Step 2: Apply gravity downward
        for(int col = 0; col < m; col++) {

            int emptyRow = n - 1;

            for(int row = n - 1; row >= 0; row--) {

                if(rotated[row][col] == '*') {
                    emptyRow = row - 1;
                }

                else if(rotated[row][col] == '#') {

                    swap(rotated[row][col], rotated[emptyRow][col]);

                    emptyRow--;
                }
            }
        }

        return rotated;
    }
};
```

---

# 💻 Approach 2: Optimal Two Pointer Code

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {

        int rows = boxGrid.size();
        int cols = boxGrid[0].size();

        // Step 1: Apply gravity
        for(int r = 0; r < rows; r++) {

            int emptyPos = cols - 1;

            for(int c = cols - 1; c >= 0; c--) {

                // obstacle
                if(boxGrid[r][c] == '*') {
                    emptyPos = c - 1;
                }

                // stone
                else if(boxGrid[r][c] == '#') {

                    swap(boxGrid[r][c], boxGrid[r][emptyPos]);

                    emptyPos--;
                }
            }
        }

        // Step 2: Rotate matrix
        vector<vector<char>> res(cols, vector<char>(rows));

        for(int r = 0; r < rows; r++) {

            for(int c = 0; c < cols; c++) {

                res[c][rows - 1 - r] = boxGrid[r][c];
            }
        }

        return res;
    }
};
```

---

# 💻 Approach 3: Queue Based Code

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {

        int m = boxGrid.size();
        int n = boxGrid[0].size();

        // Apply gravity using queue
        for(int i = 0; i < m; i++) {

            queue<int> empty;

            for(int j = 0; j < n; j++) {

                if(boxGrid[i][j] == '.') {
                    empty.push(j);
                }

                else if(boxGrid[i][j] == '*') {

                    while(!empty.empty()) {
                        empty.pop();
                    }
                }

                else if(boxGrid[i][j] == '#') {

                    if(!empty.empty()) {

                        int pos = empty.front();
                        empty.pop();

                        boxGrid[i][pos] = '#';
                        boxGrid[i][j] = '.';

                        empty.push(j);
                    }
                }
            }
        }

        // Rotate matrix
        vector<vector<char>> rotated(n, vector<char>(m));

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {

                rotated[j][m - 1 - i] = boxGrid[i][j];
            }
        }

        return rotated;
    }
};
```

---

# 🏆 Best Approach

✅ Two Pointer + Gravity Before Rotation

Why?

* Cleaner logic
* Easy implementation
* Less confusion
* Interview friendly
* Optimal complexity

---

# 💻 C++ Solution

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {

        int rows = boxGrid.size();
        int cols = boxGrid[0].size();

        // Step 1: Apply gravity
        for(int r = 0; r < rows; r++) {

            int emptyPos = cols - 1;

            for(int c = cols - 1; c >= 0; c--) {

                // obstacle
                if(boxGrid[r][c] == '*') {
                    emptyPos = c - 1;
                }

                // stone
                else if(boxGrid[r][c] == '#') {

                    swap(boxGrid[r][c], boxGrid[r][emptyPos]);

                    emptyPos--;
                }
            }
        }

        // Step 2: Rotate matrix
        vector<vector<char>> res(cols, vector<char>(rows));

        for(int r = 0; r < rows; r++) {

            for(int c = 0; c < cols; c++) {

                res[c][rows - 1 - r] = boxGrid[r][c];
            }
        }

        return res;
    }
};
```

---

# 🔍 Dry Run

## Input

```text
# . * .
# # * .
```

---

## After Gravity

```text
. # * .
# # * .
```

---

## After Rotation

```text
# .
# #
* *
. .
```

---

# 🧠 What information do I need at each step?

## During Gravity

| Information  | Purpose                     |
| ------------ | --------------------------- |
| `emptyPos`   | Next valid falling position |
| obstacle `*` | Reset section               |
| current cell | Decide movement             |

---

## During Rotation

| Information     | Purpose           |
| --------------- | ----------------- |
| `(r,c)`         | Original position |
| `(c, rows-1-r)` | Rotated position  |

---

# ⏱ Complexity Analysis

## Time Complexity

```text
O(m × n)
```

We traverse the matrix twice.

---

## Space Complexity

```text
O(m × n)
```

Extra matrix for rotation.

---

# 🧠 Common Mistakes

| Mistake                             | Problem                          |
| ----------------------------------- | -------------------------------- |
| Using `=` instead of `==`           | Assignment instead of comparison |
| Rotating before gravity incorrectly | Wrong simulation                 |
| Moving obstacles                    | Obstacles must stay fixed        |
| Traversing left → right             | Stones should fall right         |
| Wrong rotation indices              | Incorrect output                 |

---

# 🔄 Rotation Mapping

For clockwise rotation:

```text
(i,j) → (j,m-1-i)
```

---

# 🚀 Key Interview Learnings

✅ Apply gravity BEFORE rotation
✅ Obstacles never move
✅ Traverse from destination side
✅ Remember clockwise rotation mapping:

```text
(i,j) → (j,m-1-i)
```

---

# 📚 Topics Used

* Matrix
* Simulation
* Two Pointers
* Array Manipulation

---

<div align="center">

### ⭐ If you found this helpful, give the repository a star ⭐

</div>
