# 1674. Minimum Moves to Make Array Complementary

---

# Problem Statement

You are given:

* An integer array `nums` of even length `n`
* An integer `limit`

In one move, you can replace any integer in the array with another integer between `1` and `limit`.

The array is called complementary if:

```cpp
nums[i] + nums[n-1-i]
```

is the same for every index.

Return the minimum number of moves required.

---

# Example

## Input

```cpp
nums = [1,2,4,3]
limit = 4
```

## Output

```cpp
1
```

---

# Understanding the Problem

We process symmetric pairs:

```cpp
(nums[i], nums[n-1-i])
```

For all pairs, we want:

```cpp
same target sum
```

Possible target sums:

```cpp
2 to 2*limit
```

---

# Pair Analysis

For pair `(a,b)`:

---

## 0 Move

If:

```cpp
a+b == target
```

No move needed.

---

## 1 Move

Possible sums achievable by changing one element:

```cpp
[min(a,b)+1 , max(a,b)+limit]
```

---

## 2 Moves

Remaining sums require 2 moves.

---

# What Information Do We Need?

| Step                | Information Needed             |
| ------------------- | ------------------------------ |
| Create pair         | `nums[i]`, `nums[n-1-i]`       |
| Find exact sum      | `a+b`                          |
| Find one move range | `min(a,b)+1`, `max(a,b)+limit` |
| Count operations    | moves needed                   |
| Final answer        | minimum moves                  |

---

# 1. Brute Force Approach

# Idea

Try every possible target sum.

For every target:

* Check every pair
* Calculate moves needed
* Take minimum answer

---

# Brute Force Algorithm

For target from:

```cpp
2 → 2*limit
```

For every pair:

* If exact sum → 0 moves
* Else if possible with one change → 1 move
* Else → 2 moves

Return minimum total.

---

# Brute Force Dry Run

## Input

```cpp
nums = [1,2,4,3]
limit = 4
```

Pairs:

```cpp
(1,3)
(2,4)
```

Try target = 4

### Pair (1,3)

```cpp
1+3 = 4
```

0 move.

---

### Pair (2,4)

Need target 4.

Change 4 → 2.

1 move.

Total:

```cpp
1 move
```

---

# Brute Force Complexity

## Time Complexity

```cpp
O(n * limit)
```

---

## Space Complexity

```cpp
O(1)
```

---

# Brute Force Code

```cpp
class Solution {
public:

    int movesNeeded(int a, int b, int target, int limit) {

        // 0 move
        if (a + b == target)
            return 0;

        // 1 move
        if ((target - a >= 1 && target - a <= limit) ||
            (target - b >= 1 && target - b <= limit))
            return 1;

        // 2 moves
        return 2;
    }

    int minMoves(vector<int>& nums, int limit) {

        int n = nums.size();

        int ans = INT_MAX;

        for (int target = 2; target <= 2 * limit; target++) {

            int totalMoves = 0;

            for (int i = 0; i < n / 2; i++) {

                int a = nums[i];
                int b = nums[n - 1 - i];

                totalMoves += movesNeeded(a, b, target, limit);
            }

            ans = min(ans, totalMoves);
        }

        return ans;
    }
};
```

---

# 2. Better Approach

# Idea

Store results for all target sums.

Still checks all targets but organizes calculations better.

---

# Better Algorithm

For every target sum:

* Process all pairs
* Store moves in array
* Return minimum

---

# Better Complexity

## Time Complexity

```cpp
O(n * limit)
```

---

## Space Complexity

```cpp
O(limit)
```

---

# Better Approach Code

```cpp
class Solution {
public:

    int minMoves(vector<int>& nums, int limit) {

        int n = nums.size();

        vector<int> moves(2 * limit + 1, 0);

        for (int target = 2; target <= 2 * limit; target++) {

            int total = 0;

            for (int i = 0; i < n / 2; i++) {

                int a = nums[i];
                int b = nums[n - 1 - i];

                if (a + b == target)
                    total += 0;

                else if ((target - a >= 1 && target - a <= limit) ||
                         (target - b >= 1 && target - b <= limit))
                    total += 1;

                else
                    total += 2;
            }

            moves[target] = total;
        }

        int ans = INT_MAX;

        for (int target = 2; target <= 2 * limit; target++) {
            ans = min(ans, moves[target]);
        }

        return ans;
    }
};
```

