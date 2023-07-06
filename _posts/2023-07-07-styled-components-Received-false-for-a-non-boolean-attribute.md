---
title: styled-components - Received `false` for a non-boolean attribute 에러 해결하기
date: 2023-07-07 02:48:00 +09:00
categories: [styled-components, error]
tags: [styled-components, error] # TAG names should always be lowercase
---

> styled-components를 사용하는데 콘솔에 경고가 떴다.
> ![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/719b1b88-9f19-402b-898b-14187eec01f6)

내 코드는 다음과 같다.

```jsx
const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
    color: ${(props) => (props.checkvalid ? 'red' : 'black')};
  }
`

//JSX
<FormControl checkvalid={!isValid}>
    <label>Label</label>
    <input type="text" />
</FormControl>
```

경고 내용을 보면 내가 보낸 props가 DOM으로 보내지는 것 처럼 보이고, 이는 React 콘솔 오류를 발생시킬 수 있다고 한다.<br/>
마지막 부분에 보면 다음과 같은 내용이 있다. `consider using transient props (`$` prefix for automatic filtering.)`<br/>
여기서 말하는 <a href='https://styled-components.com/docs/api#transient-props' target="_blank">transient props</a>는 스타일이 지정되어있는 컴포넌트에서 사용해야 할 props가 DOM 요소로 렌더링되는 걸 방지할 수 있다고 한다.

- transient props로 해결한 코드

```jsx
const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
    color: ${(props) => (props.$checkvalid ? 'red' : 'black')};
  }
`

//JSX
<FormControl $checkvalid={!isValid}>
    <label>Label</label>
    <input type="text" />
</FormControl>
```

사용방법 : prop 이름 앞에 `$`를 붙인다. 이때 JSX코드에 있는 컴포넌트의 prop앞에도 붙여야 한다.

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
