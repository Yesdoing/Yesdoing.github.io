---
layout: post
title: "ch01 - 알고리즘의 분석"
categories:
  - 알고리즘
tags:
  - TimeComplexity
---

## 인프런 -  영리한 프로그래머를 위한 알고리즘 강좌
### Ch01. 알고리즘 분석
#### 시간복잡도

* 알고리즘을 분석
  * 알고리즘의 자원(resource) 사용량을 분석
  * 자원이란 실행시간, 메모리, 저장장치, 통신 등
  * 여기서는 실행시간의 분석에 대해서 다룸.

* 시간복잡도
  * 실행시간은 실행환경에 따라 달라짐(하드웨어, 운영체제, 언어, 컴파일러 등등)
  * 그래서 실행시간을 측정하는 대신 연산의 실행 횟수를 카운트한다.
  * 연산의 실행 횟수는 입력 데이터의 크기에 관한 함수로 표현.
  * 데이터 크기가 같더라도 실제 데이터에 따라서 달라짐.
    * worst-case analysis(최악의 경우 시간 복잡도)
    * average-case analysis(평균 시간복잡도)

* 점근적(Asymptotic) 분석
  * 점근적 표기법을 사용
    * 데이터의 개수가 n->무한대 일때 수행시간이 증가하는 growth rate(성장율)로 시간복잡도를 표현하는 기법
    * 세타 표기, O-(빅오) 표기 등을 사용
  * 유일한 분석법도 아니고 가장 좋은 분석법도 아님
    * 다만 (상대적으로) 가장 간단하며
    * 알고리즘의 실행환경에 비의존적

* 점근적 분석의 예 : 상수 시간복잡도
  * 입력으로 n개의 데이터가 저장된 배열 data가 주어지고, 그 중 n/2번째 데이터를 반환한다.
  ```
    public int simple( int data[], int n) {
      int k = n/2;
      return data[k];
    }
  ```

  * n에 관계없이 상수 시간이 소요된다. 이 경우 알고리즘의 시간복잡도는 O(1)이다.

* 점근적 분석의 예 : 선형 시간복잡도
  * 입력으로 n개의 데이터가 저장된 배열 data가 주어지고, 그 합을 구하여 반환한다.
  ```
    public int sum(int data[], int n) {
      int sum = 0;
      for (int i = 0; i < n; i++) {
        sum += data[i];
      }
      return sum;
    }
  ```
  * 이 알고리즘에서 가장 자주 실행되는 문장은 ```sum += data[i]```이며, 실행 횟수는 항상 n번이다.
  * 가장 자주 실행되는 문장의 실행횟수가 n번이라면 모든 문장의 실행 횟수의 합은 n에 선형적으로 비례하며, 모든 연산들의 실행횟수의 합도 역시 n에 선형적으로 비례한다.
  * 선형 시간복잡도를 가진다고 말하고 O(n)이라고 표기한다.

* 선형 시간복잡도 : 순차탐색
  * 배열 data에 정수 target이 있는지 검색한다.
  ```
    public int search(int data[], int target) {
      for (int i=0; i<data.length; i++) {
        if(data[i] == target) { // 이 알고리즘에서 가장 자주 실행되는 문장이며, 실행횟수는 최악의 경우 n번이다.
          return i;
        }
      }
      return -1;
    }
  ```
  * 최악의 경우 시간복잡도는 O(n)이다.

* 점근적 표기법
  * 알고리즘에 포함된 연산들의 실행 횟수를 표기하는 하나의 기법
  * 최고차항의 차수만으로 표시
  * 따라서 가장 자주 실행되는 연산 혹은 문장의 실행횟수를 고려하는 것으로 충분

* 점근 표기법
  * O- 표기 : upper bound를 표현
  * Ω- 표기 : lower bound를 표현
  * Θ-표기 : upper bound와 lower bound를 동시에 표현

* 다항시간(polynomial-time) 알고리즘
  * An algorithm is efficient if its running time is polynomial.

