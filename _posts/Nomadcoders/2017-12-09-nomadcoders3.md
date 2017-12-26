---
layout: post
title: "[실전] React Native로 온도앱 만들기"
categories:
  -	노마드코더
tags:
  -	ReactJS
  -	React Native
---

### 개발 환경

1.	expo.io
2.	VSC

### What is React Native

-	리액트 네이티브는 html css 파일을 생성하지 않는다.
-	리액트 네이티브는 object-c나 java이다. object-c = 아이폰, java = 안드로이드 이다.
-	마지막 컴파일링 할때, 각각 IOS/android 네이티브 코드로 실행된다.
-	JSX, 자바스크립트로 코드를 완성하고 그 후에 object-c나 java로 변환된다.
-	브릿지 된다고 표현한다.

### What is expo

-	리액트 네이티브로 앱을 만드는것을 도와준다.

### Basic React Native Concepts

-	div, html이 없다.
-	우리가 return할 수 있는 컴포넌트가 정해져 있다. (view, text ... )
-	React Native를 쓸때는 import를 해야한다.
-	css와 비슷한 styles가 있다.
-	엄청나게 엄격하다.

### React Native Layouts with Flexbox

-	flexbox로 디자인을 짤수 있는 앱은 리액트네이티브가 유일. - 웹 개발을 이미 배웠을 때 매우 장점!
-	하지만 css와 다르다. property가 조금씩 다르다!
-	모든 value는 문자열이고 calmel case를 쓴다.
-	리액트 네이티브에서는 shorten handle property는 통하지 않는다.

### Building the Weather View

-	linear gradient는 두 가지 property가 필요한데, 한개는 색상이고 나머지 하나는 스타일이다.
-	Expo에서 제공하는 Icon을 사용하면 많은 아이콘을 사용가능하다. [링크](https://expo.github.io/vector-icons/)
