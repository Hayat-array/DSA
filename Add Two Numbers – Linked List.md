# 🚀 Add Two Numbers – Linked List

<div align="center">

![LeetCode](https://img.shields.io/badge/LeetCode-2-orange?style=for-the-badge\&logo=leetcode)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Linked%20List-blue?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-C++-00599C?style=for-the-badge\&logo=cplusplus)

# 👨‍💻 CodeWithHSquare

### ✨ Mastering Linked Lists Step by Step

🔥 Beginner Friendly
🚀 Interview Focused
💡 Multiple Approaches Included

</div>

---

# 📌 Problem Statement

You are given two **non-empty linked lists** representing two non-negative integers.

* The digits are stored in **reverse order**.
* Each node contains a **single digit**.
* Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero except the number `0` itself.

---

# 🧾 Examples

## Example 1

```text
Input:
l1 = [2,4,3]
l2 = [5,6,4]

Output:
[7,0,8]

Explanation:
342 + 465 = 807
```

---

## Example 2

```text
Input:
l1 = [0]
l2 = [0]

Output:
[0]
```

---

## Example 3

```text
Input:
l1 = [9,9,9,9,9,9,9]
l2 = [9,9,9,9]

Output:
[8,9,9,9,0,0,0,1]
```

---

# 🎯 Visualization

## 🔗 Interactive Visualization

### 🌐 [https://add-two-numbers-linked-list-one.vercel.app/](https://add-two-numbers-linked-list-one.vercel.app/)

---

# 🧠 Understanding the Problem

The linked lists store numbers in reverse order.

```text
[2,4,3] → 342
[5,6,4] → 465
```

Now add them:

```text
342 + 465 = 807
```

Return the result again in reverse order:

```text
[7,0,8]
```

---

# 🔥 Core Idea

This problem works exactly like elementary school addition.

At every step:

✅ Add current digits
✅ Add carry
✅ Store last digit
✅ Forward carry to next node

---

# ✅ Approach 1 — Iterative Solution (Optimal)

---

# 💡 Intuition

We traverse both linked lists together.

At each step:

```text
sum = digit1 + digit2 + carry
```

Then:

```text
digit = sum % 10
carry = sum / 10
```

Store `digit` in the answer linked list.

---

# ❓ What Information Do I Need At Each Step?

| Information        | Why Needed         |
| ------------------ | ------------------ |
| Current node of l1 | First digit        |
| Current node of l2 | Second digit       |
| Carry              | Handle overflow    |
| Sum                | Current addition   |
| New node           | Store answer digit |

---

# ⚡ Algorithm

## Step-by-Step Algorithm

### Step 1

Create a dummy node.

### Step 2

Initialize:

```cpp
carry = 0
```

### Step 3

Traverse while:

```cpp
l1 != NULL || l2 != NULL || carry != 0
```

### Step 4

Take values from linked lists.

### Step 5

Compute total sum.

```text
sum = val1 + val2 + carry
```

### Step 6

Store current digit.

```text
digit = sum % 10
```

### Step 7

Update carry.

```text
carry = sum / 10
```

### Step 8

Create new node.

### Step 9

Move pointers forward.

### Step 10

Return answer list.

---

# ✅ Optimal C++ Code

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        // Dummy node for result
        ListNode* dummy = new ListNode(0);

        // Pointer to build answer
        ListNode* temp = dummy;

        int carry = 0;

        while(l1 != NULL || l2 != NULL || carry != 0) {

            int sum = carry;

            // Add value from l1
            if(l1 != NULL) {
                sum += l1->val;
                l1 = l1->next;
            }

            // Add value from l2
            if(l2 != NULL) {
                sum += l2->val;
                l2 = l2->next;
            }

            // Update carry
            carry = sum / 10;

            // Create node for current digit
            ListNode* newNode = new ListNode(sum % 10);

            temp->next = newNode;
            temp = temp->next;
        }

        return dummy->next;
    }
};
```

---

# 🔍 Detailed Dry Run

## Input

```text
l1 = [2,4,3]
l2 = [5,6,4]
```

---

# 🟢 Iteration 1

```text
2 + 5 + 0 = 7
```

| Value | Result |
| ----- | ------ |
| Digit | 7      |
| Carry | 0      |

Answer List:

```text
7
```

---

# 🟢 Iteration 2

```text
4 + 6 + 0 = 10
```

| Value | Result |
| ----- | ------ |
| Digit | 0      |
| Carry | 1      |

Answer List:

```text
7 → 0
```

---

# 🟢 Iteration 3

```text
3 + 4 + 1 = 8
```

| Value | Result |
| ----- | ------ |
| Digit | 8      |
| Carry | 0      |

Final Answer:

```text
7 → 0 → 8
```

---

# 📊 Complexity Analysis

| Complexity       | Value       |
| ---------------- | ----------- |
| Time Complexity  | O(max(N,M)) |
| Space Complexity | O(max(N,M)) |

---

# ✅ Why This Approach is Optimal?

✔ Single traversal
✔ Handles unequal lengths
✔ No reversing needed
✔ Easy carry handling
✔ Beginner friendly
✔ Interview efficient

---

# ✅ Approach 2 — Recursive Solution

---

# 💡 Intuition

Instead of loops, recursion automatically moves through the linked lists.

Each recursive call handles:

* Current digits
* Carry
* Remaining linked lists

---

# ⚡ Recursive Algorithm

1. If both lists and carry are empty → return NULL
2. Add current digits and carry
3. Create current node
4. Recursively solve next nodes
5. Return current node

---

# ✅ Recursive C++ Code

```cpp
class Solution {
public:

