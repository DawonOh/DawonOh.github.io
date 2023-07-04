---
title: Project setting - CRA, TypeScript, ESLint, Prettier, Lint-staged, Husky
date: 2023-07-05 00:28:00 +09:00
categories:
  [project, setting, CRA, TypeScript, ESLint, Prettier, Lint-staged, Husky]
tags: [project, setting, cra, typescript, eslint, prettier, lint-staged, husky] # TAG names should always be lowercase
---

> 미래의 나를 위해 남겨두는 프로젝트 세팅하기

## CRA로 TypeScript 프로젝트 세팅하기

`npx create-react-app my-app --template typescript`

## ESLint 설정

### ESLint란

자바스크립트 코드에서 설정에 위배되는 코드나 안티 패턴을 자동으로 검출하도록 도와주는 오픈소스

### 설치(npm 기준)

`npm install eslint --save-dev`

### 설정

`npm init @eslint/config` : `.eslintrc`파일이 자동으로 생성된다. 아래 이미지처럼 상황에 따라 선택하면 된다.<br/>
![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/3a031ffb-3e6a-48f9-bda6-d23c9890358a)<br/>
![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/14648b5d-472f-4a39-9661-38ffb1fc3429)<br/>
![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/7f1e940f-d9d4-4a63-a7a2-5f3a772b671f)

- package.json에 아래와 같이 설치된다.

```json
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^5.61.0",
    "eslint": "^8.44.0",
    "eslint-config-standard-with-typescript": "^36.0.0",
    "eslint-plugin-import": "^2.27.5",
    "eslint-plugin-n": "^15.7.0",
    "eslint-plugin-promise": "^6.1.1",
    "eslint-plugin-react": "^7.32.2",
    "typescript": "^5.1.6"
  }
```

- 기본 설정대로 만들어진 .eslintrc.json

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["standard-with-typescript", "plugin:react/recommended"],
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["react"],
  "rules": {}
}
```

- 직접 수정한 .eslintrc.json

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:prettier/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint", "react"],
  "rules": {
    "no-var": "warn",
    "no-multiple-empty-lines": "warn",
    "no-console": ["warn", { "allow": ["warn", "error"] }],
    "eqeqeq": "warn",
    "dot-notation": "warn",
    "react/jsx-pascal-case": "warn",
    "react/no-direct-mutation-state": "warn",
    "react/jsx-no-useless-fragment": "warn",
    "react/no-unused-state": "warn",
    "react/jsx-key": "warn",
    "react/self-closing-comp": "warn",
    "react/jsx-curly-brace-presence": "warn",
    "prettier/prettier": [
      "error",
      {
        "endOfLine": "auto"
      }
    ]
  }
}
```

- rules에서 `react`로 시작하는 설정은 링크 참고
  <a href="https://github.com/jsx-eslint/eslint-plugin-react/tree/master/docs/rules" target="_blank">eslint-plugin-react/docs/rules</a>
- `eslint:recommended`는 <a href="https://eslint.org/docs/latest/rules/" target="_blank">eslint.org/docs/latest/rules</a>에 체크되어있는 모든 규칙들이 활성화되어있다. 난 여기서 추가설정을 몇개 더 rules에 추가하였다.

  {: .prompt-info }

  > no-var : var 금지. let과 const사용하도록 함<br/> no-multiple-empty-lines : 공백 한 줄만 허용<br/> no-console : console.log() 사용 금지<br/> eqeqeq : ==안됨, ===사용<br/> dot-notation : 가급적이면 대괄호가 아닌 마침표로 notation사용

- eslint-plugin-prettier: prettier를 eslint에서 사용할 수 있게 해주는 플러그인..
  > 근데 공식문서에 추천하지 않는다고 나와있다! prettier를 단독으로 사용하는 것 보다 느려지는 등의 문제들이 있다고 한다. 참고해야 할 듯 하다.<br/><a href="https://prettier.io/docs/en/integrating-with-linters.html#notes" target="_blank">prettier.io/docs/en/integrating-with-linters.html#notes</a>
- eslint-config-prettier: 불필요하거나 prettier와 충돌하는 rule을 해제시켜주는 플러그인<br/>
  <a href="https://github.com/prettier/eslint-config-prettier" target="_blank">github.com/prettier/eslint-config-prettier</a>

---

## Prettier 설정

### Prettier란

- 코드 포맷터. 설정에 따라 코드를 정리해준다.
- eslint가 코드 작성 방식을 일관되게 잡아주는 것이라면 prettier는 코드 가독성 등 코드의 스타일(들여쓰기 같은 부분)을 잡아주는 것이라고 이해했다.

### 설치

`npm install --save-dev --save-exact prettier`

### 설정

- .prettierrc

```json
{
  "tabWidth": 2,
  "endOfLine": "lf",
  "arrowParens": "avoid",
  "singleQuote": true
}
```

설정 내용은 링크 참고 <a href="https://prettier.io/docs/en/options.html" target="_blank">prettier.io/docs/en/options.html</a>

---

## Lint-staged 설정

### Lint-staged란

- stage(git add)에 올라가있는 파일들에 lint를 실행하고 설정한 명령어를 실행해주는 라이브러리

### 설치

`npm install --save-dev lint-staged`

### 설정

- .lintstagedrc

```json
{
  "./src/**/*.[jt]s?(x)": "eslint --cache --fix"
}
```

- js, jsx, ts, tsx파일에 대해 eslint 에러를 자동으로 해결하도록 설정

## Husky 설정

### Husky란

- eslint, 코드 테스트, 커밋 메세지 체크 등을 Git-hooks에 걸어 자동화할 수 있다.

{: .prompt-tip }

> Git-hooks : 어떤 이벤트가 생겼을 때 자동으로 특정 스크립트를 실행하도록 할 수 있다.<br/> 자세한 내용은 다음 링크 참고<br/><a href="https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks">8.3 Git맞춤 - Git Hooks</a>

### 설치

`npx husky-init && npm install`

- 위 명령어를 실행하면 .husky 폴더가 생긴다.<br/>
  ![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/ba929237-367e-4730-8af5-aba9691d6821)

### 설정

`npx husky add .husky/commit-msg 'npx lint-staged'` 를 실행하면 `.husky/pre-commit` 파일이 생긴다.

- pre-commit : 커밋 전에 스크립트를 실행시키는 Git-Hooks중 하나

```script
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged

```

---

{: .prompt-warning }

> 여기저기 구글링해서 설정한 과정입니다. 부족한 부분이 많습니다! 피드백은 언제나 환영입니다!

---

출처 및 참고)<br/>
<a href="https://eslint.org/docs/latest/use/getting-started" target="_blank">eslint 공식문서</a><br/>
<a href="https://ingg.dev/eslint/" target="_blank">ingg.dev - [JS] ESLint 적용하기 </a><br/>
<a href="https://github.com/okonet/lint-staged" target="_blank">okonet/lint-staged</a><br/>
<a href="https://typicode.github.io/husky/" target="_blank">typicode.github.io/husky/</a><br/>
<a href="https://velog.io/@rookieand/Git-Hook%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-Husky%EB%8A%94-%EC%99%9C-%EC%93%B0%EB%8A%94%EA%B1%B8%EA%B9%8C" target="_blank">rookieand - Git Hook은 무엇이고, Husky는 왜 쓰는걸까?</a><br/>
<a href="https://helloinyong.tistory.com/325" target="_blank">helloinyong - ESLint, Prettier Setting, 헤매지 말고 정확히 알고 설정하자.</a>

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
