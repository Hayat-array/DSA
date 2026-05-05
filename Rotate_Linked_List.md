# 🔁 Rotate Linked List (LeetCode 61)

<div align="center">

### 👨‍💻 by **codewithhsquare (Hayat Ali)**

🚀 *Striver DSA Sheet Journey*

</div>

---

## 🧾 Problem Statement

Given the head of a singly linked list, rotate the list to the right by **k places**.

---

## 📌 Examples

```text
Input:  head = [1,2,3,4,5], k = 2  
Output: [4,5,1,2,3]
```

```text
Input:  head = [0,1,2], k = 4  
Output: [2,0,1]
```

---

# 🧠 Approach 1: Two Pointer Technique

## 💡 Intuition

* Move the **fast pointer k steps ahead**
* Move **slow & fast together**
* When fast reaches the end → slow is at the **new tail**

---

## ⚙️ Algorithm (Two Pointer)

```
1. If head is NULL or k = 0 → return head
2. Find length n of the list
3. Compute k = k % n
4. If k == 0 → return head
5. Move fast pointer k steps ahead
6. Initialize slow = head
7. Move both slow and fast until fast reaches last node
8. newHead = slow->next
9. Break link: slow->next = NULL
10. Connect last node to head: fast->next = head
11. Return newHead
```

---

## 💻 Code (Two Pointer)

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next || k == 0) return head;

        int n = 0;
        ListNode* temp = head;
        while (temp) {
            n++;
            temp = temp->next;
        }

        k = k % n;
        if (k == 0) return head;

        ListNode* fast = head;
        for (int i = 0; i < k; i++) {
            fast = fast->next;
        }

        ListNode* slow = head;
        while (fast->next) {
            slow = slow->next;
            fast = fast->next;
        }

        ListNode* newHead = slow->next;
        slow->next = NULL;
        fast->next = head;

        return newHead;
    }
};
```

---

# 🔵 Approach 2: Circular Linked List Method

## 💡 Intuition

* Convert the list into a **circular linked list**
* Find the new tail at position **(n - k)**
* Break the circle to get rotated list

---

## ⚙️ Algorithm (Circular Method)

```
1. If head is NULL or k = 0 → return head
2. Traverse list to find length n and last node
3. Compute k = k % n
4. If k == 0 → return head
5. Connect last node to head (make circular)
6. Move (n - k - 1) steps from head to find newTail
7. newHead = newTail->next
8. Break link: newTail->next = NULL
9. Return newHead
```

---

## 💻 Code (Circular Method)

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next || k == 0) return head;

        // Step 1: Find length
        ListNode* temp = head;
        int n = 1;
        while (temp->next) {
            temp = temp->next;
            n++;
        }

        // Step 2: Optimize k
        k = k % n;
        if (k == 0) return head;

        // Step 3: Make circular
        temp->next = head;

        // Step 4: Find new tail
        int steps = n - k;
        ListNode* newTail = head;
        for (int i = 1; i < steps; i++) {
            newTail = newTail->next;
        }

        // Step 5: Break
        ListNode* newHead = newTail->next;
        newTail->next = NULL;

        return newHead;
    }
};
```

---

## ⚡ Complexity Comparison

| Approach    | Time | Space |
| ----------- | ---- | ----- |
| Two Pointer | O(n) | O(1)  |
| Circular    | O(n) | O(1)  |

---

## ❗ Common Mistakes

* ❌ Incorrect length calculation
* ❌ Forgetting `k = k % n`
* ❌ Not breaking circular list
* ❌ Off-by-one pointer errors

---

## 🧠 Memory Trick

> 👉 Two Pointer: **Fast ahead → slow cuts**
> 👉 Circular: **Make circle → move → break**

---

## 🌟 Support

⭐ Star your repo
🔥 Follow **codewithhsquare**
🚀 Keep grinding DSA
