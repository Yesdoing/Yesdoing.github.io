---
layout: post
title: "꼬리 물기 최적화(Tail Call Optimization)란?"
categories: posting
---

![image.png](https://images.velog.io/post-images/yesdoing/bbb3b9a0-da8a-11e8-8483-39fd55313fb0/image.png)

출처: [https://mbtskoudsalg.com/explore/ouroboros-transparent-png/](https://mbtskoudsalg.com/explore/ouroboros-transparent-png/)

# 😺 들어가기 앞서...

이 글을 쓰게 된 계기는 [코드스피츠 3rd-3 ES6+ 함수와 OOP 2회차](https://youtu.be/h80tLv0fn88) 강의 영상을 보고 관련된 내용을 조사해서 요약한 내용입니다. 제가 이해한 부분을 작성하는 것이므로 미흡한 점이나 잘못된 점이 있으면 댓글로 알려주시면 감사하겠습니다. 🙏

# 😸 꼬리 물기 최적화란 무엇인가?

간단하게 말하자면, 서브루틴 체인이 일어날 때 각 서브루틴의 return에 함수를 호출하는 것을 말합니다.

아직 어렵네요, 좀 더 단순화해서 말하자면 꼬리 호출(Tail Call)을 최적화하는 것을 말합니다. 꼬리 호출이란 위키피디아에 따르면 "서브루틴의 호출을 프로시저(procedure)의 마지막 행위로 수행하는 것"을 뜻합니다. 익숙한 예로는 재귀 호출을 생각하시면 됩니다. 

그리고 최적화란 Return을 할 때 함수를 호출하면 "호출이 된" 함수에서 "호출을 한" 함수로 돌아오는 반환 지점을 가지고 있어야 합니다. 만약 "호출을 한" 함수가 메모리(실행 컨텍스트 - Argument, Local Variable)를 가지지 않는다면 "호출을 한" 함수로 돌아올 필요가 없으며, 함수들이 한 번씩 "호출이 되고" 마지막으로 "호출이 된" 함수는 최초 함수 호출 지점으로 값을 반환하는 것을 뜻합니다.

이것이 가능하기 위해서는,  return에서는 연산자를 사용하면 안 됩니다.(언어 스펙에서 지정한 스택에 메모리를 쌓지 않는 연산자는 사용 가능합니다.)

말이 너무 어렵습니다. 예제를 보시죠.

## Tail recursion의 올바른 예

    // 삼항 연산자는 JS 스펙에 정의된 콜스택에 메모리가 잡히지 않는 연산자입니다.
    const factorial = (x, acc=1) => {
    	return ( x <= 1 ? acc : factorial(x-1, x*acc));
    };


![image.png](https://images.velog.io/post-images/yesdoing/b7d00c40-da89-11e8-8483-39fd55313fb0/image.png)


## Tail recursion의 틀린 예

    // 삼항 연산자는 JS 스펙에 정의된 콜스택에 메모리가 잡히지 않는 연산자입니다.
    const factorial = (x) => {
    	return ( x <= 0 ? 1 : x * factorial(x-1));
    };


![image.png](https://images.velog.io/post-images/yesdoing/bd3ff460-da89-11e8-8483-39fd55313fb0/image.png)


위의 그림을 보시면 서브루틴 체인이 발생할 때 실행 컨텍스트(execution context)가 있으면 차례대로 함수가 호출이 된 순서대로 스택에 메모리가 쌓입니다. 하지만 최적화를 할 경우, 호출된 함수에 대한 메모리가 잡히지 않으므로 함수가 호출된 순서대로 실행되며 마지막 함수는 최초 호출 부의 값을 반환하고 종료됩니다.

종합하자면 서브루틴의 연쇄가 발생하였지만,  서브루틴이 루틴으로 돌아갈 필요가 없으면, 마지막 서브루틴에서 최초 호출 부로 반환 지점을 지정하여 Stack에 쌓이는 메모리를 최소화하기 위함입니다.

이 과정들은 서브루틴의 반환 지점(Return Point)을 수정하는 과정을 뜻합니다. 이것은 언어 엔진에서 정의되어 있어야 최적화를 할 수 있으며, 개발자가 임의로 적용하지는 못합니다. 

# 😹 그럼 왜 꼬리 물기 최적화를  할까요?

위의 예제를 보면 콜 스택이 쌓이는 게 다르다는 것을 볼 수 있습니다. 이것은 메모리의 최적화를 할 수 있다는 것을 의미합니다. 

이 [사이트](https://kangax.github.io/compat-table/es6/)를 보면 알 수 있지만 아직 데스크탑에서는 사파리, 모바일에서는 IOS 11.3, 12를 제외한 나머지 브라우저는 지원하지 않습니다. 

하지만 JavaScript는 ES6 스펙에서 꼬리 물기 최적화를 지원한다고 명시하고 있고, 이 [사이트](https://www.linkedin.com/pulse/tail-call-optimizations-es6-michael-clark/)를 보면 Chrome65에서 꼬리 물기 최적화를 할 시 약 17배 속도 향상을 한다고 나와 있습니다. 앞으로 웹 브라우저가 꼬리 물기 최적화를 지원하게 될 것을 대비해 미리 이렇게 코딩을 하기 시작하는 것이 좋다고 생각합니다.

# 😻 꼬리 물기 최적화에 대해 자세히 설명된 사이트들

1. [코드스피츠 3rd-3 ES6+ 함수와 OOP 2회차](https://youtu.be/h80tLv0fn88)
2. [Tail call optimization in ECMAScript 6](http://2ality.com/2015/06/tail-call-optimization.html#the-conditional-operator)
3. [ES6 Tail Call Optimization Explained](http://benignbemine.github.io/2015/07/19/es6-tail-calls/)
4. [ECMAScript 6 Proper Tail Calls in WebKit](https://webkit.org/blog/6240/ecmascript-6-proper-tail-calls-in-webkit/)