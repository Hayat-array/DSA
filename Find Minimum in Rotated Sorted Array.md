# 153. Find Minimum in Rotated Sorted Array

<div align="center">

# 🔍 Find Minimum in Rotated Sorted Array

<img src="https://media.giphy.com/media/l0HlBO7eyXzSZkJri/giphy.gif" width="500"/>

![C++](https://img.shields.io/badge/Language-C++-blue.svg)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange.svg)
![Binary Search](https://img.shields.io/badge/Topic-Binary_Search-green.svg)

</div>

---

# 📌 Problem Statement

Given a rotated sorted array containing unique elements, return the minimum element.

A sorted array is rotated between `1` and `n` times.

## Example

```cpp
Input: nums = [4,5,6,7,0,1,2]
Output: 0
```

---

# 🧠 Understanding Rotation

## Original Sorted Array

```text
0 1 2 4 5 6 7
```

## After Rotation

```text
4 5 6 7 | 0 1 2
```

Minimum element becomes the pivot point.

---

# 🎥 Visual Animation

<div align="center">

```text
Step 1

4 5 6 7 | 0 1 2
          ↑
        Minimum
```

```text
Step 2

Check Mid Element

4 5 6 7 | 0 1 2
      ↑
     mid
```

```text
nums[mid] > nums[last]

Minimum lies on RIGHT side
```

```text
4 5 6 7 | 0 1 2
            ↑
           search
```

</div>

---

# 🚀 Approach 1 — Brute Force

## 💡 Idea

Traverse the entire array and find the minimum element.

---

# 📝 Algorithm

1. Initialize minimum as first element.
2. Traverse array.
3. Update minimum.
4. Return minimum.

---

# 💻 Code

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int mini = nums[0];

        for(int i = 1; i < nums.size(); i++) {
            mini = min(mini, nums[i]);
        }

        return mini;
    }
};
```

---

# 🧪 Dry Run

## Input

```cpp
nums = [4,5,6,7,0,1,2]
```

| Index | Element | Minimum |
| ----- | ------- | ------- |
| 0     | 4       | 4       |
| 1     | 5       | 4       |
| 2     | 6       | 4       |
| 3     | 7       | 4       |
| 4     | 0       | 0       |
| 5     | 1       | 0       |
| 6     | 2       | 0       |

## Output

```cpp
0
```

---

# ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n)  |
| Space      | O(1)  |

---

# 🚀 Approach 2 — Binary Search (Optimal)

<div align="center">

<img src="https://media.giphy.com/media/26ufdipQqU2lhNA4g/giphy.gif" width="450"/>

</div>

---

# 💡 Observation

In rotated sorted array:

```text
4 5 6 7 | 0 1 2
```

Elements before minimum are:

```text
> last element
```

Elements after minimum are:

```text
<= last element
```

This creates:

```text
true true true false false false
```

Perfect for Binary Search.

---

# 📝 Algorithm

## What information do we need at each step?

| Variable    | Purpose            |
| ----------- | ------------------ |
| low         | Search start       |
| high        | Search end         |
| mid         | Middle element     |
| nums[mid]   | Decide direction   |
| nums.back() | Compare pivot side |

---

## Steps

1. Initialize low and high.
2. Find middle element.
3. If `nums[mid] > nums.back()`:

   * Minimum lies right.
4. Else:

   * Minimum lies left including mid.
5. Continue until low == high.
6. Return nums[low].

---

# 💻 Code

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] > nums.back()) {
                low = mid + 1;
            }
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🎯 Binary Search Animation

## Initial State

```text
Index : 0 1 2 3 4 5 6
Array : 4 5 6 7 0 1 2

low = 0
high = 6
mid = 3
```

```text
nums[mid] = 7
nums[last] = 2

7 > 2
```

Move Right

```text
4 5 6 7 | 0 1 2
          ↑
         low
```

---

## Second Iteration

```text
low = 4
high = 6
mid = 5
```

```text
nums[mid] = 1
nums[last] = 2

1 <= 2
```

Move Left

```text
0 1 2
↑
high
```

---

## Final

```text
low = high = 4
```

```text
nums[4] = 0
```

✅ Answer Found

---

# 🧪 Detailed Dry Run

## Input

```cpp
nums = [4,5,6,7,0,1,2]
```

| low | high | mid | nums[mid] | Action     |
| --- | ---- | --- | --------- | ---------- |
| 0   | 6    | 3   | 7         | Move Right |
| 4   | 6    | 5   | 1         | Move Left  |
| 4   | 5    | 4   | 0         | Move Left  |

Final:

```cpp
low = 4
nums[4] = 0
```

---

# ⏱ Complexity

| Complexity | Value    |
| ---------- | -------- |
| Time       | O(log n) |
| Space      | O(1)     |

---

# 🚀 Approach 3 — Using partition_point()

---

# 💡 Idea

Use STL binary search utility.

Condition:

```cpp
n > nums.back()
```

Pattern becomes:

```text
true true true false false false
```

`partition_point()` finds first false.

---

# 💻 Code Using Lambda

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        return *partition_point(
            nums.begin(),
            nums.end(),
            [&](int n) {
                return n > nums.back();
            }
        );
    }
};
```

---

# 💻 Code Without Lambda

```cpp
class Compare {
    int last;

public:
    Compare(int x) : last(x) {}

    bool operator()(int n) {
        return n > last;
    }
};

class Solution {
public:
    int findMin(vector<int>& nums) {
        return *partition_point(
            nums.begin(),
            nums.end(),
            Compare(nums.back())
        );
    }
};
```

---

# 🧪 Dry Run

## Input

```cpp
nums = [4,5,6,7,0,1,2]
```

## Predicate Check

| Element | n > 2 |
| ------- | ----- |
| 4       | true  |
| 5       | true  |
| 6       | true  |
| 7       | true  |
| 0       | false |
| 1       | false |
| 2       | false |

Pattern:

```text
true true true true false false false
```

First false:

```text
0
```

Answer:

```cpp
0
```

---

# 📊 Comparison of All Approaches

| Approach        | Time     | Space | Best? |
| --------------- | -------- | ----- | ----- |
| Brute Force     | O(n)     | O(1)  | ❌     |
| Binary Search   | O(log n) | O(1)  | ✅     |
| partition_point | O(log n) | O(1)  | ✅     |

---

# ⭐ Important Interview Points

## Why Binary Search Works?

Because rotated sorted array is divided into:

```text
Large values + Small values
```

which creates monotonic condition.

---

# ⚠ Common Mistakes

## ❌ Wrong

```cpp
mid = (low + high) / 2;
```

May overflow.

---

## ✅ Correct

```cpp
mid = low + (high - low) / 2;
```

---

# 🎯 Key Learning

✔ Rotated array problems often use Binary Search.

✔ Compare with last element to identify pivot side.

✔ `partition_point()` internally uses Binary Search.

✔ Learn monotonic conditions.

---

# 🏆 Final Optimal Solution

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] > nums.back()) {
                low = mid + 1;
            }
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

<div align="center">

# ⭐ If this helped you, star your repository ⭐

### Made with ❤️ by CodeWithHSquare

</div>
