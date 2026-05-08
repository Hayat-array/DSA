# 🔷 Pattern 22 (Concentric Number Pattern)

<h2 align="center">🚀 Pattern Printing | Nested Loops | Interview Logic</h2>

<p align="center">
  <img src="https://img.shields.io/badge/Level-Medium-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topic-Pattern%20Printing-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Language-C++-black?style=for-the-badge&logo=c%2B%2B"/>
</p>

---

# 🧩 Problem Statement

Print a concentric rectangular number pattern for a given integer `n`.

For:

```cpp
n = 4
```

Output:

```cpp
4 4 4 4 4 4 4
4 3 3 3 3 3 4
4 3 2 2 2 3 4
4 3 2 1 2 3 4
4 3 2 2 2 3 4
4 3 3 3 3 3 4
4 4 4 4 4 4 4
```

---

# 🧠 Core Idea

The pattern forms:

* Outer layer → largest number
* Inner layers → decreasing numbers

It behaves like:

```cpp
Concentric Squares
```

---

# 🔍 Pattern Observation

For:

```cpp
n = 4
```

Matrix size becomes:

```cpp
2*n - 1 = 7
```

So final matrix is:

```cpp
7 × 7
```

---

# 🚀 Approach 1: Upper Half + Lower Half Logic

## 🔹 Idea

We divide the pattern into:

* Upper half
* Lower half

For every row:

1. Print decreasing left numbers
2. Print center repeated numbers
3. Print increasing right numbers

---

# 💻 C++ Code

```cpp
#include <iostream>
using namespace std;

void pat22(int n){

    // Upper Half
    for(int i = 0; i < n; i++){

        // Left decreasing numbers
        for(int s = 0; s < i; s++){
            cout << n - s << " ";
        }

        // Middle repeated numbers
        for(int j = 0; j < 2*n - 1 - 2*i; j++){
            cout << n - i << " ";
        }

        // Right increasing numbers
        for(int s = i; s >= 1; s--){
            cout << n - s + 1 << " ";
        }

        cout << endl;
    }

    // Lower Half
    for(int i = n - 2; i >= 0; i--){

        // Left decreasing numbers
        for(int s = 0; s < i; s++){
            cout << n - s << " ";
        }

        // Middle repeated numbers
        for(int j = 0; j < 2*n - 1 - 2*i; j++){
            cout << n - i << " ";
        }

        // Right increasing numbers
        for(int s = i; s >= 1; s--){
            cout << n - s + 1 << " ";
        }

        cout << endl;
    }
}

int main() {

    int n = 4;

    pat22(n);

    return 0;
}
```

---

# 🚀 Approach 2: Mathematical Distance Formula (Best & Elegant)

## 🔹 Idea

For every cell:

* Find minimum distance from all 4 borders

Formula:

```cpp
min(min(i, j), min(size-1-i, size-1-j))
```

Then:

```cpp
value = n - minimum_distance
```

---

# 💻 C++ Code

```cpp
#include <iostream>
using namespace std;

void pattern22(int n){

    int size = 2 * n - 1;

    for(int i = 0; i < size; i++){

        for(int j = 0; j < size; j++){

            int top = i;
            int left = j;
            int right = size - 1 - j;
            int bottom = size - 1 - i;

            int mini = min(min(top, bottom), min(left, right));

            cout << n - mini << " ";
        }

        cout << endl;
    }
}

int main(){

    int n = 4;

    pattern22(n);

    return 0;
}
```

---

# 🔥 Why Approach 2 is Better

| Feature                 | Benefit              |
| ----------------------- | -------------------- |
| Short code              | Easy to remember     |
| Mathematical logic      | Interview favorite   |
| No upper/lower division | Cleaner approach     |
| Easy visualization      | Better understanding |

---

# ⚠️ Important Fix

In your original code:

```cpp
patt22(n);
```

was incorrect because function name is:

```cpp
pat22(n);
```

✅ Fixed in final code.

---

# 🧠 What Information Do I Need at Each Step?

| Step               | Required Information     |
| ------------------ | ------------------------ |
| Matrix size        | `2*n - 1`                |
| Current row/column | `i`, `j`                 |
| Border distances   | top, left, right, bottom |
| Final value        | `n - minimum_distance`   |

---

# 📊 Dry Run (n = 4)

## Row 0

```cpp
4 4 4 4 4 4 4
```

## Row 1

```cpp
4 3 3 3 3 3 4
```

## Row 2

```cpp
4 3 2 2 2 3 4
```

## Row 3

```cpp
4 3 2 1 2 3 4
```

Then pattern mirrors back.

---

# 🎯 Interview Thinking

The interviewer checks:

* Nested loop understanding
* Pattern observation
* Symmetry logic
* Mathematical thinking
* Index manipulation

---

# ⚡ Complexity Analysis

| Approach           | Time  | Space |
| ------------------ | ----- | ----- |
| Upper + Lower Half | O(n²) | O(1)  |
| Distance Formula   | O(n²) | O(1)  |

---

# 🧠 Memory Trick

👉 Think of:

```cpp
Concentric square layers
```

Outer layer:

```cpp
n
```

Inner layer:

```cpp
n-1
```

Continue until:

```cpp
1
```

---

# 🏁 Conclusion

This pattern helps improve:

* Loop control
* Symmetry building
* Matrix-style thinking
* Observation skills
* Mathematical visualization

💡 Very useful for beginners learning nested loops.

---

## ⭐ Support

If you found this helpful:

⭐ Star the repository
📌 Follow my DSA journey

---

## 👨‍💻 Author

**Hayat Ali**
💡 Brand: **codewithhsquare**
🌐 Portfolio: https://codewithhsquare.vercel.app/

---

🔥 Keep practicing patterns and DSA daily!
