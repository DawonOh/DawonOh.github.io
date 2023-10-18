---
title: Redux Toolkit
date: 2023-10-19 04:09:00 +09:00
categories: [Redux, React, Redux-Toolkit]
tags: [redux, react, redux-toolkit] # TAG names should always be lowercase
image:
  path: https://github.com/DawonOh/DawonOh.github.io/assets/89020079/1980855c-a5a5-441a-a67c-d4cf85cf3004
  alt: Redux Toolkit
---

> 노마드코더 - 초보자를 위한 리덕스 101을 듣고 정리한 내용입니다.

## Redux Toolkit이란

- Redux를 더 간단하게 사용할 수 있게 하는 라이브러리
- 기존 Redux의 단점들을 보완하였다.

  - store를 설정하는데 너무 복잡하다!
  - 보일러 플레이트 코드(boilerplate code)가 많다!
    - boilerplate code : 최소한의 변경으로 여러곳에서 재사용되며, 반복적으로 비슷한 형태를 띄는 코드 (ex : Creact React App)
  - 패키지를 많이 설치해야 한다!

## Redux Toolkit 설치

- npm : `npm install @reduxjs/toolkit`
- yarn : `yarn add @reduxjs/toolkit`

## createAction

- Redux의 action type과 action creators를 정의할 때 도움을 주는 함수

```jsx
function createAction(type, prepareAction?);
```

- 기존 React-Redux를 사용한 코드

```jsx
const ADD = "ADD";
const DELETE = "DELETE";

export const addToDo = (text) => {
  return { type: ADD, text };
};

export const deleteToDo = (id) => {
  return { type: DELETE, id: parseInt(id) };
};
```

- createAction()을 사용한 코드

```jsx
import { createAction } from "@reduxjs/toolkit";

const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");
```

- addToDo는 함수이며, 실행한 결과를 console에 출력해보면 다음과 같다.
- addToDo에 text를 인자로 전달했기 때문에 addToDo의 payload는 text가 된다.

```jsx
const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      // action에 뭘 보내든 간에 payload와 함께 보내진다.
      console.log(action);
      return [{ text: action.payload, id: Date.now() }, ...state];
    case deleteToDo.type:
      console.log(action);
      return state.filter((toDo) => toDo.id !== action.payload);
    default:
      return state;
  }
};
```

![console.log 결과](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/ec09ed56-0e83-45b3-8b0e-3ffa00a1f05e)
_console.log 결과_

---

## createReducer

- createReducer를 사용하면 state를 mutate하기 쉽게 만들어준다.
  - 기존의 방법은 state를 mutate한게 아니라 새로 state를 생성했다.
  - 첫 번째 인자 : reducer가 처음 호출될 때 사용되는 state의 초기값
  - 두 번째 인자 : `builder Callback`함수(`(builder:Builder) => void`)

### Builder Callback Notation

- `builder` 객체를 argument로 사용한다.
- `addCase`, `addMatcher`, `addDefaultCase` 함수를 제공하며, `reducer가` 처리할 `action`을 정의할 수 있다.

{: .prompt-info }

> 이외에도 `Map Object`를 사용하는 방법이 있다. 코드의 길이는 조금 짧지만, 자바스크립트에서만 동작하며 타입스크립트와 대부분의 IDE에서는 동작하지 않으므로 builder callback을 사용하는 것을 더 권장한다.

---

- <span style='font-weight: 700'>builder.addCase</span>
  - reducer가 처리할 하나의 case를 추가한다. 반드시 builder.addMatcher나 builder.addDefaultCase 전에 호출해야 한다.
  - 첫 번째 인자 : actionCreator 또는 string으로 된 action
  - 두 번째 인자 : state 변경 관련 로직

```jsx
const reducer = createReducer([], (builder) => {
  builder
    .addCase(addToDo, (state, action) => {
      // return하지 않았다..!
      state.push({ text: action.payload, id: Date.now() });
    })
    .addCase(deleteToDo, (state, action) => {
      return state.filter((toDo) => toDo.id !== action.payload);
    });
});
```

