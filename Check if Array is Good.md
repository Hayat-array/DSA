# 2784. Check if Array is Good

## Problem Statement

You are given an integer array `nums`.

An array is called **good** if it is a permutation of:

```text
base[n] = [1,2,3,...,n-1,n,n]
```

Meaning:

* Numbers from `1` to `n-1` appear exactly once
* Number `n` appears exactly twice
* Length of array becomes `n + 1`

Return `true` if the array is good otherwise return `false`.

---

# Example

## Example 1

Input:

```cpp
nums = [1,3,3,2]
```

Output:

```cpp
true
```

Explanation:

After rearranging:

```text
[1,2,3,3]
```

which matches:

```text
base[3]
```

---

# Observation

If maximum element is `n`, then:

* Array size must be `n + 1`
* Numbers `1` to `n-1` appear once
* Number `n` appears twice

---

# Approach 1 — Frequency Counting (Optimal)

## Intuition

We count how many times each number appears.

Then verify:

* `1` to `n-1` appear exactly once
* `n` appears exactly twice

---

## What information do I need at each step?

| Step            | Information Needed        |
| --------------- | ------------------------- |
| Find n          | Maximum element           |
| Validate size   | Array size should be n+1  |
| Count frequency | Occurrence of each number |
| Verify          | Required frequencies      |

---

## Algorithm

1. Find maximum element `mx`
2. Check if size is `mx + 1`
3. Create frequency array
4. Count occurrences
5. Verify:

   * `1..mx-1` appear once
   * `mx` appears twice
6. Return answer

---

## Dry Run

### Input

```cpp
nums = [1,3,3,2]
```

### Step 1

```text
Maximum = 3
```

Expected array:

```text
[1,2,3,3]
```

---

### Step 2

Size check:

```text
nums.size() = 4
mx + 1 = 4
```

Valid ✅

---

### Step 3

Frequency Table:

| Number | Count |
| ------ | ----- |
| 1      | 1     |
| 2      | 1     |
| 3      | 2     |

Valid ✅

Answer = true

---

## Animation (Step-by-Step Visualization)

```text
nums = [1,3,3,2]

Find Maximum
        ↓
mx = 3

Expected Pattern
        ↓
[1,2,3,3]

Count Frequency
        ↓
1 → 1 time
2 → 1 time
3 → 2 times

All Conditions Satisfied
        ↓
RETURN TRUE
```

---

## C++ Code (Optimal)

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int mx = *max_element(nums.begin(), nums.end());

        // Size must be mx + 1
        if (nums.size() != mx + 1)
            return false;

        vector<int> freq(mx + 1, 0);

        // Count frequency
        for (int x : nums)
            freq[x]++;

        // Check 1 to mx-1
        for (int i = 1; i < mx; i++) {
            if (freq[i] != 1)
                return false;
        }

        // mx should appear twice
        return freq[mx] == 2;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n)  |
| Space      | O(n)  |

---

# Approach 2 — Sorting Method

## Intuition

If array is good, then after sorting:

```text
[1,2,3,...,n,n]
```

So we simply sort and verify pattern.

---

## What information do I need at each step?

| Step               | Information Needed          |
| ------------------ | --------------------------- |
| Sort array         | Arrange in increasing order |
| Find n             | Last element                |
| Validate sequence  | Check 1..n                  |
| Validate duplicate | Last number repeated        |

---

## Algorithm

1. Sort the array
2. Let `n = nums.size() - 1`
3. Check:

   * `nums[i] == i+1` for first `n` elements
4. Check last element is also `n`
5. Return answer

---

## Dry Run

### Input

```cpp
nums = [1,3,3,2]
```

### After Sorting

```text
[1,2,3,3]
```

Check:

```text
1 == 1 ✅
2 == 2 ✅
3 == 3 ✅
Last element = 3 ✅
```

Return TRUE

---

## Animation (Sorting Visualization)

```text
nums = [1,3,3,2]

Sort Array
      ↓
[1,2,3,3]

Check Sequence
      ↓
1,2,3 correct

Check Duplicate
      ↓
3 repeated twice

RETURN TRUE
```

---

## C++ Code (Sorting Approach)

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        int n = nums.size() - 1;

        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1)
                return false;
        }

        return nums[n] == n;
    }
};
```

---

## Complexity Analysis

| Complexity | Value      |
| ---------- | ---------- |
| Time       | O(n log n) |
| Space      | O(1)       |

---

# Approach 3 — Using Set

## Intuition

A good array should:

* Have exactly one duplicate
* Duplicate should be maximum element
* Unique elements should form sequence `1 → n`

We can use set to verify uniqueness.

---

## Algorithm

1. Find maximum element `mx`
2. Check size is `mx + 1`
3. Insert all elements into set
4. Set size should be `mx`
5. Verify numbers `1 → mx` exist
6. Count maximum element occurrences

---

## Dry Run

Input:

```cpp
[1,3,3,2]
```

Set:

```text
{1,2,3}
```

Size:

```text
3 = mx
```

Maximum appears twice ✅

Answer = TRUE

---

## C++ Code (Set Approach)

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int mx = *max_element(nums.begin(), nums.end());

        if (nums.size() != mx + 1)
            return false;

        set<int> st(nums.begin(), nums.end());

        if (st.size() != mx)
            return false;

        for (int i = 1; i <= mx; i++) {
            if (!st.count(i))
                return false;
        }

        int cnt = 0;

        for (int x : nums) {
            if (x == mx)
                cnt++;
        }

        return cnt == 2;
    }
};
```

---

## Complexity Analysis

| Complexity | Value      |
| ---------- | ---------- |
| Time       | O(n log n) |
| Space      | O(n)       |

---

# Comparison of All Approaches

| Approach        | Time       | Space | Best For            |
| --------------- | ---------- | ----- | ------------------- |
| Frequency Array | O(n)       | O(n)  | Best Overall        |
| Sorting         | O(n log n) | O(1)  | Easy Implementation |
| Set Method      | O(n log n) | O(n)  | Cleaner Logic       |

---

# Final Recommendation

## Use Frequency Counting

Why?

✅ Linear Time

✅ Easy Validation

✅ Best Performance

✅ Simple Logic

---

# Key Learning

Whenever problem asks:

* exact frequency
* duplicate checking
* permutation validation

Think about:

* Frequency Array
* Hash Map
* Set
* Sorting

---

# Final Optimal Solution

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int mx = *max_element(nums.begin(), nums.end());

        if (nums.size() != mx + 1)
            return false;

        vector<int> freq(mx + 1, 0);

        for (int x : nums)
            freq[x]++;

        for (int i = 1; i < mx; i++) {
            if (freq[i] != 1)
                return false;
        }

        return freq[mx] == 2;
    }
};
```
