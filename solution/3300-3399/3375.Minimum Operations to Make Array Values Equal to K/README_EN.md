---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3300-3399/3375.Minimum%20Operations%20to%20Make%20Array%20Values%20Equal%20to%20K/README_EN.md
rating: 1382
source: Biweekly Contest 145 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3375. Minimum Operations to Make Array Values Equal to K](https://leetcode.com/problems/minimum-operations-to-make-array-values-equal-to-k)

[中文文档](/solution/3300-3399/3375.Minimum%20Operations%20to%20Make%20Array%20Values%20Equal%20to%20K/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code> and an integer <code>k</code>.</p>

<p>An integer <code>h</code> is called <strong>valid</strong> if all values in the array that are <strong>strictly greater</strong> than <code>h</code> are <em>identical</em>.</p>

<p>For example, if <code>nums = [10, 8, 10, 8]</code>, a <strong>valid</strong> integer is <code>h = 9</code> because all <code>nums[i] &gt; 9</code>&nbsp;are equal to 10, but 5 is not a <strong>valid</strong> integer.</p>

<p>You are allowed to perform the following operation on <code>nums</code>:</p>

<ul>
	<li>Select an integer <code>h</code> that is <em>valid</em> for the <strong>current</strong> values in <code>nums</code>.</li>
	<li>For each index <code>i</code> where <code>nums[i] &gt; h</code>, set <code>nums[i]</code> to <code>h</code>.</li>
</ul>

<p>Return the <strong>minimum</strong> number of operations required to make every element in <code>nums</code> <strong>equal</strong> to <code>k</code>. If it is impossible to make all elements equal to <code>k</code>, return -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [5,2,5,4,5], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>The operations can be performed in order using valid integers 4 and then 2.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [2,1,2], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">-1</span></p>

<p><strong>Explanation:</strong></p>

<p>It is impossible to make all the values equal to 2.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [9,7,5,3], k = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">4</span></p>

<p><strong>Explanation:</strong></p>

<p>The operations can be performed using valid integers in the order 7, 5, 3, and 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100 </code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

According to the problem description, we can choose the second largest value in the current array as the valid integer $h$ each time, and change all numbers greater than $h$ to $h$. This minimizes the number of operations. Additionally, since the operation reduces the numbers, if there are numbers in the current array smaller than $k$, we cannot make all numbers equal to $k$, so we directly return -1.

We iterate through the array $\textit{nums}$. For the current number $x$, if $x < k$, we directly return -1. Otherwise, we add $x$ to the hash table and update the minimum value $\textit{mi}$ in the current array. Finally, we return the size of the hash table minus 1 (if $\textit{mi} = k$, we need to subtract 1).

Time complexity is $O(n)$, and space complexity is $O(n)$, where $n$ is the length of the array $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        s = set()
        mi = inf
        for x in nums:
            if x < k:
                return -1
            mi = min(mi, x)
            s.add(x)
        return len(s) - int(k == mi)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        Set<Integer> s = new HashSet<>();
        int mi = 1 << 30;
        for (int x : nums) {
            if (x < k) {
                return -1;
            }
            mi = Math.min(mi, x);
            s.add(x);
        }
        return s.size() - (mi == k ? 1 : 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        unordered_set<int> s;
        int mi = INT_MAX;
        for (int x : nums) {
            if (x < k) {
                return -1;
            }
            mi = min(mi, x);
            s.insert(x);
        }
        return s.size() - (mi == k);
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) int {
	mi := 1 << 30
	s := map[int]bool{}
	for _, x := range nums {
		if x < k {
			return -1
		}
		s[x] = true
		mi = min(mi, x)
	}
	if mi == k {
		return len(s) - 1
	}
	return len(s)
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    const s = new Set<number>([k]);
    for (const x of nums) {
        if (x < k) return -1;
        s.add(x);
    }
    return s.size - 1;
}
```

#### JavaScript

```js
function minOperations(nums, k) {
    const s = new Set([k]);
    for (const x of nums) {
        if (x < k) return -1;
        s.add(x);
    }
    return s.size - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>, k: i32) -> i32 {
        use std::collections::HashSet;

        let mut s = HashSet::new();
        let mut mi = i32::MAX;

        for &x in &nums {
            if x < k {
                return -1;
            }
            s.insert(x);
            mi = mi.min(x);
        }

        (s.len() as i32) - if mi == k { 1 } else { 0 }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
