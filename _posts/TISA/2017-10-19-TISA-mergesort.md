---
layout: post
title: "ch03 - 합병정렬(merge sort)"
categories:
  - 알고리즘
tags:
  - mergeSort
  - 분할정복법
---

## 분할정복법(Divide and Conquer)
  * 분할: 해결하고자 하는 문제를 작은 크기의 동일한 문제들로 분할
    * 더 작은 크기의 동일한 문제(중요!) - recursion의 예시로 몇개 나옴
  * 정복: 각각의 작은 문제를 순환적으로 해결
  * 합병: 작은 문제의 해를 합하여(merge) 월래 문제에 대한 해를 구함
  * 위 3가지 단계를 거쳐서 문제를 해결하는 방법이 분할정복법

### 합병정렬(merge sort)
  * 데이터가 저장된 배열을 절반으로 나눔
  * 각각을 순환적으로 정렬
  * 정렬된 두 개의 배열을 합쳐 전체를 정렬
  * 실제로 분할할때 쪼개는건 아님.
  * 코딩을 할때 분할되서 정렬된 2가지를 합치는 것을 코딩해야함.
  * merge를 하는게 가장 중요한 단계이다.
  * 스도코드
  ```
    mergeSort(A[], p, r) {    // A[p ... r]을 정렬한다.
      if (p<r) then {
        q <- (p+r)/2;         // 1. p, r의 중간 지점 계산
        mergeSort(A, p, q);   // 2. 전반부 정렬
        mergeSort(A, q+1, r); // 3. 후반부 정렬
        merge(A, p, q, r);    // 4. 합병
      }
    }

    merge(A[], p, q, r) {
      정렬되어 있는 두 배열 A[p ... q]와 A[q+1 ... r]을 합하여
      정렬된 하나의 배열 A[p ... r]을 만든다.
    }
  ```
  * 코드
  ```
    void merge(int data[], int p, int q, int r) {
      int i=p, j=q+1, k=p;
      int tmp[data.length()];
      while ( i<=q && j<= r) {
        if(data[i] <= data[j])
          tmp[k++] = data[i++];
        else
          tmp[k++] = data[j++];
      }
      while( i<= q) {
        tmp[k++] = data[i++];
      }
      while( j <= r) {
        tmp[k++] = data[j++];
      }
      for(int i=p; i<=r; i++)
        data[i]=tmp[i];
    }
  ```
  * 시간복잡도
    * for문처럼 시간복잡도를 계산할수없다. (가시적으로 들어나지 않음.)
    * T(n)
      * 0 if n=1
      * T([n/2]) + T([n/2]) + n otherwise
      * O(nlogn)
