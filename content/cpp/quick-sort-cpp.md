---
title: "快速排序-C++代码"
date: 2019-08-24T11:34:24+08:00
type: posts
categories: [C++,算法]
keywords: [C++,cpp,快速排序]
tags: [C++,快速排序]
summary: "快速排序C++实现。"
---

```cpp
#include <iostream>

int partition(int a[], int l, int r)
{
    int i = l;
    int j = r + 1;
    int v = a[l];
    
    while (true) {
        while (a[--j] > v);
        while (i < j && a[++i] < v);
        
        if (i == j)
        {
            break;
        }
        std::swap(a[i], a[j]);
    }
    
    std::swap(a[i], a[l]);
    
    return i;
}

void quick_sort(int a[], int l, int r)
{
    if (l < r) {
        int m = partition(a, l, r);
        quick_sort(a, l, m - 1);
        quick_sort(a, m + 1, r);
    }
}

int main(int argc, const char * argv[])
{
    int a[] = {1, 1, 6, 6, 8, 3, 5, 100, 300, 200, 99, 99};
    quick_sort(a, 0, sizeof(a)/sizeof(a[0]) - 1);
    
    for (int i : a) {
        std::cout << i << ' ';
    }
    std::cout << std::endl;
    
    return 0;
}
```
