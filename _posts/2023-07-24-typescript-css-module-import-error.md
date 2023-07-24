---
title: typescript 사용할 때 React에서 module.css파일 import 하기
date: 2023-07-25 02:27:00 +09:00
categories: [React, TypeScript, CSS]
tags: [react, typescript, css] # TAG names should always be lowercase
---

> React에서 tsx 파일에 module.css 파일을 import하려고 했더니 에러가 발생했다.

## 에러 분석하기

```tsx
import styles from "./index.module.css";
```

"message": "Cannot find module './Button.module.css' or its corresponding type declarations."<br/>
해당 모듈을 찾을 수 없거나 해당하는 타입 선언이 없다는 뜻인데, 이럴 때는 global.d.ts파일에 모듈을 선언하면 된다.

{: .prompt-tip }

> **d.ts 파일이란**<br/>
> 타입스크립트의 타입을 정의하고 저장하는 파일(Declaration File)
> ⇒ global.d.ts 파일을 만들어서 전역적으로 사용하게 될 타입을 선언한다.

## 해결하기

- src 폴더에 global.d.ts 파일을 생성하고 아래와 같이 작성한다.

```tsx
declare module "*.css" {
  const content: { [className: string]: string };
  export = content;
}
```

- `*.css` 모듈의 타입인 객체는 'className'을 key값으로 가지며, value는 string 타입이다.
- 선언한 모듈의 타입을 export해서 다른 파일에서 모듈을 사용할 수 있도록 한다.

---

출처) <a href='https://velog.io/@drrobot409/React-TypeScript-module.scss-import' target='_blank'>[TypeScript] "module.scss" import</a>

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
