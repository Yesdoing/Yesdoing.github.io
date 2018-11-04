---
layout: post
title: "Iterator & Generator"
categories: posting
---

![](https://www.dcrainmaker.com/images/2018/04/WeekInReview_thumb_thumb_thumb_thumb_thumb-720x479.jpg)

# 🐓 Weekly I Learned

- [[bsidesoft] Generator #1](http://www.bsidesoft.com/?p=2053)
- [코드스피츠 77 - ES6+ 기초편 3회차](https://youtu.be/xTaCosid1-k)
- [Iterable protocol](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols)

# 🦏 What is the Iterator

- **In JavaScript an iterator is an object which defines a sequence and potentially a return value upon its termination. by [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Iterators_and_Generators#%EB%B0%98%EB%B3%B5%EC%9E%90)**

# 🐊 Iterator interface

1. next라는 키를 갖고
2. 값으로 인자를 받지 않고 IteratorResultObject를 반환하는 함수가 온다.
3. IteratorResultObject는 value와 done이라는 키를 갖고 있다.
4. 이 중 done은 계속 반복할수 있을지 없을지에 따라 Boolean값을 반환한다. 

위 4가지 조건을 만족하면 Iterator 객체라고 봅니다. 
```
    {
    	data:[1,2,3,4],
    	next() {
    		return {
    			done: this.data.length == 0,
    			value: this.data.pop()
    		};
    	}
    }
```
# 🦍 Iterable Interface

1. Symbol.iterator라는 키를 갖고
2. 이 키는 값으로 인자를 받지 않고 Iterator Object를 반환하는 함수를 가집니다.
```
    {
        	arr: [1,2,3,4],
        	[Symbol.iterator](){return this;},
        	next() {
        		return {
        			done: this.arr.length == 0,
        			value: this.arr.pop()
        		};
        	}
    };
```
# 🐿 Why use the Iterator?

- for나 while문 같은 반복문은 '문'이기 때문에 한번 반복이 끝나고 나면 두번 다시 사용할 수 없다.
- Iterator은 반복문을 '식'이나 '값'으로 계속 유지 시켜주기 위한 도구(반복 전용 객체)
- 위 예제의 이터레이터는 반복 자체를 하지는 않지만 외부에서 반복을 하려고 할 때 반복에 필요한 조건과 실행을 미리 준비해 놓고 그것의 복사본을 반환한다.
- 즉, 반복 행위와 반복을 위한 준비를 분리하여, 미리 반복에 대한 준비를 해두고 필요할 때 필요한 만큼 반복 시키는 것을 재현할 수 있다.
- 반복 행위는 next() 호출로 이루어 진다.
- 사용자 반복 처리기 Example
```
    const loop = (iter, f) => {
    	//Iterable이라면 Iterator를 얻음 
    	if(typeof iter[Symbol.iterator] == 'function') {
    		iter = iter[Symbol.iterator]();	
    	}else return;
    
    	//IteratorObject가 아니라면 건너뜀 
    	
    	if(typeof iter.next != 'function') return;
    
    	do {
    		const v = iter.next();
    		if(v.done) return; // 종료 처리
    		f(v.value); // 현재 값을 전달함. 반복시 마다 처리해야하는 함수 호출
    	}while(true) // 반복기 정의 이 while문은 무한루프로 그냥 반복이라는 행위만 한다. 
    };
    
    const iter = {
    	arr: [1,2,3,4],
    	[Symbol.iterator](){return this;},
    	next() {
    		return {
    			done: this.arr.length == 0,
    			value: this.arr.pop()
    		};
    	}
    };
    
    loop(iter, console.log);
    // 4, 3, 2, 1
```
# 🐂 Can use Es6+ operator When use the Iterator
- Array destructuring
- Spread operator
- Rest operator
- For ~ of

# 🦑 What is the Generator?

- The Generator object is returned by a generator function and it conforms to both the iterable protocol and the iterator protocol. by [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- 쉽게 말하자면 이터레이터를 만들기 위한 코드가 너무 많으니까 쉽게 만들기 위해서 사용한다.
```
    // Iterator
    const N2 = class {
    	constructor(max) {
    		this.max = max;
    	}
    	[Symbol.interator](){
    		let cursor = 0, max=this.max;
    		return {
    			done: false,
    			next() {
    				if(cursor > max) {
    					this.done = true;
    				} else {
    					this.value = cursor * cursor;
    					cursor++;
    				}
    				return this;
    				}
    			};
    		}
    };
    
    // Generator
    const generator = function* (max) {
    	let cursor = 0;
    	while(cursor < max) {
    		yield cursor * cursor;
    		cursor++;
    	}
    };
```
# 🐛 How to use Generator?

- function* (){} 인 Generator 생성 리터럴을 이용해서 생성한다.
- Generator는 생성 리터럴에 정의된 코드가 실행되므로 지역변수와 인자를 가지고 일반적인 제어흐름을 따른다.
- 함수에 인자와 지역변수를 사용해서 제어문을 사용하고있는 일반적인 동기화 흐름 플로우 구조를 따르고 있다. 유일하게 다른건 yield가 등장하고 있다.
- yield를 호출할때마다 Iterator의 next() 가 반환되는 것과 똑같은 효과를 볼 수 있다.
- Iterator와 다른 점은 yield를 만나면 함수가 그 자리에서 멈추고 Iterator의 result를 반환하며, Generator 객체가 next()를 호출해야 다시 멈춘 지점에서 동작한다.
- Generator에서 yield를 만나는 동안은 done: false를 유지하며, Generator를 빠져나갈 때 done: true가 된다.

# 🦕 마치며...

- 이 글의 모든 예제와 내용은 코드스피츠 youtube와 bsidesoft 사이트의 Generaotr 글에서 확인 할 수 있습니다.