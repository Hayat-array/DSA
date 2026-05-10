# 🚀 Add Two Numbers – Linked List (LeetCode 2)

<div align="center">

## 👨‍💻 by **codewithhsquare (Hayat Ali)**

🔥 *Striver DSA Sheet Journey*
💡 *Mastering Linked Lists Step by Step*

</div>

---

# 📌 Problem Statement

You are given two non-empty linked lists representing two non-negative integers.

* Digits are stored in **reverse order**
* Each node contains a single digit
* Add the two numbers and return the sum as a linked list

You may assume the two numbers do not contain leading zeroes except the number `0`.

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

# 🧠 Understanding the Problem

Since digits are stored in reverse order:

```text
[2,4,3] → 342
[5,6,4] → 465
```

Addition:

```text
342 + 465 = 807
```

Return in reverse order:

```text
[7,0,8]
```

---

# ✅ Approach 1 — Iterative Solution (Optimal)

---

# 💡 Intuition

We perform addition exactly like elementary school addition.

At every step:

* Add digits
* Add carry
* Store current digit
* Forward carry to next node

---

# ❓ What Information Do I Need At Each Step?

| Information        | Purpose                 |
| ------------------ | ----------------------- |
| Current node of l1 | Get first digit         |
| Current node of l2 | Get second digit        |
| Carry              | Handle overflow         |
| Sum                | Calculate current digit |
| New node           | Store answer digit      |

---

# ⚡ Algorithm

## Step-by-Step

1. Create a dummy node.
2. Initialize `carry = 0`.
3. Traverse both linked lists.
4. Take values from current nodes.
5. Compute:

```text
sum = val1 + val2 + carry
```

6. Store:

```text
digit = sum % 10
```

7. Update carry:

```text
carry = sum / 10
```

8. Create new node with digit.
9. Move pointers forward.
10. Return `dummy->next`.

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

        ListNode* dummy = new ListNode(0);
        ListNode* temp = dummy;

        int carry = 0;

        while(l1 != NULL || l2 != NULL || carry != 0) {

            int sum = carry;

            if(l1 != NULL) {
                sum += l1->val;
                l1 = l1->next;
            }

            if(l2 != NULL) {
                sum += l2->val;
                l2 = l2->next;
            }

            carry = sum / 10;

            ListNode* newNode = new ListNode(sum % 10);

            temp->next = newNode;
            temp = temp->next;
        }

        return dummy->next;
    }
};
```

---

# 🔍 Dry Run

## Input

```text
l1 = [2,4,3]
l2 = [5,6,4]
```

---

## Iteration 1

```text
2 + 5 + 0 = 7

digit = 7
carry = 0
```

Result:

```text
7
```

---

## Iteration 2

```text
4 + 6 + 0 = 10

digit = 0
carry = 1
```

Result:

```text
7 → 0
```

---

## Iteration 3

```text
3 + 4 + 1 = 8

digit = 8
carry = 0
```

Result:

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
✔ No reversing needed
✔ Handles carry efficiently
✔ Works for unequal sizes
✔ Simple implementation

---

# ✅ Approach 2 — Recursive Solution

---

# 💡 Intuition

Instead of loops, recursion automatically moves through linked lists.

Each recursive call handles:

* Current digits
* Carry
* Next nodes

---

# ⚡ Recursive Algorithm

1. If both lists and carry are empty → return NULL
2. Add current values and carry
3. Create node using:

```text
digit = sum % 10
```

4. Recursive call for next nodes
5. Attach returned node

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

# ⚠️ Recursive Drawbacks

❌ Uses recursive stack space
❌ Slightly slower
❌ Risk of stack overflow for huge input

---

# ❌ Approach 3 — Convert to Integer (Not Recommended)

---

# 💡 Idea

1. Convert linked list into integer
2. Add integers
3. Convert answer back into linked list

---

# ❌ Why This Fails?

Numbers can become extremely large.

Example:

```text
9999999999999999999999999
```

Integer overflow occurs.

---

# ❌ Complexity

| Complexity       | Value  |
| ---------------- | ------ |
| Time Complexity  | O(N+M) |
| Space Complexity | O(1)   |

But practically unsafe.

---

# 🏆 Best Approach

| Approach           | Recommended |
| ------------------ | ----------- |
| Iterative          | ✅ BEST      |
| Recursive          | ✅ Good      |
| Integer Conversion | ❌ Avoid     |

---

# 🎯 Key Interview Points

## Important Concepts

### 1️⃣ Dummy Node

Helps simplify insertion.

---

### 2️⃣ Carry Handling

Very important:

```text
9 + 9 = 18
```

Store:

```text
8
carry = 1
```

---

### 3️⃣ Unequal Length Lists

Loop continues while:

```cpp
l1 || l2 || carry
```

---

### 4️⃣ Reverse Storage Advantage

No need to reverse lists.

---

# 🧠 Important Edge Cases

## Case 1

```text
[0] + [0]
```

Output:

```text
[0]
```

---

## Case 2

```text
[9,9,9] + [1]
```

Output:

```text
[0,0,0,1]
```

---

## Case 3

Different lengths:

```text
[2,4]
[5,6,4]
```

---

# 🌟 Interview Follow-Up Questions

## Q1. What if digits are NOT reversed?

Then:

* Reverse both lists first
* OR use stacks

---

## Q2. Why use dummy node?

To avoid handling special head cases separately.

---

## Q3. Can we solve without extra list?

Possible but messy and unsafe.

---

# 🏁 Final Takeaway

This problem teaches:

✅ Linked List Traversal
✅ Carry Handling
✅ Dummy Node Technique
✅ Simulation Problems
✅ Recursive Thinking

---

<div align="center">

# ⭐ If this helped you, star your DSA repository ⭐

### 🔥 Keep Grinding — FAANG Journey Continues 🚀

</div>
