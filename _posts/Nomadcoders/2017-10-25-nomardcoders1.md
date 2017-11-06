---
layout: post
title: "노마드코더 #1"
categories:
  - 노마드코더
tags:
  - 니꼴라스
  - HTML
  - CSS
  - git
---

### git이란?
  * 사용자가 파일에서 만든 변경사항을 추적하는 시스템
  * 워드 이미지 포토샴 혹은 소스코드 등등..
  * 변경사항을 다알려준다.(백업, 언제 무엇을 어떻게 변경햇는지, 팀으로 함께 일햇는지? 등등)

### version control의 개념
  * repository - 소스코더를 저장하는 폴더
  * commit - 파일 변경 기록, 프로젝트의 상황판
  * branch - 나무의 가지, 새로운 기능을 만들때 가지를 하나 생성해서 개발하고 작업이 끝나면 마스터에 결합.

### git vs github
  * 커피와 커피허브의 관계 (커피허브는 저장소, 커피는 내용물)
  * 깃 허브 이외에도 다양한 서비스가 있다.
    * 빗버켓(private git 관리 가능)
    * 깃허브(모두가 아는 그것.)

### HTML
  * Hyper Text Mark up Language
  * 브라우저에게 이건 제목, 이건 링크, 이건 리스트, 이건 문단, 헤더, 푸터, ..., 등등을 알려주는 것.

### CSS
  * 브라우저에게 어떻게 생겨먹엇는지 알려주는 것.
  * cascading style sheet

### HTML 이론
  * HTML은 tag들로 이루어져 있다.
  * <!DOCTYPE html>은 브라우저에게 html 문서임을 말해준다.
  * 위 태그는 self-contained tag다. 닫지 않아도 그 자체 정보가 있을뿐이라 열고 닫을 필요가 없는 태그다.
  * html 태그는 2가지 태그를 포함한다. head와 body이다.
  * head태그는 보통 웹사이트에게 보이지 않고 구글 크롬 같은 브라우저에게 필요한 정보를 제공할 뿐이다. 사람들이 읽는 컨탠츠는 body에 있다.
  * meta 태그 - 엑스트라, 추가정보 (정말 정말 많다. 어떤 특정 플랫폼을 위한 태그도 있다.)
    * 이 정보는 브라우저를 위한 정보라 유저에게 보이지 않는다.
  * Semantic 태그 vs Non Semantic 태그
    * Non Semantic tags - 뜻이 없는 태그
      * div, span - text를 위한 컨테이너
    * Semantic tags - 뜻이 잇는 태그
      * header, section, article
  * ID vs Class
    * id는 각 element마다 한개만 갖을 수 있다. 왜냐하면 ID는 고유하기 때문(여권번호같이)
    * Class는 여러개가 존재 가능, class는 반복해서 여러번 사용가능
    * class를 주로 사용한다 element중에 고유한게 많이 없다.

### CSS Syntax
  ```
    Selector (id, tag, class) {
      property-name: value;
      property-name: value;
      property-name: value;
      property-name: value;
    }
  ```

  * Inline vs Block vs Inline Block
    * Inline - Text라고 생각하자 (Content size만큼만 적용됨)
    * Block - 박스들이 밑에 붙는것 박스옆에 박스가 존재할수없다.
    * Inline Block - 박스들이 일자로 나열될수 있는것

  * Position property
    * default : static - element를 붙이면 거기에 고정되어있다.
    * fixed - element가 그위치에 오버랩해서 고정된다. 스크롤해도 보임
    * absolute - fixed와 비슷하지만 스크롤해도 안보임. html상에서 해당 element와 관계잇는 relative elements를 살펴보고 이에 상응하는 포지션이 결정된다.

  * flex
    * flex는 부모 박스에만 적용을 한다.
    * 자식 박스는 건드리지 않는다.
    * 부모-박스가 아래 딸려있는 칠드런 박스를 움직인다. 그리고 박스가 그 안에 들어있는 컨텐츠(1)을 움직인다.(플렉스박스라)
    * 부모 컨테이너(father)를 플렉스로 선언하면, 그 안에 종속된 칠드런 박스들을(children) 움지깅ㄹ 수 있다. 그렇기때문1에, 각각 박스에게 일일히 명령할 필요가 없다.
    * [flex연습사이트](http://flexboxfroggy.com/#ko)

  * CSS Selectors and Pseudo Selectors
    * 레이아웃이 복잡해질때가 있다.
    * 팀으로 일할때 class로 style을 조정할 수 있지만 팀으로 일할때는 막 하면 안된다.
    * Pseudo Selector(가상 셀렉터) - 셀렉터인데 element가 아닌 것을 뜻함.
      * 가상 셀렉터의 컨셉을 이해하는것이 중요하다. 태그이름이나, class, id를 쓰지 않고, 선택하는 방법이 있다는 것.
      * 사용법 : ```input[type="password"]{ background-color: blue;}```

  * Element State with CSS
    * :hover = 위에 마우스 올리면 변화
    * :active = 마우스 클릭 시 변화
    * :focus = 경계색

  * Transitions(트랜지션)
    * focus, active, hover에 적용됨
    * 효과과 적용될때 시간을 설정해서 시간이 지남에 따라 효과가 적용되게 한다.
    * 하나의 state에서 다른 state로 넘어가는 것

  * Transformations (트랜스포메이션)
    * html 문서의 element들을 변경, 모습이 변하는 효과를 뜻함.
    * 뒤집거나. 비틀거나. 크기를 줄이거나 등등.

  * Animations
    * 트랜지션과 트랜스포메이션의 효과를 계속 적용하고 싶을 때
    * @keyFrames를 만들어 from-to || 0%-50%-100% 에 효과 적용

  * Media Queries
    * 화면에 따른 css적용 변화
    * 반응형 웹디자인에 사용(모바일-데스크탑 환경)