    ListNode* solve(ListNode* l1, ListNode* l2, int carry) {

        if(l1 == NULL && l2 == NULL && carry == 0)
            return NULL;

        int sum = carry;

        if(l1 != NULL)
            sum += l1->val;

        if(l2 != NULL)
            sum += l2->val;

        ListNode* node = new ListNode(sum % 10);

        carry = sum / 10;

        node->next = solve(
            l1 ? l1->next : NULL,
            l2 ? l2->next : NULL,
            carry
        );

        return node;
    }

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        return solve(l1, l2, 0);
    }
};
```

---

# 📊 Complexity Analysis

| Complexity       | Value                       |
| ---------------- | --------------------------- |
| Time Complexity  | O(max(N,M))                 |
| Space Complexity | O(max(N,M)) Recursive Stack |

---

# ⚠️ Drawbacks of Recursive Solution

❌ Uses recursive stack memory
❌ Slightly slower
❌ Risk of stack overflow for large inputs

---

# ❌ Approach 3 — Convert to Integer (Not Recommended)

---

# 💡 Idea

1. Convert linked lists into integers
2. Add both numbers
3. Convert answer back into linked list

---

# ❌ Why This Approach Fails?

Large numbers may overflow.

Example:

```text
999999999999999999999999999999
```

Most integer types cannot store such large values.

---

# 🏆 Best Approach Comparison

| Approach           | Time | Space | Recommended |
| ------------------ | ---- | ----- | ----------- |
| Iterative          | O(N) | O(N)  | ✅ BEST      |
| Recursive          | O(N) | O(N)  | ✅ Good      |
| Integer Conversion | O(N) | O(1)  | ❌ Unsafe    |

---

# 🎯 Important Interview Concepts

## 1️⃣ Dummy Node

Dummy node simplifies linked list insertion.

Without dummy node, handling head becomes complicated.

---

## 2️⃣ Carry Handling

Example:

```text
9 + 9 = 18
```

Store:

```text
Digit = 8
Carry = 1
```

---

## 3️⃣ Unequal Length Lists

Loop continues while:

```cpp
l1 || l2 || carry
```

This handles:

```text
[9,9,9]
[1]
```

---

## 4️⃣ Reverse Storage Advantage

Because digits are reversed, addition becomes easy.

No need to reverse linked lists.

---

# 🧠 Important Edge Cases

## Edge Case 1

```text
[0] + [0]
```

Output:

```text
[0]
```

---

## Edge Case 2

```text
[9,9,9] + [1]
```

Output:

```text
[0,0,0,1]
```

---

## Edge Case 3

Different sizes:

```text
[2,4]
[5,6,4]
```

---

# 🌟 Interview Follow-Up Questions

## Q1. What if digits are NOT reversed?

Possible solutions:

* Reverse both lists
* OR use stacks

---

## Q2. Why use dummy node?

To simplify insertion logic.

---

## Q3. Can we solve in-place?

Possible, but implementation becomes complicated.

---

# 🏁 Final Takeaway

This problem teaches:

✅ Linked List Traversal
✅ Carry Propagation
✅ Simulation Technique
✅ Dummy Node Pattern
✅ Recursive Thinking
✅ Pointer Manipulation

---

# 📚 Topics Covered

* Linked List
* Simulation
* Math
* Recursion
* Iteration
* Carry Handling

---

<div align="center">

# ⭐ Star This Repository If You Found It Helpful ⭐

## 🚀 Keep Practicing DSA

### 💖 Made with dedication by CodeWithHSquare

🔥 Consistency + Practice = Success 🔥

</div>
