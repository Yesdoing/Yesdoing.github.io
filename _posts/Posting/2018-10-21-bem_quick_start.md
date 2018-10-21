---
layout: post
title: "BEM( Block, Element, Modifier) Quick start"
categories: posting
---


# 들어가며 👀

- 이 글은 [BEM Quick Start](https://en.bem.info/methodology/quick-start/#block) 를 읽고 공부한 내용을 복습하기 위해 번역을 했으며, 내용이 요약된 게 있을 수 있습니다.

# 🐶  BEM 소개

BEM이란 Block, Element, Modifier의 첫 글자만 가져와 이름 지은 Component 기반의 웹 개발 접근법 중 하나 입니다. 

이 방법론은 유저 인터페이스를 독립된 여러 개의 블록으로 분리하자는 것이 목표입니다. 이것은 복잡한 UI를 가진 페이지의 인터페이스 개발 환경을 쉽고 빠르게 하며, Copy and Paste 없이 존재하는 코드의 재활용을 가능하게 합니다. 

# 🐱 Block

기능적으로 독립된 페이지의 Component는 재사용 될 수 있어야 합니다. HTML에서 블록들은 class 속성으로 표현됩니다. 

아래는 Block의 특징들 입니다. 

- 블록의 이름은 그 블록의 목적으로 기술되어야 하며, 그 블록의 상태를 나타내지 않습니다.
    - "이것이 무엇인가?" - menu 또는 button  ⇒  ⭕️
    - "이것이 무엇으로 보이는가?" - red 또는 big ⇒ ❌

        <!-- 예시 -->
        <!-- 옳은 예, 에러 블록이 의미상 적절 합니다. -->
        <div class="error"></div>
        
        <!-- 틀린 예, 블록은 어떻게 표현되어지는 지를 나타내지 않습니다. -->
        <div class="red-text"></div>

- 블록은 자신의 환경에 영향을 끼칠 수 없습니다. 예를 들어 당신은 블록의 positioning(top, left..)이나  margin 값과 같은 외부 기하학적 요소를 설정할 수 없습니다.
- 당신은 또한 BEM을 사용할 때 CSS 태그나 ID 셀렉터를 사용할 수 없습니다.

이런 특징들이 블록의 재사용성이나 블록이 움직일 때 필요한 독립성을 보장합니다. 

# 블록 사용을 위한 가이드라인

## 중첩

- 블록들은 서로 중첩될 수 있습니다.
- 중첩의 레벨은 얼마든지 가질 수 있습니다.

## Example

    <!-- `header` block -->
    <header class="header">
        <!-- Nested `logo` block -->
        <div class="logo"></div>
    
        <!-- Nested `search-form` block -->
        <form class="search-form"></form>
    </header>

# 🐭 Element

블록에서 분리되어 사용될 수 없는 한 블록을 이루는 부분 입니다. 

아래는 Element의 특징들 입니다. 

- Element의 이름은 그 Element의 목적으로 기술되어야 하며, 그 Element의 상태를 나타내지 않습니다.
    - "이것이 무엇인가?" - item 또는 text  ⇒  ⭕️
    - "이것이 무엇으로 보이는가?" - red 또는 big ⇒ ❌
- 한 element의 풀 네임 구조는 `block-name__element-name` 입니다. element 이름은 double underscore(`__`)를 사용해서 블록 이름으로부터 분리되어야 합니다.

    <!-- Example -->
    <!-- `search-form` block -->
    <form class="search-form">
        <!-- `input` element in the `search-form` block -->
        <input class="search-form__input">
    
        <!-- `button` element in the `search-form` block -->
        <button class="search-form__button">Search</button>
    </form>

# Element 사용을 위한 가이드라인

## 중첩

- Element 들은 서로 중첩될 수 있습니다.
- 중첩 레벨은 얼마든지 가질 수 있습니다.
- 한 element는 항상 한 블록의 부분이어야 합니다. 이것은 element의 이름이 계층구조로 선언 될 수 없음을 의미합니다. (`block__elem1__elem2`)

## Example

    <!--
        옳은 예. element의 이름 구조가 아래의 패턴을 따르고 있습니다.
        `block-name__element-name`
    -->
    <form class="search-form">
        <div class="search-form__content">
            <input class="search-form__input">
    
            <button class="search-form__button">Search</button>
        </div>
    </form>
    
    <!--
        틀린 예. element의 이름 구조가 아래의 패턴을 따르지 않습니다.
        `block-name__element-name`
    -->
    <form class="search-form">
        <div class="search-form__content">
            <!-- Recommended: `search-form__input` or `search-form__content-input` -->
            <input class="search-form__content__input">
    
            <!-- Recommended: `search-form__button` or `search-form__content-button` -->
            <button class="search-form__content__button">Search</button>
        </div>
    </form>

블록 이름을 namespace로 정의합니다. 이 namespace는 element들에게 block에 의존하고 있는 것을 보장합니다. (`block__elem`)

한 블록은 DOM tree에서 여러 개로 중첩된 element들을 가질 수 있습니다.

Example

    <div class="block">
        <div class="block__elem1">
            <div class="block__elem2">
                <div class="block__elem3"></div>
            </div>
        </div>
    </div>

그러나, 이 블록 구조는 BEM 방법론에서 element 들의 목록들로서 표현되어 집니다. 

Example 

    .block {}
    .block__elem1 {}
    .block__elem2 {}
    .block__elem3 {}

이것은 각각의 분리된 element에 대해 코드의 변화없이 블록의 DOM 구조를 변화시키는 것을 허락합니다. 

Example

    <div class="block">
        <div class="block__elem1">
            <div class="block__elem2"></div>
        </div>
    
        <div class="block__elem3"></div>
    </div>

블록 구조가 변화되더라도 element들의 규칙과 그들의 이름은 똑같이 유지되어야 합니다. 

## Membership

한 element는 항상 한 블록의 부분이어야 하며, 블록으로부터 분리되어 사용될 수 없습니다. 

Example

    <!-- 옳은 예. element들은 the `search-form` block 내부에 위치하고 있습니다. -->
    <!-- `search-form` block -->
    <form class="search-form">
        <!-- `input` element in the `search-form` block -->
        <input class="search-form__input">
    
        <!-- `button` element in the `search-form` block -->
        <button class="search-form__button">Search</button>
    </form>
    
    <!--
        틀린 예. element들이 the `search-form` block 의 밖에 위치하고 있습니다.
    -->
    <!-- `search-form` block -->
    <form class="search-form">
    </form>
    
    <!-- `input` element in the `search-form` block -->
    <input class="search-form__input">
    
    <!-- `button` element in the `search-form` block-->
    <button class="search-form__button">Search</button>

## Optionality

element는 block component에서 선택사항입니다. 모든 block 들이 element를 가지진 않습니다. 

Example 

    <!-- `search-form` block -->
    <div class="search-form">
        <!-- `input` block -->
        <input class="input">
    
        <!-- `button` block -->
        <button class="button">Search</button>
    </div>

# Block 이나 Element를 만들어 볼까요?

## Block 만들기

만약 코드의 한 부분이 재사용 되어 지고, 그 부분이 구현된 다른 페이지의 컴포넌트들에 의존하지 않는다면 Block으로 만들 수 있습니다. 

## Element 만들기

코드의 한 부분이 부모 entity(the block) 없이 분리되어 사용되어 질 수 없다면 element로 만들 수 있습니다. 

하지만 간소화한 개발을 위하여 작은 부분들로 분리되어야 하는 element들은 예외입니다. BEM 방법론에서 element 들의 element 들은 만들 수 없습니다. 이와 같은 경우, element를 만드는 것 대신에 service 블록을 만들어야 합니다. 

# 🦁 Modifier

block이나 element의 행동이나 상태, 외양을 정의하는 entity 입니다. 

아래는 Modifier의 특징들 입니다. 

- modifier 이름은 그 자신의 외양이나 상태, 행동을 기술해야 합니다.
    - "사이즈는 무엇입니까?" or "어떤 테마입니까?" ⇒ `size_s` or `theme_islands`
    - "서로 어떻게 다른가요?" ⇒ `disabled` or `focused`
    - "그것은 어떻게 동작하나요?" or "유저에게 어떻게 반응하나요?" ⇒ `directions_left-top`
- modifier의 이름은 block이나 element 이름으로부터 single underscore(`_`)로 분리되어야 합니다.

# Types of modifiers

## Boolean

- modifier의 유무가 중요할 때 사용됩니다. 예를 들어, 값이 상관없으면  `disabled` . 만약 Boolean modifier가 나타난다면 그 값은 `true` 로 추측되어 질 겁니다.
- modifier의 풀 네임 구조는 아래 패턴을 따릅니다.
    - `block-name_modifier-name`
    - `block-name__element-name_modifier-name`

Example

    <!-- The `search-form` block 은 `focused` Boolean modifier를 가집니다. -->
    <form class="search-form search-form_focused">
        <input class="search-form__input">
    
        <!-- The `button` element 는 `disabled` Boolean modifier 를 가집니다.-->
        <button class="search-form__button search-form__button_disabled">Search</button>
    </form>

## Key-value

- modifier 값이 중요할 때 사용됩니다. 예를 들어, "`islands` 디자인 테마를 가진 메뉴" : `menu_theme_islands`
- modifier의 풀 네임 구조는 아래를 따릅니다.
    - `block-name_modifier-name_modifier-value`
    - `block-name__element-name_modifier-name_modifier-value`

Example 

    <!-- The `search-form` block 은 the value `islands`를 가진 `theme` modifier를 가집니다.   -->
    <form class="search-form search-form_theme_islands">
        <input class="search-form__input">
    
        <!-- The `button` element has the `size` modifier with the value `m` -->
        <button class="search-form__button search-form__button_size_m">Search</button>
    </form>
    
    <!-- two identical modifiers는 different values 상황적으로 가집니다. -->
    <form class="search-form
                 search-form_theme_islands
                 search-form_theme_lite">
    
        <input class="search-form__input">
    
        <button class="search-form__button
                       search-form__button_size_s
                       search-form__button_size_m">
            Search
        </button>
    </form>

# 🐧 Mix

단일 DOM node에서 다른 BEM entity들을 사용하기 위한 기술 

아래와 같이 혼합할 수 있습니다. 

- 행동과 다수의 entity들을 코드의 중첩없이 결합
- 이미 존재하는 UI 컴포넌트를 기반으로 한 의미 상 새로운 UI 컴포넌트를 만들 때

Example 

    <!-- `header` block -->
    <div class="header">
        <!--
            `search-form` block 은 the `header` block의 `search-form` element 와 
    					같이 사용되어 집니다.
        -->
        <div class="search-form header__search-form"></div>
    </div>

위 예제에서 행동(behavior)과 `search-form` block의 스타일과 `header` block 의 `search-form` element 의 스타일을 결합했습니다. 이런 접근은 `search-form` block이 일반적으로 유지되는 동안, `header__search-form` element의 위치와 margin값을 설정할 수 있습니다.  결과적으로, 우리는 어떤 padding값도 정의하지 않기 때문에 모든 환경에서도 block을 사용할 수 있습니다. 이것이 우리가 독립적이라고 부를 수 있는 이유입니다. 

# 🐯 File structure

BEM 방법론에서 채택 된 컴포넌트 접근법은 또한 프로젝트의 파일구조에도 적용된다.  block과 element, modifier의 구현은 독립적인 기술 파일들로 분리된다. 이 분리된 파일들을 각각 따로 연결할 수 있다. 

File structure은 아래의 특성을 지닌다. 

- 단일 block은 단일 directory를 가진다.
- block과 directory는 같은 이름을 가진다. 예를 들어, `header` block은 `header/` directory안에, 있고 `menu` block은 `menu/` directory 안에 있다.
- block들의 구현체는 독립된 기술 파일들로 나뉘어진다. 예를 들어,  `header.css`와 `header.js`로 구성된다.
- block directory는 그 block의 element들과 modifier들을 위한 subdirectory를 위해 root directory에 있다.
- element directory의 이름은 이중 underscore(`__`)로 시작한다. 예를 들어, `header/__logo` 나 `menu/__item/` 과 같다.
- modifier  directory의 이름은 단일 underscore(`_`)로 시작한다. 예를 들어, `header/_fixed/` 와 `menu/_theme_islands/` 와 같다.
- element들과 modifier의 구현체는 독립된 기술 파일들로 분리된다. 예를 들어, `header__input.js`와 `header_theme_islands.css` 와 같다.

Example

    search-form/                           # Directory of the search-form
    
        __input/                           # Subdirectory of the search-form__input
            search-form__input.css         # CSS implementation of the
                                           # search-form__input element
            search-form__input.js          # JavaScript implementation of the
                                           # search-form__input element
    
        __button/                          # Subdirectory of the search-form__button
                                           # element
            search-form__button.css
            search-form__button.js
    
        _theme/                            # Subdirectory of the search-form_theme
                                           # modifier
            search-form_theme_islands.css  # CSS implementation of the search-form block
                                           # that has the theme modifier with the value
                                           # islands
            search-form_theme_lite.css     # CSS implementation of the search-form block
                                           # that has the theme modifier with the value
                                           # lite
    
        search-form.css                    # CSS implementation of the search-form block
        search-form.js                     # JavaScript implementation of the
                                           # search-form block

위와 같은 파일 구조는 코드의 재사용성을 높인다. 

위의 파일 구조는 추천 사항이지 필수 사항이 아니다. 다른 대안의 프로젝트 구조를 사용 할 수 있으며 파일을 구성하기 위한 BEM 원칙을 따르기만 해도 된다. 

원문 : [BEM Quick start](https://en.bem.info/methodology/quick-start/#modifier)