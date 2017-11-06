---
layout: post
title: "ch03 - 빠른정렬(quicksort)"
categories:
  - 알고리즘
tags:
  - quicksort
  - 분할정복법
---

### 분할정복법
  * 분할 : 배열을 다음과 같은 조건이 만족되도록 두 부분으로 나눈다.
    * elements in lower parts <= elements in upper parts
  * 정복 : 각 부분을 순환적으로 정렬한다.
  * 합병 : nothing to do

### Quick sort
  * 정렬할 배열이 주어짐. 마지막 수를 기준(pivot)으로 삼는다.
  * 기준보다 작은 수는 기준의 왼쪽에 나머지는 기준의 오른쪽에 오도록 재배치 분할 한다.
  * 기준의 왼쪽과 오른쪽을 각각 순환적으로 정렬한다.

#### 스도코드
```
  quickSort(A[], p, r)            // A[p, ..., r]을 정렬한다.
  {
    if (p<r) then {
      q = partition(A, p, r);     // 분할 q = pivot의 위치 
      quickSort(A, p, q-1);       // 왼쪽 부분배열 정렬
      quickSort(A, q+1, r);       // 오른쪽 부분배열 정렬
    }
  }

  partition(A[], p, r) {
    배열 A[p...r]의 원소들을 A[r]을 기준으로 양쪽으로 재배치하고
    A[r]이 자리한 위치를 return 한다.
  }
```
