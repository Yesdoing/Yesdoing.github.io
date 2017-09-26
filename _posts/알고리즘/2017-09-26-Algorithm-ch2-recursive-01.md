---
layout: post
title: "ch02 - Recursion01"
categories:
  - 알고리즘
tags:
  - recursion
---

## 순환(Recursion의 개념과 기본 예제 1
### 재귀란?
* 자기 자신을 호출하는 함수.
  ```
    void func(...) {
      ...
      func(...);
      ...
    }
  ```

* 예시1. - 종료 조건이 없기때문에 무한루프에 빠진다.
  ```
  public class Code01 {
  	public static void main(String[] args) {
  		func();			// 종료 조건이 없기때문에 무한루프에 빠짐.
  	}

  	public static void func() {
  		System.out.println("Hello...");
  		func();
  	}
  }
  ```
* 예시2.  - 적절한 종료조건을 사용하여 무한루프에 빠지지 않음.
  ```
  public class Code02 {
  	public static void main(String[] args) {
  		int n = 4;
  		func(n);
  	}

  	public static void func(int k) {
  		if( k <= 0 ) {  // Base case : 적어도 하나의 recursion에 빠지지 않는 경우가 존재해야 한다.
  			return;
  		} else {        
  			System.out.println("Hello...");
  			func(k-1);   // Recursive case : recursion을 반복하다보면 결국 base case로 수렴해야 한다.
  		}
  	}
  }
  ```

  * 적어도 하나의 base case가 존재해야한다.  - 조건 1
  * recursion을 반복하다 보면 결국 base case로 수렴해야한다. - 조건 2
  * 조건 1과 조건 2가 충족해야 Recursion은 무한루프에 빠지지 않는다.

* 예시3.  - 1~n의 합을 구하는 함수
  ```
  public class Code01 {
  	public static void main(String[] args) {
  		int result = func(4);
  		System.out.println(result);
  	}

  	public static int func(int n) {
  		if(n<=0) {
  			return 0;
  		} else {
  			return n + func(n-1);			
  		}
  	}
  }
  ```

  * base case가 실행된 함수로 리턴된다. 0, 1, 3, 6, 10의 순서로 결과가 반환된다.

  * recursion의 해석
  ```
    public static int func(int n) { // 이 함수의 mission은 0~n까지의 합을 구하는 것이다.
      if (n==0) {
        return 0;                   // n=0이라면 합은 0이다.
      } else {
        return n + func(n-1);       // n이 0보다 크면 0에서 n까지의 합은 0에서 n-1까지의 합에 n을 더한 것이다.
      }
    }
  ```
  * 이 함수의 mission은 0~n까지의 합을 구하는 것이다.
  * n=0이라면 합은 0이다.
  * n이 0보다 크면 0에서 n까지의 합은 0에서 n-1까지의 합에 n을 더한 것이다.

* 순환함수와 수학적 귀납법
  * 정리 : func(int n)은 음이 아닌 정수 n에 대해서 0에서 n까지의 합을 올바르게 계산한다.
  * 증명 :
    1. n=0인 경우 : n=0인 경우 0을 반환한다. 올바르다.
    2. 임의의 양의 정수 k에 대해서 n < k 인 경우 0에서 n까지의 합을 올바르게 계산하여 반환한다고 가정하자.
    3. n=k인 경우를 고려해보자. func은 먼저 func(k-1) 호출하는데 2번의 가정에 의해서 0에서 k-1까지의 합이 올바로 계산되어 반환된다. 메서드 func은 그 값에 n을 더해서 반환한다. 따라서 func은 0에서 k까지의 합을 올바로 계산하여 반환한다.

* Recursion이 일반적인 예
  * Factorial : n!
    * 0! = 1
    * n! = n*(n-1)! n>0
    ```
      public static int factorial(int n) {
        if (n==0) {
          return 1;
        } else {
          return n*factorial(n-1);
        }
      }
    ```

    * factorial(4) <- return 4 * factorial(3) <- 3 * factorial(2) <- 2 * factorial(1) <- 1 * factorial(0) <- 1

    * 정리 : factorial(int n)은 음이 아닌 정수 n에 대해서 n!을 올바로 계산한다.
    * 증명 :
      1. n=0인 경우 : n=0인 경우 1을 반환한다. 올바르다.
      2. 임의의 양의 정수 k에 대해서 n<k인 경우 n!을 올바르게 계산한다고 가정하자.
      3. n=k인 경우를 고려해보자. factorial은 먼저 factorial(k-1) 호출하는데 2번의 가정에 의해서 (k-1)!이 올바로 계산되어 반환된다. 따라서 메서드 factorial은 k*(k-1)! = k! 을 반환한다.

    * Fibonacci
      ```
        public static int fibonacci(int n) {
      		if (n < 2) {
      			return n;
      		} else {
      			return fibonacci(n-1) + fibonacci(n-2);
      		}
      	}
      ```
    * Euclid
      * 유클리드 호제법은 최대 공약수를 구하는 방법이다.
      * 두 수 a와 b가 있을 때 a를 b로 나눈 나머지는 r이라고 하자(단, a>b)
      * a와 b의 최대공약수는 b와 r의 최대공약수와 같다고 하고, 이 성질에 따라 b를 r로 나눈 나머지를 구하고 다시 r을 r’로 나눈 나머지를 구하는 과정을 반복하여 나머지가 0이 되었을 때 나누는 수 b가 최대 공약수가 된다.
      * 아래의 경우 p와 q 중 누가 큰지 작은지는 상관이 없는것이 만약 q가 p보다 크면 서로의 값이 바뀐다.
      ```
        public static int gcd(int p, int q) {
      		if(q==0) {
      			return p;
      		} else {
      			return gcd(q, p&q);
      		}
      	}      
      ```
