---
title: "堆排序-C++代码"
date: 2019-08-24T11:33:07+08:00
type: posts
categories: [C++,算法]
keywords: [C++,cpp,堆排序]
tags: [C++,堆排序]
summary: "堆排序C++实现。"
---

```
#include <iostream>

// 堆调整（非递归版）
void max_heapify_no_recur(int a[], int len, int i)
{
    int l = 0;
    int r = 0;
    int m = 0;

    while (true) {
        l = i * 2 + 1;  //左节点
        r = i * 2 + 2;  //右节点
        m = i;

        // 找出最大值节点
        if (l < len && a[l] > a[m]) {
            m = l;
        }
        if (r < len && a[r] > a[m]) {
            m = r;
        }
        if (m != i) {
            // 把当前节点设为最大，继续调整子节点
            std::swap(a[i], a[m]);
            i = m;
        }
        else {
            break;
        }
    }
}

// 堆调整（递归版）
void max_heapify(int a[], int len, int i)
{
    int l = i * 2 + 1;  //左节点
    int r = i * 2 + 2;  //右节点
    int m = i;

    // 找出最大值节点
    if (l < len && a[l] > a[m]) {
        m = l;
    }
    if (r < len && a[r] > a[m]) {
        m = r;
    }
    if (m != i) {
        // 把当前节点设为最大，递归调整子节点
        std::swap(a[i], a[m]);
        max_heapify(a, len, m);
    }
}

// 建堆
void build_max_heap(int a[], int len)
{
    for (int i = len/2 - 1; i >= 0; i--) {
//        max_heapify(a, len, i);
        max_heapify_no_recur(a, len, i);
    }
}

// 堆排序
void heap_sort(int a[], int len)
{
    build_max_heap(a, len);
    for (int i = len - 1; i > 0; i--) {
        std::swap(a[i], a[0]);
//        max_heapify(a, i, 0);
        max_heapify_no_recur(a, i, 0);
    }
}

int main(int argc, const char * argv[])
{
    int a[] = {1, 1, 6, 6, 8, 3, 5, 100, 300, 200, 99, 99};
    heap_sort(a, sizeof(a)/sizeof(a[0]));

    for (int i : a) {
        std::cout << i << ' ';
    }
    std::cout << std::endl;
    
    return 0;
}

```
