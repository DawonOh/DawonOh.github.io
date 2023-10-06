---
title: React Redux 기초
date: 2023-10-07 02:25:00 +09:00
categories: [Redux, React]
tags: [redux, react] # TAG names should always be lowercase
---

> 노마드코더 - 초보자를 위한 리덕스 101을 듣고 정리한 내용입니다.

{: .prompt-info }

> 공식문서에서 connect()보다 useSelector()와 useDispatch()를 사용하는 것을 권장하고 있다.<br/>
> 참고 ) <a href='https://react-redux.js.org/api/connect' target="\_blank">React Redux - connect()</a>

# React Redux로 ToDo 앱 만들어보기

## 0. 개발 환경 세팅

- react-redux 설치
  - npm : `npm install react-redux`
  - yarn : `yarn add react-redux`

## 1. store 연결하기

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./components/App";
import { Provider } from "react-redux";

// store 생성한 파일에서 import
import store from "./store";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  // Provider로 감싼다.
  <Provider store={store}>
    <App />
  </Provider>
);
```

## 2. store에서 데이터 가져오기 및 변경하기

- ToDo 추가 및 삭제 기능 구현
  ![todolist](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/833d3662-5513-49b1-acbc-6c8db4dc0439)

### connect() - ToDo 추가 기능

- <b>컴포넌트를 store에 연결</b>한다.
- store에 연결할 때는 export할 때 `connect()`를 사용한다.
  - `connect()`는 2개의 인자를 받는다 : `mapStateToProps`, `mapDispatchToProps`<br/>
    → 두 인자는 모두 함수이며, 함수 명은 다른 걸로 지어도 되지만 보통 위의 이름으로 짓는다.
- `mapStateToProps` : state를 컴포넌트에 props로 전달한다.
  - 2개의 인자를 받는다.
    - state : store에서 가져온 state
    - ownProps : 현재 위치한 컴포넌트의 props(생략 가능)
- `mapDispatchToProps` : state값을 변경한다.
  - 인자로 `dispatch`함수와 `ownProps`를 받는다.

```jsx
// Home.js
import { connect } from "react-redux";
import { actionCreators } from "../store";

const mapStateToProps = (state, ownProps) => {
  return { toDos: state };
};

const mapDispatchToProps = (dispatch) => {
  return {
    // 여기서 actionCreators는 코드 내에서 addToDo 함수 명이 겹치는 것을 막기 위해서
    // store.js에 export하였다.
    // 그냥 store.js에서 바로 addToDo함수를 export 해서 사용해도 된다.
    addTodo: (text) => dispatch(actionCreators.addToDo(text))
  };
};

// 만약 mapStateToProps가 필요없다면 connect(null, mapDispatchToProps)
export default connect(mapStateToProps, mapDispatchToProps)(Home);

// 참고 ) store.js에 추가한 actionCreators 코드
export const actionCreators = {
  addToDo,
  deleteToDo
};
```

### connect - ToDo 삭제 기능

- 삭제 시에는 id를 비교해 특정 ToDo만 삭제한다. 그렇기 때문에 id를 deleteToDo함수의 인자로 전달해야 한다.
  → ownProps는 현재 컴포넌트의 props이므로 props로 받은 id를 사용할 수 있다!
- `mapStateToProps`는 필요없고 `mapDispatchToProps`만 사용한다면 connect()의 첫번째 인자에 null을 전달하면 된다.

```jsx
import { deleteToDo } from "../store";

const ToDo = ({ text, id }) => {
  return (
    <li>
      {text}
      <button onClick={onBtnClick}>DEL</button>
    </li>
  );
};

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    // 여기서 ownProps.id를 deleteToDo함수의 인자로 전달한다.
    onBtnClick: () => dispatch(actionCreators.deleteToDo(ownProps.id))
  };
};

// 첫 번째 인자로 null 전달
export default connect(null, mapDispatchToProps)(ToDO);
```

### useSelector, useDispatch - ToDo 추가 기능

- `useSelector` : getState처럼 store에서 정보를 가져온다. → `mapStateToProps` 대체
- `useDispatch` : Redux store에서 dispatch 함수에 대한 참조를 반환한다. action을 dispatch하는데 사용할 수 있다. → `mapDispatchToProps` 대체

```jsx
// Home.js
import { useDispatch, useSelector } from "react-redux";
import { addToDo } from "../store";

const toDo = useSelector((state) => state);

const dispatch = useDispatch();

const onSubmit = (event) => {
  event.preventDefault();
  dispatch(addToDo(text));
  setText("");
};
```

### useSelector, useDispatch - ToDo 삭제 기능

- 공부하면서 느낀 점 : 전체적으로 connect보다 코드가 간결하고 이해하기가 쉬운 것 같다.

```jsx
import React from "react";
import { useDispatch } from "react-redux";
import { deleteToDo } from "../store";

const ToDo = ({ text, id }) => {
  const dispatch = useDispatch();

  const onBtnClick = () => {
    dispatch(deleteToDo(id));
  };

  return (
    <li>
      <button onClick={onBtnClick}>DEL</button>
    </li>
  );
};

export default ToDo;
```

🙂전체 코드 보기 - <a href='https://github.com/DawonOh/vanilla-redux/tree/4.react_redux' target="\_blank">GitHub로 이동</a>

---

출처) <br/>
<a href='https://nomadcoders.co/redux-for-beginners/lobby' target="\_blank">노마드코더 - 초보자를 위한 리덕스 101</a><br/>
<a href='https://react-redux.js.org/api/hooks' target="\_blank">React Redux 공식문서 - Hooks</a>

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
