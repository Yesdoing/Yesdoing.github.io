---
layout: post
title: "ch02 - Counting Cells in a Blob"
categories:
  - 알고리즘
tags:
  - recursion
---

## Counting Cells in a Blob
  * 이 문제를 Recursion으로 해결하는 방법.
  * 이 문제는 입력으로 하나의 Binary 이미지가 주어진다.
  * 각 픽셀은 background pixel 이거나 image pixel
  * 서로 연결된 image pixel들의 집합을 blob라고 부른다.
  * 연결의 기준은 상하좌우 및 대각방향으로도 연결된 것으로 간주한다.

### 입력
  * N*N 크기의 2차원 그리드(grid)
  * 하나의 좌표 (x,y)

### 출력
  * 픽셀 (x,y)가 포함된 blob의 크기,
  * (x,y)가 어떤 blob에도 속하지 않는 경우에는 0

### Recursive Thinking
```
  현재 픽셀이 이 속한 blob의 크기를 카운트하려면    
    현재 픽셀이 image color가 아니라면    
      0을 반환한다.    
    현재 픽셀이 image color라면    
      먼저 현재 픽셀을 카운트한다. (count = 1).   
      현재 픽셀이 중복 카운트되는 것을 방지하기 위해 다른 색으로 칠한다.    
      현재 픽셀에 이웃한 모든 픽셀들에 대해서 (8개의 픽셀)   
        그 픽셀이 속한 blob의 크기를 카운트하여 카운터에 더해준다.   
      카운터를 반환한다.
```

### 설명
* 다음과 같은 2차원 배열이 있다고 하자.
  ```
  private static int N = 8;
  private static int[][] grid = {
      { 1, 0, 0, 0, 0, 0, 0, 1},
      { 0, 1, 1, 0, 0, 1, 0, 0},
      { 1, 1, 0, 0, 1, 0, 1, 0},
      { 0, 0, 0, 0, 0, 1, 0, 0},
      { 0, 1, 0, 1, 0, 1, 0, 0},
      { 0, 1, 0, 1, 0, 1, 0, 0},
      { 1, 0, 0, 0, 1, 0, 0, 1},
      { 0, 1, 1, 0, 0, 1, 1, 1}
  };
  ```
* 이때 0은 background color이며 1은 Image color이고, 2는 이미 Count된 자리다.
  ```
  private static int BACKGROUND_COLOR = 0;
  private static int IMAGE_COLOR = 1;
  private static int ALREADY_COUNTED = 2;
  ```
* 이때 (x,y)가 주어질때 다음과 같이 동작한다.
  ```
  public static int countCells(int x, int y) {
    if (x < 0 || x >= N || y < 0 || y >= N) {
      return 0;
    } else if (grid[x][y] != IMAGE_COLOR) {
      return 0;
    } else {
      grid[x][y] = ALREADY_COUNTED;
      return 1 + countCells(x-1, y+1) + countCells(x, y+1) + countCells(x+1, y+1) + countCells(x-1, y) + countCells(x+1, y) +
          countCells(x-1, y-1) + countCells(x, y-1) + countCells(x+1, y-1);
    }

  }
  ```
