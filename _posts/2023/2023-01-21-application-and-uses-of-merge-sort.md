---
title: Application and uses of Merge Sort
author: yanshi
date: 2023-01-21
categories: [Code, Algorithm]
tags: [sorting]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/

---

Today is Jan 21, 2023.

Happy Lunar New Year! 🐰

[Merge Sort](https://xuyanshi.github.io/posts/merge-sort/)

## Merge Sort Template

```c++
/* Preparation */

const int N = 1e6 + 10;
int tmp[N]; // Extra space to store the elements.

void merge_sort(int a[], int l, int r) {
    if (l >= r) { return; }
    int mid = (l + r) / 2; // Or: l + (r - l) / 2
    merge_sort(a, l, mid);
    merge_sort(a, mid + 1, r);

    int i = l, j = mid + 1, k = 0;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) { tmp[k++] = a[i++]; }
        else { tmp[k++] = a[j++]; }
    }
    while (i <= mid) { tmp[k++] = a[i++]; }
    while (j <= r) { tmp[k++] = a[j++]; }
    for (i = l, k = 0; i <= r; i++, k++) { a[i] = tmp[k]; }
}

// Demo
int main() {
    int nums[] = {3, 1, 4, 5, 2, 6, 3};
    int n = length(nums);
    print(nums); // 3 1 4 5 2 6 3
    merge_sort(nums, 0, n - 1);
    print(nums); // 1 2 3 3 4 5 6
    return 0;
}
```

## LeetCode Offer 51. Reverse Pairs in Array

[LeetCode Offer 51. Reverse Pairs in Array](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)



Given an integer array `nums`, return *the number of **reverse pairs** in the array*.

A **reverse pair** is a pair `(i, j)` where:

- `0 <= i < j < nums.length` and

- `nums[i] > nums[j]`. 

**(Different from the next question LeetCode 493)**

 

**Example:**

```
Input: nums = [7,5,6,4]
Output: 5
```

 

**Constraints:**

- `0 <= nums.length <= 50000`



**Solution:**

```c++
class Solution {
public:
    static const int N = 5e4 + 10;
    int tmp[N]; // Extra space to store the elements.
    
    int merge_sort(vector<int>& a, int left, int right) {
        if (left >= right) { return 0; } // Different
        int mid = (left + right) / 2; 
        int res = merge_sort(a, left, mid) + merge_sort(a, mid + 1, right); // Different

        int i = left, j = mid + 1, k = 0;
        while (i <= mid && j <= right) {
            if (a[i] <= a[j]) { tmp[k++] = a[i++]; }
            else {
                tmp[k++] = a[j++];
                res += mid - i + 1; // Different
            }
        }
        while (i <= mid) { tmp[k++] = a[i++]; }
        while (j <= right) { tmp[k++] = a[j++]; }
        for (i = left, k = 0; i <= right; i++, k++) { a[i] = tmp[k]; }
        return res; // Different
    }
    
    int reversePairs(vector<int>& nums) {
        return merge_sort(nums, 0, nums.size()-1);
    }
};
```



## LeetCode 493. Reverse Pairs

[LeetCode 493. Reverse Pairs](https://leetcode.cn/problems/reverse-pairs/)



Given an integer array `nums`, return *the number of **reverse pairs** in the array*.

A **reverse pair** is a pair `(i, j)` where:

- `0 <= i < j < nums.length` and

- `nums[i] > 2 * nums[j]`. 

**(Different from the former question LeetCode Offer 51)**

 

**Example 1:**

```
Input: nums = [1,3,2,3,1]
Output: 2
Explanation: The reverse pairs are:
(1, 4) --> nums[1] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 3, nums[4] = 1, 3 > 2 * 1
```

**Example 2:**

```
Input: nums = [2,4,3,5,1]
Output: 3
Explanation: The reverse pairs are:
(1, 4) --> nums[1] = 4, nums[4] = 1, 4 > 2 * 1
(2, 4) --> nums[2] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 5, nums[4] = 1, 5 > 2 * 1
```

 

**Constraints:**

- `1 <= nums.length <= 5 * 10^4`
- `-2^31 <= nums[i] <= 2^31 - 1`



**Solution:**

```c++
class Solution {
public:
    static const int N = 5e4 + 10;
    int tmp[N]; // Extra space to store the elements.
    
    int merge_sort(vector<int>& a, int left, int right) {
        if (left >= right) { return 0; }
        int mid = (left + right) / 2; // Or: left + (right - left) / 2
        int res = merge_sort(a, left, mid) + merge_sort(a, mid + 1, right);

        int i = left, j = mid + 1, k = 0;
        while (i <= mid && j <= right) {
            if ((long long)a[i] <= 2 * (long long)a[j]) { tmp[k++] = a[i++]; }
            else {
                tmp[k++] = a[j++];
                res += mid - i + 1;
            }
        }
        
        // Then merge these two sorted arrays.
        vector<int> sorted(right - left + 1);
        int p1 = left, p2 = mid + 1;
        int p = 0;
        while (p1 <= mid || p2 <= right) {
            if (p1 > mid) {
                sorted[p++] = a[p2++];
            } else if (p2 > right) {
                sorted[p++] = a[p1++];
            } else {
                if (a[p1] < a[p2]) {
                    sorted[p++] = a[p1++];
                } else {
                    sorted[p++] = a[p2++];
                }
            }
        }
        for (int i = 0; i < sorted.size(); i++) {
            a[left + i] = sorted[i];
        }
        return res;
    }
    
    int reversePairs(vector<int>& nums) {
        return merge_sort(nums, 0, nums.size()-1);
    }
};
```

