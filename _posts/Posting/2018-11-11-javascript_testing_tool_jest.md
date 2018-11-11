---
layout: post
title: "Javascript Testing Tool - Jest"
categories: posting
---


# 🐣 What is the Jest?

Jest란 페이스북에서 사용되는 리액트 애플리케이션을 포함한 모든 자바스크립트 테스트 도구입니다. Jest의 철학 중 하나는 "Zero-configuration", 설정 없는 테스트 환경을 제공하는 것입니다. 이들은 개발자에게 사용 준비가 된 도구가 주어질 때, 개발자가 보다 많은 test를 작성한다고 말합니다. 그리고 이런 테스트는 보다 안정적이고 건강한 코드의 결과로 돌아옵니다. 

# 🐥 How to install Jest?

Jest는 Zero-Configuration이라는 단어에 걸맞게 `yarn` 이나 `npm`  으로 설치만 하면 끝입니다. 

- `yarn` 을 이용한 설치
```shell
    yarn add --dev jest
```
- `npm` 을 이용한 설치
```shell
    npm install --save-dev jest
```
   

# 🐔 How to use Jest?

Jest는 여러가지 타입의 테스트 기능을 제공합니다. 

- Common Matchers
```javascript
    test('adds 1 + 2 to equal 3', () => {
        expect(sum(1, 2)).toBe(3);
    });
    
    test('object assignment', () => {
        const data = { one: 1};
        data['two'] = 2;
        expect(data).toEqual({one: 1, two: 2});
    });
    
    test('adding positive numbers is not zero', () => {
        for (let a = 1; a < 10; a++) {
            for (let b = 1; b < 10; b++) {
                expect(a + b).not.toBe(0);
            }
        }
    });
```
- Truthiness
    - 참, 거짓 테스트에서는 때때로 undefined, null, false를 구별해 줘야 할 때가 있습니다.
    - 하지만 사용자가 이것의 구별을 원하지 않는다면 그에 따른 기능도 제공합니다.
```javascript
    test('null', () => {
        const n = null;
        expect(n).toBeNull();           // only null
        expect(n).toBeUndefined();      // only undefined 
        expect(n).not.toBeUndefined();  // opposite of toBeUndefined
        expect(n).not.toBeTruthy();     // Anything that an if statement treats as true  
        expect(n).toBeFalsy();          // Anything that an if statement treats as false 
    });
    
    test('zero', () => {
        const z = 0;
        expect(z).not.toBeNull();
        expect(z).toBeDefined();
        expect(z).not.toBeUndefined();
        expect(z).not.toBeTruthy();
        expect(z).toBeFalsy();
    });
```
- Number
```javascript
    test('two plus two', () => {
        const value = 2 + 2;
        expect(value).toBeGreaterThan(3);
        expect(value).toBeGreaterThanOrEqual(3.5);
        expect(value).toBeLessThan(5);
        expect(value).toBeLessThanOrEqual(4.5);
    
        expect(value).toBe(4);
        expect(value).toEqual(4);
    });
    
    // float값 테스트 시, toEqual 대신에 toBeCloseTo를 사용해야합니다.
    // toEqual 사용시 미세하게 값이 다르게 나올 수 있어 테스트가 틀릴수도 있습니다.
    
    test('adding floating point numbers', () => {
        const value = 0.1 + 0.2;
        expect(value).toBeCloseTo(0.3);
    });
    
    // Strings
    test('there is no I in team', () => {
        expect('team').not.toMatch(/I/);
    });
    
    test('but there is a "stop" in Christoph', () => {
        expect('Christoph').toMatch(/stop/);
    });
```
- Array
```javascript
    const shoppingList = [
        'diapers',
        'kleenex',
        'trash bags',
        'paper towels',
        'beer'
    ];
    
    test('the shopping list has beer on it', () => {
        expect(shoppingList).toContain('beer');
    });
```
# 🍗  Unit Test Example

Jest를 사용해보기 위해 간단한 로또번호 생성기를 만들어 보았습니다.

- 요구사항
    - 돈을 입력하면 금액에 맞는 로또 번호들이 자동으로 생성되어 출력됩니다.

- Host code
```javascript
    const myLottos = shop.buyTicket('5000');
    
    console.log(myLottos.showTickets());
    /*
    [
    	[ 10, 2, 15, 28, 26, 8, 31 ],
      [ 16, 45, 17, 20, 30, 29, 24 ],
      [ 31, 15, 4, 28, 42, 35, 24 ],
      [ 5, 34, 35, 22, 20, 40, 11 ],
      [ 5, 13, 25, 38, 20, 8, 34 ] 
    ]
    */
```
- lotto.spec.js
```javascript
    test('buy ticket at shop', () => {
        const ticket = shop.buyTicket(5000);
        expect(ticket.showTickets().length).toBe(5);
    });
    
    test('create random Lotto numbers ', () => {
    	expect(shop.createRandomNumbers().length).toBe(6);
    });
```

- lotto.js

```javascript 
    const lotto = (function() {
        const numbers = Symbol('numbers');
        return ({
        [numbers]: [],
        inputNumbers(inputNumbers) {
            this[numbers] = inputNumbers;
        },
        create: function (inputNumbers) {
            const lotto = Object.assign({}, this);
            lotto.inputNumbers(inputNumbers);
            return lotto;
        },
        showNumber() {
            return this[numbers];
        }
    });
    })();
    
    const shop = ( function () {
        const lottos = Symbol('lottos');
        return ({
        [lottos]: [],
        buyTicket(money) {
            if(money < 1000) {
                throw Error('1000원 이상의 금액만 가능합니다.');
            }
            const count = money / 1000;
            for(let i=0; i<count; i++) {
                this[lottos].push(lotto.create(this.createRandomNumbers()));
            }
        },
        createRandomNumbers() {
            const numbers = [];
            for(let i=1; i<=45; i++) {
                numbers.push(i);
            }
            numbers.sort(()=> Math.random() - 0.5);
            return numbers.slice(0,7);
        },
        showTickets() {
            console.log(this[lottos].map( a => a.showNumber() ));
            return this[lottos];
        }
    });
    })();
    
    module.exports = { lotto, shop };
    const shop = ( function () {
        const lottos = Symbol('lottos');
        return ({
        [lottos]: [],
        buyTicket(money) {
            if(money < 1000) {
                throw Error('1000원 이상의 금액만 가능합니다.');
            }
            const count = money / 1000;
            for(let i=0; i<count; i++) {
            this[lottos].push(lotto.create(this.createRandomNumbers()));
            }
        },
        createRandomNumbers() {
            const numbers = [];
            for(let i=1; i<=45; i++) {
                numbers.push(i);
            }
            numbers.sort(()=> Math.random() - 0.5);
            return numbers.slice(0,7);
        },
        showTickets() {
            console.log(this[lottos].map( a => a.showNumber() ));
            return this[lottos];
        }
    });
    })();
    
    module.exports = { lotto, shop };
```


![image.png](https://images.velog.io/post-images/yesdoing/509b27f0-e59a-11e8-864a-e9a6fea11f86/image.png)


# 💩 이 글을 마치며...

아직 Jest를 Unit test로 밖에 사용하지 못하였지만 Jest에는 Snapshot이라는 기능을 사용하여 React UI Test를 지원합니다. 다음 글에는 이 UI test와 관련된 예제로 글을 준비해 보겠습니다.