---
title: react-router-dom 라우트 정의하기
date: 2023-11-22 12:59:00 +09:00
categories: [React, react-router-dom]
tags: [react, react-router-dom] # TAG names should always be lowercase
---

> 프로젝트 리팩토링을 진행하면서 보니 <a href='https://reactrouter.com/en/main/start/overview' target="\_blank">react-router-dom v6.4</a> 이상에서 권장하는 방법이 아닌 다른 방법으로 router를 관리하고 있었다. 이번에 리팩토링 하면서 새로 알게 된 방법을 정리해보려 한다.

## createBrowserRouter

- 모든 React Router 웹 프로젝트에 권장되는 방법이다.
- 인자로 routing될 경로와 컴포넌트를 요소로 가지는 객체를 전달한다.
- 경로가 여러개인 경우에는 배열로 인자를 전달한다.

## routerProvider

- createBrowserRouter로 생성한 객체를 routerProvider컴포넌트에 router 속성으로 넣어 랜더링한다.

```jsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

const router = createBrowserRouter([
  { path: "/", element: <HomePage /> },
  { path: "/products", element: <ProductsPage /> }
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

## createRoutesFromElements

- 객체가 아닌 JSX 코드로 라우팅을 설정할 수 있다.
- `createRoutesFromElements`로 JSX코드를 만들어 인자로 전달한다.
  - 이때 `Router`를 `import`해서 태그로 사용한다.
  - `createRoutesFromElements`로 만든 변수를 `createBrowserRouter`의 인자에 전달한다.
  - 최종적으로 `RouterProvider`에 `router`속성으로 만든 `router`를 전달하면 된다.

```jsx
import {
  createBrowserRouter,
  createRoutesFromElements,
  RouterProvider,
  Route
} from "react-router-dom";

const routeDefinitions = createRoutesFromElements(
  <Route>
    <Route path="/" element={<HomePage />} />
    <Route path="/products" element={<ProductsPage />} />
  </Route>
);

const router = createBrowserRouter(routeDefinitions);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

{: .prompt-info }

> `createBrowserRouter`와 `createRoutesFromElements`중 편한 방법으로 사용하면 된다.

---

출처)<br/>
<a href='https://www.udemy.com/course/best-react/' target="\_blank">udemy - React 완벽 가이드</a><br/>
<a href='https://reactrouter.com/en/main' target="\_blank">React Router 공식문서</a>

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
