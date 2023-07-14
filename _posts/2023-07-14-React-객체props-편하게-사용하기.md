---
title: React - props를 객체로 받아서 편하게 사용하기
date: 2023-07-14 22:35:00 +09:00
categories: [React]
tags: [react] # TAG names should always be lowercase
---

> (제목을 뭐라고 해야 할 지 잘 모르겠다.) 부모 컴포넌트에서 자식 컴포넌트에 props를 넘겼을 때 자식 컴포넌트에서 JSX요소에 하나 하나 적기에는 너무 많을 때 사용하면 좋은 방법 같다!

```jsx
// 부모 컴포넌트
<Input
  input={% raw %}{{
    id: "amount",
    type: "number",
    min: "1",
    max: "5",
    step: "1",
    defaultValue: "1"
  }}{% endraw %}
/>;

// 자식 컴포넌트
const Input = (props) => {
  return <input {...props.input} />;
};
```

- 부모 컴포넌트에서 props를 객체로 보낸다.
- 자식 컴포넌트는 spread 연산자를 사용한다. (위의 코드는 아래 코드와 같다.)

```jsx
<input id="amount" type="number" min="1" max="5" step="1" defaultValue="1" />
```

---

출처) <a href='https://www.udemy.com/course/best-react/' target='_blank'>Udemy - React 완벽 가이드 with Redux, Next.js, TypeScript</a>

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
