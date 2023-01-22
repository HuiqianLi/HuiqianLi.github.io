---
title: Quick Sort
author: yanshi
date: 2023-01-05 02:12 +0800
categories: [Code, Algorithm]
tags: [sorting]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/

---


Quicksort is an efficient, general-purpose sorting algorithm. Of course, we could sort arrays with Standard Library or STL simply and quickly. However, sorting is a FAQ in interviews for a job. I will try to sort arrays in different ways.

## Preparation

Print an array or get the length of it. 

```c++
#include <iostream>
#include <algorithm>

using namespace std;

//Just for arrays but not work for vectors.
template<class T>
int length(const T &a) { 
    return sizeof(a) / sizeof(a[0]);
}

template<class T>
void print(const T &a) {
    for (int i = 0; i < length(a); i++) {
        cout << a[i] << " ";
    }
    cout << endl;
}
```

## Standard Libraries/STL

We could include the algorithm library and sort an array with it.

```c++
int main() {
    int nums[] = {3, 1, 4, 5, 2, 6, 3}; 
    print(nums); // 3 1 4 5 2 6 3
    sort(nums, nums + length(nums));
    print(nums); // 1 2 3 3 4 5 6
    return 0;
}
```

Vectors also can be sorted by the sort() method.

```c++
int nums[] = {3, 1, 4, 5, 2, 6, 3};
vector<int> vec(nums, nums + length(nums)); 
sort(vec.begin(), vec.end()); // Diffrenet Syntax
```

## Quick Sort

[QuickSort - GeeksforGeeks](https://www.geeksforgeeks.org/quick-sort/)

Like [Merge Sort](https://www.geeksforgeeks.org/merge-sort/), **QuickSort** is a[ Divide and Conquer algorithm](https://www.geeksforgeeks.org/divide-and-conquer-algorithm-introduction/). It picks an element as a pivot and partitions the given array around the picked pivot. There are many different versions of quickSort that pick pivot in different ways. 

- Always pick the first element as a pivot.
- Always pick the last element as a pivot (implemented below)
- Pick a random element as a pivot.
- Pick median as the pivot.

The key process in quickSort is a partition(). The target of partitions is, given an array and an element x of an array as the pivot, put x at its correct position in a sorted array and put all smaller elements (smaller than x) before x, and put all greater elements (greater than x) after x. All this should be done in linear time.

![QuickSort](QuickSort.png)

## Quick Sort Template

There is a universal template of quick sorting below and we'd better memorize it completely.

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



## Others about Quick Sort

- Time complexity of QuickSort is O(**nLogn**)
- In-place algorithm
- NOT stable
