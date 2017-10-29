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
