---
layout: post
title: "ch02 - n queens problem"
categories:
  - 알고리즘
tags:
  - recursion
---

## n queens problem
* 가로 세로 크기가 N*N인 배열이 주어진다.
* 배열에는 N개의 말을 놓을 수 있는데 가로,세로에 중복되는 말이 있으면 안된다.
* Back tracking - 내가 결정을 하며 내려가다가 막다른 길에 다다르면 가장 최근의 결정을 번복하고 다시 선택하는 것.
* 이것을 시각적으로 가시화 하면 상태공간트리가 나온다.
  * 상태 공간 트리란 찾는 해를 포함하는 트리.
  * 즉 해가 존재한다면 그것은 반드시 이 트리의 어떤 한 노드에 해당함. 따라서 이 트리를 체계적으로 탐색하면(모든 노드를 다 찾는다면) 해를 구할 수 있음.
  * 모든 경우의 수를 나열하는 것.(모두 할려면 엄청나게 커진다.)

### 상태공간트리
  * 상태공간 트리의 모든 노드를 탐색해야 하는 것은 아님
  * 개념적인, 논리적인 것이라고 생각하자.
  * 구현코드는 트리의 노드를 탐색하는 코드를 만들자.(트리 자체를 만드는 것이 아님.)
  * 실제로는 조건에 의해 방문하지 않아도 되는 노드가 있다.(infeasible, 해가 되기위한 기본적인 조건 불충족)

### 되추적 기법(Backtracking)
  * 상태공간 트리를 깊이 우선 방식으로 탐색하여 해를 찾는 알고리즘을 말한다.
  * 구현을 할때는 recursion을 사용하거나 stack자료형을 사용해서 구현할 수 있다.
  * recursion을 이용하여 구현하는게 쉬워서 주로 이걸로 구현한다.

### Design Recursion
  ```
     return-type queens( arguments ) {    // 함수가 실행되는 순간은 노드에 도착하는 순간이다. 실행내용은 어떤 노드에 도착했을때 처리해야하는 일의 순서.
       if infeasible                      // 내가 현재 도착한 노드가 답이 될가능성이 있는가?, 내려갈 필요가 있는가?
        report failure and return;        // 문제에 따라서 꽝이라 할필요가 있고 그냥 되돌아가도 된다.
      else if success                     // 내가 현재 방문한 노드가 내가 찾고있는 답인가?
        report answer and return;
      else
        visit children recursively;       // 모든 경우가 아니라면 recursively하게 탐색한다.
     }
  ```
  * 이 함수가 호출이 된다는 것은 어떤노드에 도착했다는 것.
  * 내가 어떤노드에 도착했는지에 대한 정보가 매개변수로 주어져야한다.
  * 모든 정보를 매개변수로 주면 매개변수가 너무 많아질 수 있다.
  * cols[level]열에 놓엿다. 상태공간트리에서 어떤 노드에 있는지 알려준다.
  * return-type 은 boolean으로 한다.
  ```
    int [] cols = new int[N+1];  
     boolean queens( int level ) {
       if (!promising(level))
        report false;
      else if (level==N)
        for(int i=1; i<=N; i++)
          System.out.println("(" + i + ", " + cols[i] + ")");
        report true;
      for (int i=1; i<=N; i++) {
        cols[level+1] = i;
        if (queens(level+1))
          return true;
      }
      return false;
     }
  ```
  * promising Test
    * 충돌이 일어나는지 검사하는 함수
    * cols에 담긴 모든 말을 검사할 필요가 없다. level 이전의 값들은 충돌이 없음이 보장되어 있음(이전에 검사를 했기때문에)
    * 따라서 마지막에 놓인 이 말이 이전에 놓인 다른 말들과 충돌하는지 검사하는 것으로 충분하다.
    * 1~level-1까지의 말이 level번째 말과 충돌하는지 검사.
    ```
      boolean promising( int level ) {
        for (int i=1; i<level; i++) {
          if(cols[i] == cols[level])
            return false;
          else if (level -i == Math.abs(cols[level]-cols[i])) // 대각선 검사
            return false;
        }
        return true;
      }
    ```
