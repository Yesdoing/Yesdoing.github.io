---
layout: post
title: "ch02 - powerset"
categories:
  - 알고리즘
tags:
  - recursion
---

## powerset(멱집합)
  * 집합론에서, 어떤 집합의 멱집합은 그 집합의 모든 부분 집합을 모은 집합이다.
  * 원소의 갯수가 n인 집합에 모든 가능한 부분집합의 개수는 2^n개이다.
  * data = {a,b,c,d}인 집합이 잇을때 부분집합의 개수는 2^4 = 16개이다.
  * {a, b, c, d, e, f}의 모든 부분집합을 나열하려면
    * a를 제외한 {b, c, d, e, f}의 모든 부분집합들을 나열하고 -> 2^5
    * {b, c, d, e, f}의 모든 부분집합에 {a}를 추가한 집합들을 나열한다. -> 2^5
    * 더하면 2^6
  * {b,c,d,e,f}의 모든 부분집합에 {a}를 추가한 집합들을 나열하려면
    * {c,d,e,f}의 모든 부분집합들에 {a}를 추가한 집합들을 나열하고
    * {c,d,e,f}의 모든 부분집합에 {a,b}를 추가한 집합들을 나열한다.
  * {c,d,e,f}의 모든 부분집합에 {a}를 추가한 집합들을 나열하려면
    * {d,e,f}의 모든 부분집합들에 {a}를 추가한 집합들을 나열하고
    * {d,e,f}의 모든 부분집합에 {a,c}를 추가한 집합들을 나열한다.   

  * 스도코드
    ```
      powerSet(S)
        if S is an empty set
          print nothing;
        else
          let t be the first element of S;
          find all subsets of S-{t} by calling powerSet(S-{t});
          print the subsets;
          print the subsets with adding t;
    ```
    * 이렇게 하려면 powerSet 함수는 여러 개의 집합들을 return해야 한다.
  * 스도코드2
    ```
      // Mission: S의 멱집합을 구한 후 각각에 집합 P를 합집합하여 출력하라.
      powerSet(P, S)
      if S is an empty set
        print P;
      else
        let t be the first element of S;
        powerSet(P, S-{t});
        powerSet(P U {t}, S-{t});
    ```
    * recursion 함수가 두 개의 집합을 매개변수로 받도록 설계해야 한다는 의미다. 두 번째 집합의 모든 부분집합들에 첫 번째 집합을 합집합하여 출력한다.

  * 실제 코드
    ```
      // Mission: data[k], ..., data[n-1]의 멱집합을 구한 후 각각에 include[i] = true, i=0, ..., k-1, 인 원소를 추가하여 출력하라.
      // 처음 이 함수를 호출할 때는 powerSet(0)로 호출한다. 즉 P는 공집합이고 S는 전체 집합이다.
      public class PowerSet {
      	private static char data[] = {'a', 'b', 'c', 'd', 'e', 'f'};
      	private static int n = data.length;
      	private static boolean[] include = new boolean[n];

      	public static void powerSet(int k) {
      		if (k==n) {
      			for(int i=0; i<n; i++) {
      				if(include[i]) System.out.print(data[i] + " ");
      			}
      			System.out.println();
      			return;
      		}

      		include[k] = false;
      		powerSet(k+1);
      		include[k] = true;
      		powerSet(k+1);
      	}

      	public static void main(String[] args) {
      		powerSet(0);
      	}
      }
    ```

### 상태공간트리 (state space tree)
  * 해를 찾기 위해 탐색할 필요가 있는 모든 후보(노드)들을 포함하는 트리(정의)
  * 트리의 모든 노드들을 방문하면 해를 찾을 수 있다.
  * 루트에서 출발하여 체계적으로 모든 노드를 방문하는 절차를 기술한다.
  * 코드 예시
    * 위의 코드에서 전역변수 include와 매개변수 int k가 트리상에서 현재 나의 위치를 표현한다.
    * if(k==n)는 만약 내 위치가 리프노드라면
    * 첫번째 powerSet은 왼쪽으로 내려가는 것.
    * 첫번째 powerSet은 오른쪽으로 내려가는 것.
