# 🔄 Rotate Image (LeetCode 48)

## 🧩 Problem Statement

Given an **n × n 2D matrix**, rotate the image by **90 degrees clockwise**.

⚠️ **Important Constraint:**
You must rotate the matrix **in-place** (do not use extra space).

---

## 📌 Example

### Input

```
1 2 3
4 5 6
7 8 9
```

### Output

```
7 4 1
8 5 2
9 6 3
```

---

## 🚀 Approach 1: Transpose + Reverse (Easy & Intuitive)

### 🔹 Idea

1. Transpose the matrix (swap rows ↔ columns)
2. Reverse each row

---

### 🔹 Steps

#### ✅ Step 1: Transpose

Swap:

```
matrix[i][j] ↔ matrix[j][i]
```

#### ✅ Step 2: Reverse each row

Use two-pointer approach:

```
start ↔ end
```

---

### 💻 Code (C++)

```cpp
#include <iostream>
using namespace std;

void rotate(int matrix[][20], int n) {
    // Step 1: Transpose
    for(int i = 0; i < n; i++) {
        for(int j = i + 1; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }

    // Step 2: Reverse each row
    for(int i = 0; i < n; i++) {
        int start = 0, end = n - 1;
        while(start < end) {
            swap(matrix[i][start], matrix[i][end]);
            start++;
            end--;
        }
    }
}
```

---

### 🧠 What information do I need at each step?

| Step      | Required Info                   |
| --------- | ------------------------------- |
| Transpose | indices `(i, j)` and `(j, i)`   |
| Reverse   | `start`, `end` pointers per row |

---

## 🚀 Approach 2: Layer-by-Layer Rotation (Optimal Swap Method)

### 🔹 Idea

Rotate matrix **layer by layer** (like onion layers), performing **4-way swaps**.

---

### 🔹 Visualization

Each rotation moves:

```
top → right  
right → bottom  
bottom → left  
left → top  
```

---

### 💻 Code (C++ - LeetCode Style)

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size(), k = n - 1;

        for(int i = 0; i < n >> 1; i++){
            for(int j = i; j < k - i; j++){
                int t = matrix[i][j];

                matrix[i][j] = matrix[k - j][i];
                matrix[k - j][i] = matrix[k - i][k - j];
                matrix[k - i][k - j] = matrix[j][k - i];
                matrix[j][k - i] = t;
            }
        }
    }
};
```

---

### 🧠 What information do I need at each step?

| Step              | Required Info       |
| ----------------- | ------------------- |
| Layer traversal   | `i` (layer index)   |
| Element traversal | `j`                 |
| Boundary          | `k = n - 1`         |
| Positions         | 4-way index mapping |

---

## ⚡ Complexity Analysis

| Type             | Value           |
| ---------------- | --------------- |
| Time Complexity  | O(n²)           |
| Space Complexity | O(1) (in-place) |

---

## 🧠 Key Insights

* No extra matrix allowed → must modify in-place
* Two popular approaches:

  * ✅ Transpose + Reverse (easy)
  * ✅ Layer Rotation (interview favorite)

---

## 🧩 Memory Trick

👉 **"Transpose + Reverse Rows = Clockwise Rotation"**

---

## 🔁 Bonus

| Rotation Type  | Method                      |
| -------------- | --------------------------- |
| Clockwise      | Transpose + Reverse rows    |
| Anti-clockwise | Transpose + Reverse columns |

---

## 🏁 Conclusion

This problem tests:

* Matrix manipulation
* Index handling
* In-place operations

💡 Very common in interviews (FAANG level)

---

## ⭐ If you found this helpful

Give this repo a ⭐ and keep practicing DSA 🚀

---
