# 🔢 2553. Separate the Digits in an Array

> LeetCode Easy Problem
> Topic: Array, Simulation
> Difficulty: Easy

---

# 📘 Problem Statement

Given an array of positive integers `nums`, return an array `answer` that consists of the digits of each integer in `nums` after separating them in the same order they appear.

To separate the digits of an integer means extracting every digit individually while maintaining the original order.

---

# 🧾 Example 1

## Input

```cpp
nums = [13,25,83,77]
```

## Output

```cpp
[1,3,2,5,8,3,7,7]
```

## Explanation

* 13 → [1,3]
* 25 → [2,5]
* 83 → [8,3]
* 77 → [7,7]

Final Answer:

```cpp
[1,3,2,5,8,3,7,7]
```

---

# 🧾 Example 2

## Input

```cpp
nums = [7,1,3,9]
```

## Output

```cpp
[7,1,3,9]
```

---

# 📌 Constraints

```cpp
1 <= nums.length <= 1000
1 <= nums[i] <= 10^5
```

---

# 🧠 Understanding the Problem

We need to:

1. Traverse every number in the array.
2. Extract all digits from that number.
3. Store digits in the same order.
4. Return the final digit array.

---

# ❓ What Information Do We Need at Each Step?

| Step | Required Information           |
| ---- | ------------------------------ |
| 1    | Current number from array      |
| 2    | Digits inside the number       |
| 3    | Correct order of digits        |
| 4    | Store digits into final answer |

---

# 🚀 Approach 1 — String Conversion (Easy & Beginner Friendly)

## 💡 Idea

* Convert number into string.
* Traverse each character.
* Convert character back to integer digit.
* Store into answer vector.

---

# ✅ Algorithm

1. Create an empty result vector.
2. Traverse every number in nums.
3. Convert number to string.
4. Traverse characters of string.
5. Convert character to digit using `c - '0'`.
6. Push digit into result.
7. Return result.

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

# 🚀 Approach 2 — Using Mathematics (Without String)

## 💡 Idea

* Extract digits using `% 10`.
* Digits come in reverse order.
* Reverse temporary vector.
* Add digits into final answer.

---

# ✅ Algorithm

1. Create result vector.
2. Traverse every number.
3. Extract digits using modulo `% 10`.
4. Store digits into temporary vector.
5. Reverse temporary vector.
6. Append into result.
7. Return result.

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

## 💡 Idea

This is a clever mathematical optimization.

Normally:

* Digits extracted using `%10` come reversed.
* So we reverse them.

But here:

* We traverse the original array backward.
* Extract reversed digits.
* Finally reverse the whole result array.

Both reversals cancel each other.

---

# ✅ Algorithm

1. Traverse array from back to front.
2. Extract digits using `% 10`.
3. Store directly into result.
4. Reverse the final result vector.
5. Return answer.

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

## Input

```cpp
nums = [13,25]
```

---

## Process

### Traverse Backward

### 25

Extract digits:

```cpp
5
2
```

Result:

```cpp
[5,2]
```

---

### 13

Extract digits:

```cpp
3
1
```

Result:

```cpp
[5,2,3,1]
```

---

## Reverse Final Array

```cpp
[1,3,2,5]
```

✅ Correct Answer

---

# 🎯 Key Interview Points

✔ Maintain digit order carefully
✔ `%10` extracts digits in reverse order
✔ String method is easiest
✔ Math method shows strong fundamentals
✔ Reverse traversal trick is clever and optimized

---

# 🏁 Final Thoughts

This problem is a great beginner-level array and simulation problem.

It teaches:

* Digit extraction
* Array traversal
* Reversing techniques
* Multiple problem-solving approaches

---

# ⭐ If You Found This Helpful

Give this repository a ⭐ and follow for more DSA solutions.

---

# 👨‍💻 Author

## CodeWithHSquare

🚀 DSA | Web Development | AI/ML | Competitive Programming
