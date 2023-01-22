---
title: Merge Sort
author: yanshi
date: 2023-01-08 23:17 +0800
categories: [Code, Algorithm]
tags: [sorting]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/

---

## Preparation

Same with the preparation of [quick sort](https://xuyanshi.github.io/posts/quick-sort/#preparation).


## Merge Sort

[Merge Sort - GeeksforGeeks](https://www.geeksforgeeks.org/merge-sort/)

The **Merge Sort** algorithm is a sorting algorithm that is based on the **Divide and Conquer** paradigm. In this algorithm, the array is initially divided into two equal halves and then they are combined in a sorted manner.

Merge Sort Working Process:

Think of it as a recursive algorithm continuously splits the array in half until it cannot be further divided. This means that if the array becomes empty or has only one element left, the dividing will stop, i.e. it is the base case to stop the recursion. If the array has multiple elements, split the array into halves and recursively invoke the merge sort on each of the halves. Finally, when both halves are sorted, the merge operation is applied. Merge operation is the process of taking two smaller sorted arrays and combining them to eventually make a larger one.

## Merge Sort Template

There is a universal template of merge sorting below and we'd better memorize it completely.

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



## Others about Merge Sort

- **Time Complexity:** O(N log(N))
- NOT In-place algorithm: In merge sort the merging step requires extra space to store the elements.
- Stable (插、冒、归、基)

