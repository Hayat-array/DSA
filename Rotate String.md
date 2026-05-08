# 🔄 Rotate String (LeetCode 796)

<h2 align="center">🚀 Multiple Approaches | Optimized Solutions | Interview Notes</h2>

<p align="center">
  <img src="https://img.shields.io/badge/Level-Easy-brightgreen?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topic-String-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Platform-LeetCode-orange?style=for-the-badge"/>
</p>

---

# 🧩 Problem Statement

Given two strings `s` and `goal`, return `true` if and only if `s` can become `goal` after some number of shifts on `s`.

A shift on `s` consists of moving the leftmost character to the rightmost position.

---

## 📌 Example

### Input

```id="ex1"
s = "abcde"
goal = "cdeab"
```

### Output

```id="out1"
true
```

---

# 🧠 Core Idea

If we concatenate:

```id="core1"
s + s
```

Then every possible rotation of `s` will exist inside it.

Example:

```id="core2"
s = "abcde"

s + s = "abcdeabcde"
```

Now:

```id="core3"
"cdeab"
```

exists inside:

```id="core4"
"abcdeabcde"
```

So answer = `true`

---

# 🚀 Approach 1: Using `search()` STL Function

## 🔹 Idea

* Combine string:

```id="a1"
s + s
```

* Use STL `search()` to find `goal`

---

## 💻 Code

```cpp id="code1"
class Solution {
public:
    bool rotateString(string s, string goal) {
        if(s.size() != goal.size()) return false;

        string temp = s + s;

        return (search(temp.begin(), temp.end(),
                       goal.begin(), goal.end()) != temp.end());
    }
};
```

---

## ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n²) |
| Space      | O(n)  |

---

# 🚀 Approach 2: Using `find()` Function (Best & Clean)

## 🔹 Idea

Use:

```id="a2"
(s + s).find(goal)
```

If found:

```id="a3"
!= string::npos
```

then rotation exists.

---

## 💻 Code

```cpp id="code2"
class Solution {
public:
    bool rotateString(string s, string goal) {
        if(s.size() != goal.size()) return false;

        return (s + s).find(goal) != string::npos;
    }
};
```

---

## ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n²) |
| Space      | O(n)  |

---

# 🚀 Approach 3: Manual Rotation Simulation

## 🔹 Idea

Rotate string one-by-one manually.

Example:

```id="a4"
abcde
bcdea
cdeab
```

Compare after every rotation.

---

## 💻 Code

```cpp id="code3"
class Solution {
public:
    bool rotateString(string s, string goal) {
        if(s.size() != goal.size()) return false;

        int n = s.size();

        for(int i = 0; i < n; i++) {

            char first = s[0];

            s = s.substr(1) + first;

            if(s == goal)
                return true;
        }

        return false;
    }
};
```

---

## ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n²) |
| Space      | O(n)  |

---

# 🚀 Approach 4: Using `substr()` + `compare()`

## 🔹 Idea

* Create:

```id="a5"
temp = s + s
```

* Check every substring of size:

```id="a6"
s.size()
```

* Compare with `goal`

---

## 💻 Code

```cpp id="code4"
class Solution {
public:
    bool rotateString(string s, string goal) {

        if(s.size() != goal.size())
            return false;

        string temp = s + s;

        for(int i = 0; i < s.size(); i++) {

            if(temp.substr(i, s.size()).compare(goal) == 0)
                return true;
        }

        return false;
    }
};
```

---

## ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n²) |
| Space      | O(n)  |

---

# 📊 Comparison of Approaches

| Approach                 | Best Feature                    |
| ------------------------ | ------------------------------- |
| `search()`               | STL knowledge                   |
| `find()`                 | Most clean & interview favorite |
| Manual rotation          | Easy to understand              |
| `substr()` + `compare()` | Good string practice            |

---

# 🧠 What Information Do I Need at Each Step?

| Step            | Required Information                  |
| --------------- | ------------------------------------- |
| Length check    | `s.size() == goal.size()`             |
| Rotation logic  | `s + s`                               |
| Search target   | `goal`                                |
| Manual rotation | first character + remaining substring |

---

# 🎯 Interview Tip

The interviewer usually expects:

```id="tip1"
(s + s).find(goal)
```

because it is:

* Clean
* Short
* Efficient
* Easy to explain

---

# ⚡ Complexity Summary

| Method           | Time  | Space |
| ---------------- | ----- | ----- |
| search()         | O(n²) | O(n)  |
| find()           | O(n²) | O(n)  |
| Manual Rotation  | O(n²) | O(n)  |
| substr + compare | O(n²) | O(n)  |

---

# 🧠 Memory Trick

👉 Every rotation of string `s` always exists inside:

```id="trick1"
s + s
```

---

# 🏁 Conclusion

This problem teaches:

* String manipulation
* Rotation concepts
* STL usage
* Substring searching

💡 A very common beginner-to-intermediate interview problem.

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

🔥 Keep grinding DSA daily!
