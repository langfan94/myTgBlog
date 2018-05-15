---
title: 冒泡排序和快速排序
date: 2018-05-15 15:23:40
tags:
---

# 冒泡排序
> 依次比较相邻的两个元素，如果后一个小于前一个，则交换，这样从头到尾一次，就将最大的放到了末尾。

![冒泡排序图解](https://user-gold-cdn.xitu.io/2017/5/24/2b79ee330b7276e15152c96e93ffb782?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

##　冒泡排序Demo
```js
// 冒泡排序法
console.time('冒泡排序法共花费了')
function bubbleSort(oArr, endIndex) {
    endIndex = typeof endIndex !== 'number' ? oArr.length - 1 : endIndex
    for(let i = 0; i <= endIndex; i++) {
        if(i + 1 <= endIndex) {
            if(oArr[i] > oArr[i + 1]) {
                swap(oArr, i, i+1);
            }
        }
    }
    --endIndex;
    if(endIndex > 0) {
        bubbleSort(oArr, endIndex)
    }else {
        // console.log('冒泡排序法', oArr);
    }
}

let oArr2 = arrPro();
bubbleSort(oArr2);
console.timeEnd('冒泡排序法共花费了')       // 冒泡排序法共花费了: 0.369ms
```

# 快速排序
>快速排序是由东尼·霍尔所发展的一种排序算法。在平均状况下，排序 n 个项目要 Ο(nlogn) 次比较。在最坏状况下则需要 Ο(n2) 次比较，但这种状况并不常见。事实上，快速排序通常明显比其他 Ο(nlogn) 算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来。快速排序使用分治法（Divide and conquer）策略来把一个串行（list）分为两个子串行（sub-lists）。

![快速排序图解](https://user-gold-cdn.xitu.io/2017/5/24/e091a0dc1b5122f936904027108fa317?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


## 快速排序Demo1
```js
    // 快速排序法
console.time('快速排序法共花费了')
function quickSort(oArray, leftIndex, rightIndex) {
    let len = oArray.length;
    let partitionIndex;
    leftIndex = typeof leftIndex !== 'number' ? 0 : leftIndex;
    rightIndex = typeof rightIndex !== 'number' ? len - 1 : rightIndex;
    if(leftIndex < rightIndex) {
        partitionIndex = partition(oArray, leftIndex, rightIndex);
        // console.log('partitionIndex', partitionIndex);
        quickSort(oArray, leftIndex, partitionIndex - 1);
        quickSort(oArray, partitionIndex + 1, rightIndex);
    }else {
        // console.log('oArray', oArray);
        // return oArray;
    }
}

function partition(oArray, leftIndex, rightIndex) {
    let swapIndex = leftIndex + 1;
    for (let i=swapIndex; i<=rightIndex ; i++) {
        if(oArray[i] < oArray[leftIndex]) {
            swap(oArray, i, swapIndex);
            ++swapIndex;
        }
    }
    swap(oArray, leftIndex, swapIndex-1);
    return swapIndex-1;
}

let oArr = arrPro();
quickSort(oArr);
// console.log('快速排序法', oArr);
console.timeEnd('快速排序法共花费了')   // 快速排序法共花费了: 0.221ms
```

## 快速排序Demo2
```js
    console.time('快速排序2法共花费了')
function partition2(arr, low, high) {
    let pivot = arr[low];
    while (low < high) {
      while (low < high && arr[high] > pivot) {
        --high;
      }
      arr[low] = arr[high];
      while (low < high && arr[low] <= pivot) {
        ++low;
      }
      arr[high] = arr[low];
    }
    arr[low] = pivot;
    return low;
  }
  
  function quickSort2(arr, low, high) {
    if (low < high) {
      let pivot = partition2(arr, low, high);
      quickSort2(arr, low, pivot - 1);
      quickSort2(arr, pivot + 1, high);
    }
    return arr;
  }


let oArr3 = arrPro();
quickSort2(oArr3, 0, oArr3.length - 1)
// console.log('oArr3', oArr3);
console.timeEnd('快速排序2法共花费了')          // 快速排序2法共花费了: 0.110ms
```