- 첫 번째 addCase에서 새로운 state를 생성해 return하지 않고 바로 state를 변경했다.<br/>
  → redux toolkit은 `immer`를 기반으로 돌아가는데, redux toolkit은 내가 state에 뭔가 추가하고 싶다는 걸 알기 때문에 뒤에서 redux toolkit이 return 작업을 한다.<br/>
  ⇒ <span style='background-color: #FEBEBE; color: #fff'>결과적으로 state가 mutate되는 것이 아니라고 한다..!</span> <br/>
  (immer : 현재 상태를 변경하여 다음 불변 상태를 만든다. - <a href='https://immerjs.github.io/immer/' target="\_blank">https://immerjs.github.io/immer/</a> )
- 어떤 걸 return할 때는 반드시 새 state를 만들어서 return해야 한다.

---

- <span style='font-weight: 700'>builder.addMatcher</span>
  - 인자로 받은 조건에 따라 state 로직을 처리한다.

```jsx
import { createAction, createReducer } from "@reduxjs/toolkit";

const initialState = {};
const resetAction = createAction("reset-tracked-loading-state");

// 인자로 받은 action이 끝에 '/pending'으로 끝난다면 true, 아니면 false를 return
function isPendingAction(action) {
  return action.type.endsWith("/pending");
}

const reducer = createReducer(initialState, (builder) => {
  builder
    // state 초기화
    .addCase(resetAction, () => initialState)
    // addMatcher는 isPendingAction의 return값이 true라면 두 번째 인자로 받은 함수를 실행한다.
    .addMatcher(isPendingAction, (state, action) => {
      state[action.meta.requestId] = "pending";
    })
    // action이 '/rejected'로 끝나는 경우에만 두 번째 인자로 받은 함수 실행
    .addMatcher(
      (action) => action.type.endsWith("/rejected"),
      (state, action) => {
        state[action.meta.requestId] = "rejected";
      }
    )
    // action이 '/fulfilled'로 끝나는 경우에만 두 번째 인자로 받은 함수 실행
    .addMatcher(
      (action) => action.type.endsWith("/fulfilled"),
      (state, action) => {
        state[action.meta.requestId] = "fulfilled";
      }
    );
});
```

---

- <span style='font-weight: 700'>builder.addDefaultCase</span>
  - 앞서 실행한 addCase와 addMatcher에 해당하지 않는 action을 실행한다.
  - 인자로 state를 처리할 로직을 가진 함수를 받는다.

```jsx
import { createReducer } from "@reduxjs/toolkit";
const initialState = { otherActions: 0 };
const reducer = createReducer(initialState, (builder) => {
  builder
    // .addCase(...)
    // .addMatcher(...)
    .addDefaultCase((state, action) => {
      state.otherActions++;
    });
});
```

---

## createSlice

- action과 reducer를 한 번에 생성할 수 있다!
- `slice` : 기능 별 작은 store
- 객체를 인자로 받으며, 객체 구조는 다음과 같다.
  - `name` : action 이름을 지정한다. → createSlice가 action 이름을 지정하는데 사용되는 고정값이다.
  - `initialState` : state 초기값
  - `reducers` : redux에서 switch문으로 만들었던 case들을 만든다.
    - 객체의 key값은 Redux DevTools 확장 프로그램에서 표시된다.

```jsx
const toDos = createSlice({
  name: "toDosReducer",
  initialState: [],
  reducers: {
    add: (state, action) => {
      state.push({ text: action.payload, id: Date.now() });
    }
  },
  remove: (state, action) => {
    return state.filter((toDo) => toDo.id !== action.payload);
  }
});

console.log(toDos.actions);
```

![console.log 결과 이미지](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/bdf84b01-8ae7-48de-8da3-5c1f0dc87309)
_console.log(toDos.actions)_

- toDos의 actions가 reducers에서 선언한 함수들인 것을 확인할 수 있다.

---

## configureStore

- `createSlice`로 만든 `slice`를 모아서 store를 만들 때 사용한다.
- 객체를 전달해야 한다.
- 기존 `createStore`의 지원이 끝나면서 `configureStore`을 사용하라고 한다.
  - 하나의 reducer에 slice들을 합치지 않아도 된다. (`combineReducers` 사용 X)
  - thunk가 적용되어있다. (`applyMiddleware` 사용 X)
  - Redux DevTools를 기본으로 지원한다.
- 필수값 : `reducer` (s 안붙는다!)

```jsx
// store.js
const store = configureStore({ reducer: toDos.reducer });

export const { add, remove } = toDos.actions;

// Home.js
import { add } from "../store";

const dispatch = useDispatch();

const onSubmit = (event) => {
  // ...
  dispatch(add(text));
};
```

- 객체 디스트럭쳐링을 사용하지 않는 경우

```jsx
// store.js
export const toDos = createSlice({
  // ...
});

// Home.js
import { toDos } from "../store";

const dispatch = useDispatch();

const onSubmit = (event) => {
  // ...
  dispatch(toDos.actions.add(text));
};
```

---

출처) <br/>
<a href='https://nomadcoders.co/redux-for-beginners/lobby' target="\_blank">노마드코더 - 초보자를 위한 리덕스 101</a><br/>
<a href='https://redux-toolkit.js.org/' target="\_blank">Redux Toolkit 공식문서</a><br/>
<a href='https://www.youtube.com/watch?v=UKnLwVm9suY&t=173s' target="\_blank">코딩알려주는누나 - 아직도 옛날 리덕스 쓴다고..? 옛날 리덕스를 최신 리덕스 Toolkit으로 바꿔보자!(Youtube)</a><br/>
<a href='https://www.youtube.com/watch?v=9wrHxqI6zuM' target="\_blank">생활코딩 - Redux toolkit(Youtube)</a>

---

<div class='giscus'></div>
<script src="https://giscus.app/client.js"
        data-repo="DawonOh/DawonOh.github.io"
        data-repo-id="R_kgDOJiw-zQ"
        data-category="Comments"
        data-category-id="DIC_kwDOJiw-zc4CWhdL"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="ko"
        crossorigin="anonymous"
        async>
</script>
