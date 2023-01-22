---
title: Application and uses of Quick Sort
author: yanshi
date: 2023-01-19 01:10 +0800
categories: [Code, Algorithm]
tags: [sorting]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/

---

[Quick Sort](https://xuyanshi.github.io/posts/quick-sort/)

## Quick Sort Template

```c++
/* Preparation */

void quick_sort(int a[], int l, int r) {
    if (l >= r) { return; }
    int pivot = a[l], i = l - 1, j = r + 1; // Two Pointers(双指针)
    while (i < j) {
        i++; j--;
        while (a[i] < pivot) { i++; }
        while (a[j] > pivot) { j--; }
        if(i<j) { swap(a[i],a[j]);}
    }
    quick_sort(a,l,j);
    quick_sort(a,j+1,r);
}

// demo
int main() {
    int nums[] = {3, 1, 4, 5, 2, 6, 3};
    print(nums); // 3 1 4 5 2 6 3
    quick_sort(nums, 0, length(nums) - 1);
    print(nums); // 1 2 3 3 4 5 6
    return 0;
}
```



## LeetCode 215. Kth Largest Element in an Array

[LeetCode 215. Kth Largest Element in an Array](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

It's a frequent coding question in interviews.

**Question:**

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

 

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

 

**Constraints:**

- `1 <= k <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`



**Solutions:**

1. Brute Force Algorithm: Sort completely and then choose the Kth Largest Element.

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        final int N = nums.length;
        Arrays.sort(nums); // O(nlogn) time complexity
        return nums[N-k];
    }
}
```


2. Heap/Priority Queue.

```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    for (int num : nums) {
        pq.offer(num);
        if (pq.size() > k) {
            pq.poll();
        }
    }
    return pq.peek();
}
```

**3. Quick Selection Algorithm **

Based on the partition (divide and conquer) method, the same one as used in quicksort.

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, k);
    }

    public int quickSelect(int[] nums, int left, int right, int k) {
        if (left >= right) {
            return nums[left];
        }
        int i = left - 1, j = right + 1, pivot = nums[(left + right) / 2];
        while (i < j) {
            i++;
            j--;
            while (pivot < nums[i]) {
                i++;
            }
            while (pivot > nums[j]) {
                j--;
            }
            if (i < j) { // swap
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
            }
        }
        int cntLeft = j - left + 1;
        if (cntLeft >= k) { // search Kth element in left section because cntLeft>=k
            return quickSelect(nums, left, j, k);
        }
        // When cntLeft < k
        // The kth element must in the right section
        // All elements in the left section are less than the kth element
        // So we just search (k-cntLeft)th element in the right section recursively
        return quickSelect(nums, j + 1, right, k - cntLeft);
    }
}
```




## LeetCode Interview 17.14. Smallest K LCCI

[LeetCode Interview 17.14. Smallest K LCCI](https://leetcode.cn/problems/smallest-k-lcci/)

Design an algorithm to find the smallest K numbers in an array. Output in an **arbitrary** order.

**Example:** 

```
Input:  arr = [1,3,5,7,2,4,6,8], k = 4
Output:  [1,2,3,4]
```

**Note:** 

- `0 <= len(arr) <= 100000`
- `0 <= k <= min(100000, len(arr))`

**Solution with Quick Selection Algorithm:**

```java
class Solution {
    public int[] smallestK(int[] arr, int k) {
        randomizedSelected(arr, 0, arr.length - 1, k);
        int[] vec = new int[k];
        for (int i = 0; i < k; ++i) {
            vec[i] = arr[i];
        }
        return vec;
    }

    private void randomizedSelected(int[] arr, int l, int r, int k) {
        if (l >= r) {
            return;
        }
        int pos = randomizedPartition(arr, l, r);
        int num = pos - l + 1;
        if (k == num) {
            return;
        } else if (k < num) {
            randomizedSelected(arr, l, pos - 1, k);
        } else {
            randomizedSelected(arr, pos + 1, r, k - num);
        }
    }

    private int randomizedPartition(int[] nums, int l, int r) {
        int i = new Random().nextInt(r - l + 1) + l;
        swap(nums, r, i);
        return partition(nums, l, r);
    }

    private int partition(int[] nums, int l, int r) {
        int pivot = nums[r];
        int i = l - 1;
        for (int j = l; j <= r - 1; ++j) {
            if (nums[j] <= pivot) {
                i = i + 1;
                swap(nums, i, j);
            }
        }
        swap(nums, i + 1, r);
        return i + 1;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

/*
https://leetcode.cn/problems/smallest-k-lcci/solutions/590916/zui-xiao-kge-shu-by-leetcode-solution-o5eg/
*/
```



## LeetCode 1738. Find Kth Largest XOR Coordinate Value

[LeetCode 1738. Find Kth Largest XOR Coordinate Value](https://leetcode.cn/problems/find-kth-largest-xor-coordinate-value)You are given a 2D `matrix` of size `m x n`, consisting of non-negative integers. You are also given an integer `k`.

The **value** of coordinate `(a, b)` of the matrix is the XOR of all `matrix[i][j]`where `0 <= i <= a < m` and `0 <= j <= b < n` **(0-indexed)**.

Find the `kth` largest value **(1-indexed)** of all the coordinates of `matrix`.

 

**Example 1:**

```
Input: matrix = [[5,2],[1,6]], k = 1
Output: 7
Explanation: The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.
```

**Example 2:**

```
Input: matrix = [[5,2],[1,6]], k = 2
Output: 5
Explanation: The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.
```

**Example 3:**

```
Input: matrix = [[5,2],[1,6]], k = 3
Output: 4
Explanation: The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 10^6`
- `1 <= k <= m * n`



**Solutions:**

```java
// TODO
```

