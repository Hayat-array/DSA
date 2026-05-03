# 🔄 Rotate String & C++ String Functions

## 📌 Overview

This project demonstrates:

* Checking if one string is a rotation of another
* Understanding important C++ string functions:

  * `find()`
  * `compare()`
  * `search()` (STL)
* Performance comparison of different approaches

---

# 🔁 1. Rotate String Problem

### ✅ Problem

Check if string `goal` is a rotation of string `s`.

### 📖 Example

```text
s = "abcde"
goal = "cdeab" → true
```

---

## 🚀 Optimal Solution (Using `find()`)

```cpp
#include <iostream>
using namespace std;

bool rotateString(string s, string goal) {
    if(s.size() != goal.size()) return false;
    return (s + s).find(goal) != string::npos;
}

int main() {
    string s = "abcde";
    string goal = "cdeab";

    cout << (rotateString(s, goal) ? "True" : "False");
}
```

---

## 🧠 Idea

If `goal` is a rotation of `s`,
then it must be a substring of:

```text
s + s
```

---

# 🔍 2. String Functions in C++

---

## 🔹 2.1 `find()`

### 📌 Syntax

```cpp
s.find(substring)
```

### 📖 Example

```cpp
string s = "hello world";
int pos = s.find("world");
```

### 🔁 Return

* Index → if found
* `string::npos` → if not found

### ⏱️ Complexity

**O(n)**

---

## 🔹 2.2 `compare()`

### 📌 Syntax

```cpp
s1.compare(s2)
```

### 📖 Example

```cpp
string a = "abc";
string b = "abc";

if(a.compare(b) == 0)
    cout << "Equal";
```

### 🔁 Return

* `0` → equal
* `<0` → smaller
* `>0` → greater

### ⏱️ Complexity

**O(n)**

---

## 🔹 2.3 `search()` (STL)

### 📌 Syntax

```cpp
search(start1, end1, start2, end2)
```

### 📖 Example

```cpp
#include <algorithm>

string s = "abcdeabcde";
string goal = "cdeab";

auto it = search(s.begin(), s.end(), goal.begin(), goal.end());

if(it != s.end())
    cout << "Found";
```

### 🔁 Return

* Iterator → if found
* `end()` → if not found

### ⏱️ Complexity

**O(n * m)**

---

# ⚡ 3. Performance Comparison

| Function    | Use Case         | Complexity | Recommendation |
| ----------- | ---------------- | ---------- | -------------- |
| `find()`    | Substring search | O(n)       | ⭐ Best         |
| `compare()` | Equality check   | O(n)       | Good           |
| `search()`  | Generic search   | O(n*m)     | Advanced       |

---

# 🧠 Key Concepts

* Rotation check using string doubling
* Efficient substring search
* STL vs built-in string methods
* Time complexity awareness

---

# 📌 Conclusion

* Use **`find()`** for optimal rotation check
* Use **`compare()`** for string equality
* Use **`search()`** when working with generic containers

---

# ⭐ Author

C++ DSA practice – String Algorithms
