---
layout: post
title: "Usage with TypeScript"
categories: posting
---


원문 링크: https://redux.js.org/recipes/usage-with-typescript

타입스크립트는 자바스크립트의 상위집합이다. 이것은 최근에 유명해졌다. 어플리케이션에서, 그것이 가져오는 이익 때문에. 만약 당신이 타입스크립트가 새롭다면 진행하기전에 친해지길 강력히 추천한다. 이 [문서](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)로 친해질 수 있다.

타입스크립트는 리덕스 어플리케이션에게 다음의 이익을 가져오기 위한 잠재력을 가진다. 

1. 리듀서, 상태, 액션 생성자에 대한 Type safety
2. 쉬운 타입 코드 리펙토링
3. 팀 환경에서 우수한 개발자 경험

# A Practical Example

우리는 단순화한 챗 어플리케이션을 경험할 것이다. static typing을 포함한 있을 수 있는 접근을 입증하기 위해. 이 챗 어플리케이션은 두 개의 리듀서를 가질 것이다. chat reducer는 chat history를 저장하는 것에 집중할 것 이고, system reducer는 session information을 저장하는 것에 집중할 것이다. 

완전한 소스 코드는 [여기서](https://codesandbox.io/s/w02m7jm3q7) 이용 가능 하다. 기억해라. 이 예제를 조사하는 것은 너 자신에게 타입스크립트를 사용함으로써 오는 몇몇 장점을 경험 할 것이다. 

# Type Checking State

각각 분리된 상태의 타입을 추가하는 것은 좋은 예다. 시작하기 좋은. 다른 타입들이 연달아 오지 않는다면. 이 예제에서 우리는 시작할 것이다. 분리된 상태의 chat reducer를 묘사함으로 써

    // src/store/chat/types.ts
    
    export interface Message {
      user: string
      message: string
      timestamp: number
    }
    
    export interface ChatState {
      messages: Message[]
    }

그리고 system reducer에 대해서도  똑같은 작업을 해라.

    // src/store/system/types.ts
    
    export interface SystemState {
      loggedIn: boolean
      session: string
      userName: string
    }

기억해라. 우리는 이 interface 들을 노출시켰다. reducers 과 ation creators 에서 재사용하기 위해.

# Type Checking Actions & Action Creators

우리는 문자열 리터럴을 사용할 것이다. 그리고 `typeof` 를 사용할 것이다. 우리의 액션 상수들과 타입 추론을 선언하기 위해서.

기억해라. 우리는 이곳에 거래를 만들 것이다. 우리가 분리된 파일 안에서 우리의 타입들을 선언할 때. 

분리된 파일 안에서 분리된 우리의 타입들을 위한 교환으로, 우리는 계속해서 유지할거다. 우리의 다른 파일들을 보다 저 목적에 초점을 맞추기 위해서.

이 교환은 코드베이스의 정비성을 향상 시킬 수 있는 반면에, 니가 맞는 것을 본다면 너의 프로젝트에 구성하는 것은  완벽하게 괜찮다.

Chat Action Constants & Shape: 

    // src/store/chat/types.ts
    export const SEND_MESSAGE = 'SEND_MESSAGE'
    export const DELETE_MESSAGE = 'DELETE_MESSAGE'
    
    interface SendMessageAction {
      type: typeof SEND_MESSAGE
      payload: Message
    }
    
    interface DeleteMessageAction {
      type: typeof DELETE_MESSAGE
      meta: {
        timestamp: number
      }
    }
    
    export type ChatActionTypes = SendMessageAction | DeleteMessageAction

기억해라. 우리는 이곳에서 타입스크립트의 Union Type을 사용할 수 있다. 가능한 action 들을 표현하기 위해.

이러한 선언 되어진 타입 들을 가지고 우리는 chat의 action creator들에 대한 또한 타입 체크를 할 수 있다. 이 예제에서 우리는 타입스크립트 interface의 장점을 취할 것이다:

    // src/store/chat/actions.ts
    
    import { Message, SEND_MESSAGE, DELETE_MESSAGE } from './types'
    
    // TypeScript infers that this function is returning SendMessageAction
    export function sendMessage(newMessage: Message) {
      return {
        type: SEND_MESSAGE,
        payload: newMessage
      }
    }
    
    // TypeScript infers that this function is returning DeleteMessageAction
    export function deleteMessage(timestamp: number) {
      return {
        type: DELETE_MESSAGE,
        meta: {
          timestamp
        }
      }
    }

System Action Constants & Shape:

    // src/store/system/types.ts
    export const UPDATE_SESSION = 'UPDATE_SESSION'
    
    interface UpdateSessionAction {
      type: typeof UPDATE_SESSION
      payload: SystemState
    }
    
    export type SystemActionTypes = UpdateSessionAction

이 타입들을 가진 우리는 system action creator 들에 타입 체크를 할 수 있다. 

# Type Checking Reducers

Reducers 는 단지 순수 함수이다. 이전의 상태와 액션 그리고 반환되는 다음 상태를 가지는, 

이 예제에서, 우리는 명확하게 액션의 타입을 선언한다. 이  리듀서가 그것이 반환 해야 하는 것과 마찬가지로 받을 수 있는. 

이런 장점을 가진 타입스크립트는 줄 것이다. 우리의 액션과 상태의 속성들에 부유한 인텔리센스를. 게다가 우리는 또한 ChatState 가 반환되지 않는 것에 대해 오류를 얻을 것이다. 

Type checked chat reducer:

    // src/store/chat/reducers.ts
    
    import {
      ChatState,
      ChatActions,
      ChatActionTypes,
      SEND_MESSAGE,
      DELETE_MESSAGE
    } from './types'
    
    const initialState: ChatState = {
      messages: []
    }
    
    export function chatReducer(
      state = initialState,
      action: ChatActionTypes
    ): ChatState {
      switch (action.type) {
        case SEND_MESSAGE:
          return {
            messages: [...state.messages, action.payload]
          }
        case DELETE_MESSAGE:
          return {
            messages: state.messages.filter(
              message => message.timestamp !== action.meta.timestamp
            )
          }
        default:
          return state
      }
    }

지금 우리는 루트 reducer 함수를 생성해야한다. 보통 `combineReducers` 를 사용하여 동작하는. 기억해라. 우리는 App State를 위한 새로운 인터페이스를 명확히 선언하지 않는다. 우리는 `ReturnType` 을 사용할 수 있다. 상태의 모양을 추론하기 위해 `rootReducer`로 부터

    // src/store/index.ts
    
    import { systemReducer } from './system/reducers'
    import { chatReducer } from './chat/reducers'
    
    const rootReducer = combineReducers({
      system: systemReducer,
      chat: chatReducer
    })
    
    export type AppState = ReturnType<typeof rootReducer>

# Usage with React Redux

React Redux는 redux 그 자체로부터 분리된 라이브러리인 반면에, 그것은 react에서 흔하게 사용된다. 이런 이유로, 우리는 겪을 것이다. 어떻게 React Redux가 타입스크립트에서 동작하는지. 이 세션에서 이전에 사용됬던 같은 예제를 사용함으로써.

> 기억해라: React Redux는 그 자체에 type checking을 가지지 않는다. 당신은 설치해야한다. `@types/react-redux`  를.

우리는 지금  `mapStateToProps` 를 받아 들이는 파라메터를 위한 type checking을 추가할 것이다. 운이 좋게도, 우리는 이미 선언된 걸 가졌다.  스토어에 정의되어진 타입으로 부터 보여질 것이다. `rootReducer` 로 부터 추론되어지는.

    // src/App.tsx
    
    import { AppState } from './store'
    
    const mapStateToProps = (state: AppState) => ({
      system: state.system,
      chat: state.chat
    })

이 예제 안에서 우리는 선언했다. `mapStateToProps` 안에 두개의 다른 속성들을. 이 파라미터에 타입 체크를 위해, 우리를 interface를 생성할 것이다. 적절히 상태가 분리된.

    // src/App.tsx
    
    import { SystemState } from './store/system/types'
    
    import { ChatState } from './store/chat/types'
    
    interface AppProps {
      chat: ChatState
      system: SystemState
    }

이제 우리는 이 인터페이스를 사용할 수 있다. 적절한 컴포넌트가 받을 수 있는 Props를 기술하기 위해.

    // src/App.tsx
    
    class App extends React.Component<AppProps> {

이 컴포넌트에서 우리는 또한 action creator 들을 연결 할것이다.  컴포넌트의 props 안에서 이용하기 위해. AppProps 인터페이스와 같이, 우리는 강력한 `typeof` 의 특성을 사용할 것이다.  타입스크립트는 우리의 action creator 들이 이와 같다는 것을 알게하기 위해:

    // src/App.tsx
    
    import { SystemState } from './store/system/types'
    import { updateSession } from './store/system/actions'
    
    import { ChatState } from './store/chat/types'
    import { sendMessage } from './store/chat/actions'
    
    interface AppProps {
      sendMessage: typeof sendMessage
      updateSession: typeof updateSession
      chat: ChatState
      system: SystemState
    }

이런 만들어진 추가된 것들을 가지고 리덕스로 부터 오는 props는 지금 type checked 되었다. 

필요한 만큼 자유롭게 인터페이스를 확장해라. 부모컴포넌트로 부터 내려오는 추가적인 props들을 간주하기 위해

# Usage with Redux Thunk

Redux Thunk 는 흔히 사용되는 middleware다. 비동기 오케스트레이션을 위한. 편하게 이 [문서](https://github.com/reduxjs/redux-thunk)를 확인해라. thunk는 `dispatch` 와 `getState` 파라미터를 가지는 또 다른 함수를 반환하는 함수이다.  Redux Thunk는 `ThunkAction` 타입을 가졌다. 우리가 이와같이 유틸화 할 수 있게: 

    // src/thunks.ts
    
    import { Action } from 'redux'
    import { sendMessage } from './store/chat/actions'
    import { AppState } from './store'
    import { ThunkAction } from 'redux-thunk'
    
    export const thunkSendMessage = (
      message: string
    ): ThunkAction<void, AppState, null, Action<string>> => async dispatch => {
      const asyncResp = await exampleAPI()
      dispatch(
        sendMessage({
          message,
          user: asyncResp,
          timestamp: new Date().getTime()
        })
      )
    }
    
    function exampleAPI() {
      return Promise.resolve('Async Chat Bot')
    }

당신의 dispatch에서 액션 생성자들을 사용하는 것을 적극 추천한다. 당신이 이런 함수들의 타입 검사를 하기 위한 작업을 재 사용한 이후에

# Notes & Considerations

- 이 문서는 주로 리덕스의 입장에서 타입 검사를 한다. 목적을 증명하기 위해, 코드 샌드박스 예제는 또한 통합을 입증하기 위해 React Redux를 가진 React를 사용한다.
- 리덕스에서 타입 검사를 위해 여러 개의 접근법이 있다. 이것은 많은 접근법 중에 하나이다.
- 이 예제는 오직 이 접근을 보여줄 목적으로 보낸다. 다른 상급의 개념들이 의미하는 것은 간다하게 유지되는 것을 없애는 것이다. 만약 당신이 코드 스플리팅을 한다면 [이것](https://medium.com/@matthewgerstman/redux-with-code-splitting-and-type-checking-205195aded46)을 봐라.
- 이해해라. 타입스크립트가 교환하는 것에 대해 동작하는 것을. 이 교환이 당신의 어플리케이션에서 가치가 있을때 이해하는 것은 좋은 생각이다.