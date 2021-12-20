---
title: Merge-Sort
date: 2021-12-20 16:09:15
tags: algorithm
---

# Timsort
> Timsort 是一种混合稳定的排序算法，源自合并排序和插入排序，旨在较好地处理真实世界中各种各样的数据。它使用了 Peter Mcllroy 的"乐观排序和信息理论上复杂性"中的技术，参见 第四届年度ACM-SIAM离散算法研讨会论文集，第467-474页，1993年。 它由 Tim Peters 在2002年实现，并应用于 Python编程语言。该算法通过查找已经排好序的数据子序列，在此基础上对剩余部分更有效地排序。 该算法通过不断地将特定子序列（称为一个 run ）与现有的 run 合并，直到满足某些条件为止来达成的更有效的排序。 从 2.3 版本起，Timsort 一直是 Python 的标准排序算法。 它还被 Java SE7[4], Android platform[5], GNU Octave,[6] 谷歌浏览器,[7] 和 Swift[8] 用于对非原始类型的数组排序。

## 合并排序实现
```js
  /* Merge-sort */
  function merge(left, right) {
    const arr = [];
    while (left.length > 0 && right.length > 0) {
      if (left[0] < right[0]) {
        arr.push(left.shift());
      } else {
        arr.push(right.shift());
      }
    }

    return [...arr, ...left, ...right];
  }

  function divSort(array) {
    const half = array.length / 2;
    if (array.length < 2) {
      return array;
    }

    const left = array.splice(0, half);
    return merge(divSort(left), divSort(array));
  }

  const testArray = [4, 8, 7, 2, 11, 1, 3];
  console.log(divSort(testArray));
```

## TimSort 和 quicksort
TimSort是高度优化的mergesort，它比旧的mergesort更稳定，更快。

与quicksort相比，它有两个优点：

1. 对于几乎排序的数据序列（包括反向排序数据）来说，速度快得令人难以置信;
2. 最坏的情况仍然是O（N * LOG（N））。
老实说，我不认为＃1是一个优势，但它给我留下了深刻的印象。

这是QuickSort的优势

1. QuickSort非常简单，即使是高度优化的实现，我们也可以在20行内写下它的pseduo代码;
2. QuickSort在大多数情况下是最快的;
3. 内存消耗为LOG（N）。