#### 이진검색과 정렬
* 이진검색
  * 배열 data에 n개의 문자열이 오름차순으로 정렬되어있다.
  ```
    int binarySearch(int n, char* data[], char *target) {
      int begin = 0, end = n-1;
      while(begin <= end) {
        int middle = (begin + end) / 2;
        int result = strcmp(data[middle], target);
        if (result == 0) {
          return middle;
        } else if (result < 0) {
          begin = middle+1;
        } else {
          end = middle-1;
        }
      }
      return -1;
    }
  ```
  * 한 번 비교할 때마다 남아있는 데이터가 절반으로 줄어든다. 따라서 시간복잡도는 O(log2n)이다.
  * 데이터가 연결리스트에 오름차순으로 정렬되어있다면?
    * 연결리스트에서는 가운데(middle) 데이터를 O(1)시간에 읽을 수 없음.
    * 따라서 이진검색을 할 수 없다.
  * 순차검색의 시간복잡도는 O(n)이고 이진검색은 O(log2n)이다. 둘의 차이는 매우 크다.

* 버블 정렬
  ```
    void bubbleSort(int data[], int n) {
      for (int i=n-1; i>0; i--) {
        for (int j=0; j<i; i++) {
          if (data[j] > data[j+1]) {
            int tmp = data[j];
            data[j] = data[j+1];
            data[j+1] = tmp;
          }
        }
      }
    }
  ```
  * 시간 복잡도는?

* 삽입정렬
  ```
    void insertion_sort(int n, int data[]) {
      for (int i=1; i<n; i++) {
        int tmp = data[i];
        int j = i-1;
        while ( j>=0 && data[j] > data[i]) {
          data[j+1] = data[j];
          j--;
        }
        data[j+1] = tmp;
      }
    }
  ```
  * 시간 복잡도는?

* 퀵소트(quicksort) 알고리즘
  * 최악의 경우 O(n*n), 하지만 평균 시간복잡도는 O(nlog2n)
* 최악의 경우 O(nlog2n)의 시간복잡도를 가지는 정렬 알고리즘
  * 합병정렬(merge sort)
  * 힙 정렬(heap sort)등


* 스택을 이용한 미로찾기
  ```
    int maze() {
      Stack s = create();
      Position cur;
      cur.x = 0; cur.y=0;
      int init_dir = 0; // 어떤 위치에 도착했을 때 처음으로 시도해 볼 이동 방향

      while(1) {
        maze[cur.x][cur.y] = VISITED;
        if (cur.x == n-1 && cur.y == n-1) {
          break;
        }

        bool forwarded = false;
        for (int dir = init_dir; dir<4; dir++) {
          if(movable(cur, dir)) { // 어떤 셀도 2번 방문되지 않으며, 이 문장에 의해서 4번 이상 검사되지 않는다.
            push(s, dir);
            cur = move_to(cur, dir);
            forwarded = true;
            init_dir=0;
            break;
          }
        }
        if (!forwarded) {
          maze[cur.x][cur.y] = BACKTRACKED;
          if (is_empty(s)) {
            break;
          }
          int d = pop(s);
          cur = move_to(cur, (d+2)%4);
          init_dir = d+1;
        }
      }
    }
  ```

  * 시간복잡도는 O(n*n)이며 이는 선형시간이다.

* 큐를 이용한 미로 탐색
```

Queue queue = create_queue();
Position cur;
cur.x = 0;
cur.y = 0;
enqueue(queue, cur);
maze[0][0] = -1;
bool found = false;
while(!is_empty(queue)) {
 Position cur = dequeue(queue);
 for (int dir=0; dir<4; dir++) {
 if (movable(cur, dir)) {
 Position p = move_to(cur, dir);
 maze[p.x][p.y] = maze[cur.x][cur.y] - 1;
 if (p.x == n-1 && p.y == n-1) {
 printf("Found the path.\n");
 found = true;
 break;
 }
 enqueue(queue, p);   // 어떤 셀도 2번이상 큐에 들어가지 않는다. 
 }
 }
}

```
