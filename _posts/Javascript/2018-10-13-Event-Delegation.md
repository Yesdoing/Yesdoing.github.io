# 이벤트 위임(Event Delegation)의 구현

# 이 글을 쓰는 목적

- 최근 바닐라 스크립트로 개발을 하던 중 동적 엘리먼트의 이벤트 처리 부분을 엘리먼트 생성 시 각 버튼 이벤트를 추가했었는데, 스터디를 같이하는 개발자분께서 이런 동적 엘리먼트는 이벤트 위임 방식으로 구현할 수도 있다고 말씀하셨다. 평소에 이벤트 위임 패턴에 대해 지식으로만 알고 있었지만 실제로 구현해서 적용하지 않다 보니 어떻게 적용해야 하는지 몰라 직접 예제를 구현하며 공부한 것을 복습하고자 이 글을 적게 되었습니다.
- 이 글의 모든 예제는 제가 직접 구현한 것이므로 틀린 부분이 있을 수 있습니다.
- 이 글의 모든 예제는 [깃허브 저장소](https://github.com/Yesdoing/js_event_delegation)에 있습니다.

[https://codepen.io/YesDoing/full/NOvoqP](https://codepen.io/YesDoing/full/NOvoqP)

# 이벤트 위임이란?

- 이벤트 위임의 원리에 대한 설명은 다른 블로그에 잘 설명되어 있기에 어떻게 구현하는지에 대해서만 다루겠습니다.
- 이벤트 위임이란 쉽게 말해 동적으로 노드를 생성하고 삭제할 때 '각 노드에 대해 이벤트를 추가하지 않고, 상위 노드에서 하위 노드의 이벤트들을 제어하는 방식'입니다.

매번 블로그에서 문장으로만 읽고 지나가다 보니 실제 코드에 전혀 적용을 못하여 직접 구현해 보았습니다.

간단한 ToDoList 예제입니다.

# 이벤트 위임 코드 구현

- html 구현 부분

    <div class="container">
            <h1> 위임 패턴 예제</h1>
            <form class="todo-form">
                <input type="text" id="todoInput">
                <button type="submit">추가</button>
            </form>
            <div class="todo-list">
    					<!--
    						<div class="todo">
    				        <div class="todo-text">
    				            <span>${todo}</span>
    				        </div>
    				        <div class="todo-state">
    				            <div class="todo-button" data-action="onDone">done</div>
    				            <div class="todo-button" data-action="onRemove">remove</div>
    				            <div class="todo-button" data-action="onUpdate">update</div>
    				        </div>
    				    </div>
    					-->
            </div>
    </div>

- javascript 구현 부분

    import {todo} from '../data/todo.js';
    import {todoTemplate} from '../template/todoTemplate.js';
    
    export default class ToDoComponent {
        constructor(elem) {
            this._elem = elem;
        }
    
        onSave(text) {
            const $todoList = document.querySelector('.todo-list');
    
            $todoList.innerHTML = '';
            todo.push({todo: text });
            todo.map(x => {
                $todoList.insertAdjacentHTML('afterbegin', todoTemplate(x));
            });
        }
    
        onRemove() {
            alert('removing');
        }
    
        onDone() {
            alert('done!');
        }
    
        onUpdate() {
            alert('updating');
        }
    
        onEmit() {
            console.log('onEmit');
            this._elem.addEventListener('click', (e) => {
                const elemTarget = e.target;
                if ( elemTarget && elemTarget.dataset.action === "onDone" ) {
                    this.onDone();
                } else if ( elemTarget && elemTarget.dataset.action === "onRemove" ) {
                    this.onRemove();
                } else if ( elemTarget && elemTarget.dataset.action === "onUpdate" ) {
                    this.onUpdate();
                }
            });
        }
    }

- Form 태그에서 할 일을 입력받고 Submit 이벤트를 호출 시키면 .todo-list 요소의 자식으로 .todo 요소가 추가됩니다.
- 이때 .todo요소에 대해 이벤트를 추가하지 않고 부모 요소인 .todo-list에 click에 대한 이벤트를 추가하고 .todo-list 요소에서 클릭 이벤트의 콜백 함수에 분기 처리를 통해 각 버튼에 대한 처리를 등록할 수 있습니다.
- 만약 자식 요소가 자신의 자식 요소 이벤트를 가지고 있을 때는 정확한 이벤트 타깃을 찾기 위해 event.closest()를 사용할 수 있습니다. ([MDN 링크](https://developer.mozilla.org/en-US/docs/Web/API/Element/closest))

# 정리하며..

- 이벤트 위임은 부모 요소가 자식 요소의 이벤트 처리를 위임받는 것을 뜻한다고 저는 이해하였습니다.
- 이벤트 위임을 적용하였을 때 이벤트 처리가 안되어 있는 부분 (예제의 텍스트 부분)에 대해 클릭 이벤트가 발생할 시 불필요한 이벤트가 발생할 수 있습니다.
- 이벤트 위임을 적용할 지 안 할지는 개발자 본인의 선택이며 필수는 아닙니다.

## 참고 사이트

- 캡틴 판교님의 블로그 : [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%9C%84%EC%9E%84---event-delegation)
- DWB 블로그 : [How JavaScript Event Delegation Works](https://davidwalsh.name/event-delegate)
- [javascript.info](http://javascript.info) : [Event delegation](https://javascript.info/event-delegation)