---

# 3. Optimal Approach

# Main Optimization

We use:

```cpp
Difference Array + Prefix Sum
```

---

# Key Observation

For each pair `(a,b)`:

---

## 2 Moves

Initially assume:

```cpp
All sums require 2 moves
```

---

## 1 Move Range

```cpp
[min(a,b)+1 , max(a,b)+limit]
```

needs only 1 move.

---

## 0 Move

```cpp
a+b
```

needs 0 moves.

---

# Difference Array Concept

Difference array helps perform range updates in:

```cpp
O(1)
```

instead of:

```cpp
O(range)
```

---

# Range Updates

For pair `(a,b)`:

```cpp
low  = min(a,b)+1
high = max(a,b)+limit
sum  = a+b
```

Updates:

| Range       | Update |
| ----------- | ------ |
| All sums    | +2     |
| [low, high] | -1     |
| sum         | -1     |

---

# Optimal Algorithm

## Step 1

Create difference array.

---

## Step 2

Process every pair.

Apply range updates.

---

## Step 3

Take prefix sum.

---

## Step 4

Minimum prefix value = answer.

---

# Architecture / Flow

```text
Input Array
     |
     v
Create Symmetric Pairs
     |
     v
Find:
- low
- high
- sum
     |
     v
Apply Difference Updates
     |
     v
Build Prefix Sum
     |
     v
Find Minimum Moves
     |
     v
Return Answer
```

---

# Optimal Dry Run

## Input

```cpp
nums = [1,2,4,3]
limit = 4
```

Pairs:

```cpp
(1,3)
(2,4)
```

---

## Pair (1,3)

```cpp
sum = 4
low = 2
high = 7
```

---

## Pair (2,4)

```cpp
sum = 6
low = 3
high = 8
```

After prefix calculation:

```cpp
Minimum moves = 1
```

---

# Optimal Complexity

## Time Complexity

```cpp
O(n + limit)
```

---

## Space Complexity

```cpp
O(limit)
```

---

# Optimal Code

```cpp
class Solution {
public:

    int minMoves(vector<int>& nums, int limit) {

        int n = nums.size();

        vector<int> diff(2 * limit + 2, 0);

        for (int i = 0; i < n / 2; i++) {

            int a = nums[i];
            int b = nums[n - 1 - i];

            int low = min(a, b) + 1;
            int high = max(a, b) + limit;
            int sum = a + b;

            // Assume 2 moves for all sums
            diff[2] += 2;

            // 1 move range
            diff[low] -= 1;
            diff[high + 1] += 1;

            // 0 move exact sum
            diff[sum] -= 1;
            diff[sum + 1] += 1;
        }

        int ans = INT_MAX;
        int curr = 0;

        for (int target = 2; target <= 2 * limit; target++) {

            curr += diff[target];

            ans = min(ans, curr);
        }

        return ans;
    }
};
```

---

# Comparison Table

| Approach    | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Brute Force | O(n * limit)    | O(1)             |
| Better      | O(n * limit)    | O(limit)         |
| Optimal     | O(n + limit)    | O(limit)         |

---

# Why Optimal Approach is Fast?

Instead of checking every target for every pair:

```cpp
O(n * limit)
```

Difference array performs smart range updates.

Prefix sum reconstructs final answers efficiently.

---

# Key Learning

This problem teaches:

* Pair Processing
* Prefix Sum
* Difference Array
* Range Updates
* Optimization Techniques
* Transition from Brute Force → Optimal

---

# Interview Explanation

"We process symmetric pairs independently.

For each pair:

* Exact current sum requires 0 moves
* A range of sums requires 1 move
* Remaining sums require 2 moves

Using Difference Array + Prefix Sum, we efficiently process all target sums.

Finally, we return the minimum moves among all target sums."
