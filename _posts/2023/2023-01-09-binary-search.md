---
title: Binary Search
author: yanshi
date: 2023-01-09 18:22 +0800
categories: [Code, Algorithm]
tags: [binary search]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/

---

## Binary Search

[Binary Search - GeeksforGeeks](https://www.geeksforgeeks.org/binary-search/)

**Binary Search** is a [searching algorithm](https://www.geeksforgeeks.org/searching-algorithms/) used in a **sorted** array by repeatedly dividing the search interval in half. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to O(Log n).



## Binary Search Template

Two universal templates of binary search below.

```c++
// [left, right] -> [left, mid] , [mid + 1, right]
int binary_search1(int left, int right) {
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (check(mid)) { right = mid; } // check() is depending on the question.
        else { left = mid + 1; }
    }
    return left;
}

// [left, right] -> [left, mid - 1] , [mid, right]
int binary_search2(int left, int right) {
    while (left < right) {
        int mid = left + (right - left + 1) / 2;
        if (check(mid)) { left = mid; }
        else { right = mid - 1; }
    }
    return left;
}
```



## LeetCode 704. Binary Search

[LeetCode 704. Binary Search](https://leetcode.cn/problems/binary-search/)

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.



Use the template 1:

```c++
class Solution {
public:
    int search(vector<int> &nums, int target) {
        int left = 0, right = nums.size(); // Initialization: [0, n)
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] > target) right = mid;
            else left = mid + 1;
        }
        return -1;
    }
};
```


```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        return -1
```



