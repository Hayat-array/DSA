# 🔢 2553. Separate the Digits in an Array

<div align="center">

![LeetCode](https://img.shields.io/badge/LeetCode-Easy-brightgreen?style=for-the-badge\&logo=leetcode)
![C++](https://img.shields.io/badge/C%2B%2B-17-blue?style=for-the-badge\&logo=c%2B%2B)
![DSA](https://img.shields.io/badge/DSA-Array-orange?style=for-the-badge)
![CodeWithHSquare](https://img.shields.io/badge/CodeWithHSquare-DSA-red?style=for-the-badge)

</div>

---

# 🌟 Problem Overview

Given an array of positive integers `nums`, return an array containing all digits of every number in the same order.

Each number must be separated digit-by-digit while preserving the original sequence.

---

---

# 📘 Problem Statement

You are given an array of positive integers `nums`.

Your task is to create a new array `answer` such that:

* Every integer in `nums` is separated into individual digits.
* Digits must appear in the same order as the original number.
* Final order of all digits must remain preserved.

---

# 🧾 Example 1

## ✅ Input

```cpp
nums = [13,25,83,77]
```

## ✅ Output

```cpp
[1,3,2,5,8,3,7,7]
```

## 🔍 Explanation

| Number | Separated Digits |
| ------ | ---------------- |
| 13     | [1,3]            |
| 25     | [2,5]            |
| 83     | [8,3]            |
| 77     | [7,7]            |

### Final Answer

```cpp
[1,3,2,5,8,3,7,7]
```

---

# 🧾 Example 2

## ✅ Input

```cpp
nums = [7,1,3,9]
```

## ✅ Output

```cpp
[7,1,3,9]
```

## 🔍 Explanation

All numbers already contain a single digit.

---

# 📌 Constraints

```cpp
1 <= nums.length <= 1000
1 <= nums[i] <= 10^5
```

---

# 🧠 Problem Understanding

We need to:

✅ Traverse every number
✅ Extract all digits
✅ Maintain correct order
✅ Store digits in a new array

---

# ✨ Visualization Project

<div align="center">

## 🚀 Interactive Visualization

### 🔗 [https://separate-the-digits-in-an-array.vercel.app/](https://separate-the-digits-in-an-array.vercel.app/)

</div>

---

# ❓ What Information Do We Need at Each Step?

| Step | Required Information          |
| ---- | ----------------------------- |
| 1    | Current number from array     |
| 2    | Individual digits of number   |
| 3    | Correct order of digits       |
| 4    | Store digits in answer vector |

---

# 🚀 Approach 1 — String Conversion Method

<div align="center">

## ⭐ Beginner Friendly Approach

</div>

---

# 💡 Idea

Convert every number into a string.

Then:

* Traverse every character.
* Convert character back to integer.
* Store into result vector.

---

# ✅ Algorithm

```text
1. Create an empty result vector.
2. Traverse every number in nums.
3. Convert number into string.
4. Traverse each character.
5. Convert character into digit.
6. Push digit into result.
7. Return result.
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        vector<int> result;

        for (int num : nums) {
            string s = to_string(num);

            for (char c : s) {
                result.push_back(c - '0');
            }
        }

        return result;
    }
};
```

---

# 📊 Complexity Analysis

| Complexity       | Value         |
| ---------------- | ------------- |
| Time Complexity  | O(N × digits) |
| Space Complexity | O(N × digits) |

---

# 🚀 Approach 2 — Mathematical Digit Extraction

<div align="center">

## ⭐ No String Conversion

</div>

---

# 💡 Idea

Use mathematics:

* `% 10` gives last digit.
* `/ 10` removes last digit.

Since digits come in reverse order:

➡ Reverse temporary vector.

---

# ✅ Algorithm

```text
1. Create result vector.
2. Traverse every number.
3. Extract digits using modulo.
4. Store digits in temporary vector.
5. Reverse temporary vector.
6. Append digits to result.
7. Return answer.
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        vector<int> result;

        for (int num : nums) {
            vector<int> temp;

            while (num > 0) {
                temp.push_back(num % 10);
                num /= 10;
            }

            reverse(temp.begin(), temp.end());

            for (int digit : temp) {
                result.push_back(digit);
            }
        }

        return result;
    }
};
```

---

# 📊 Complexity Analysis

| Complexity       | Value         |
| ---------------- | ------------- |
| Time Complexity  | O(N × digits) |
| Space Complexity | O(N × digits) |

---

# 🚀 Approach 3 — Optimized Reverse Traversal Trick

<div align="center">

## ⭐ Clever Mathematical Optimization

</div>

---

# 💡 Core Observation

Normally:

* `%10` extracts digits in reverse order.
* So we reverse digits later.

But here:

✅ Traverse original array backward
✅ Extract reversed digits
✅ Reverse final result once

Both reversals cancel each other.

---

# ✅ Algorithm

```text
1. Traverse nums from back to front.
2. Extract digits using %10.
3. Push digits directly into result.
4. Reverse final result vector.
5. Return answer.
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        vector<int> res;

        for (int i = nums.size() - 1; i >= 0; i--) {
            int x = nums[i];

            while (x > 0) {
                res.push_back(x % 10);
                x /= 10;
            }
        }

        reverse(res.begin(), res.end());

        return res;
    }
};
```

---

# 📊 Complexity Analysis

| Complexity       | Value         |
| ---------------- | ------------- |
| Time Complexity  | O(N × digits) |
| Space Complexity | O(N × digits) |

---

# 🔍 Dry Run

# ✅ Input

```cpp
nums = [13,25]
```

---

# ⚙ Step-by-Step Process

## Traverse Backward

### Number = 25

Extract digits:

```cpp
25 % 10 = 5
2 % 10 = 2
```

Result:

```cpp
[5,2]
```

---

### Number = 13

Extract digits:

```cpp
13 % 10 = 3
1 % 10 = 1
```

Result:

```cpp
[5,2,3,1]
```

---

# 🔄 Reverse Final Array

```cpp
[1,3,2,5]
```

✅ Correct Answer

---

# 🎯 Interview Tips

✔ Maintain correct order carefully
✔ `%10` extracts digits from right side
✔ String method is easiest to explain
✔ Math method shows strong fundamentals
✔ Reverse traversal trick impresses interviewers

---

# 🏆 Key Learnings

After solving this problem, you learn:

* Array Traversal
* Digit Extraction
* Reversing Techniques
* Simulation Problems
* Multiple Approach Thinking

---

# 🏁 Final Thoughts

This is an excellent beginner-level problem for improving:

✅ Logical thinking
✅ Array manipulation
✅ Mathematical operations
✅ Problem-solving confidence

---

<div align="center">

# ⭐ Support the Repository

If you found this helpful:

⭐ Star the repository
🍴 Fork the project
📢 Share with friends

</div>

---

# 👨‍💻 Author

<div align="center">

# CodeWithHSquare

🚀 DSA | Competitive Programming | AI/ML | Web Development

</div>
