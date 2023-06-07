---
title: Node - Readline
date: 2023-05-24 00:47:00 +09:00
categories: [JavaScript, Node]
tags: [javascript, node] # TAG names should always be lowercase
---

> 아래 문제는 프로그래머스 0단계 문제 중 '직각삼각형 출력하기' 이다.<br/> > `"_"의 높이와 너비를 1이라고 했을 때, "_"을 이용해 직각 이등변 삼각형을 그리려고합니다. 정수 n 이 주어지면 높이와 너비가 n 인 직각 이등변 삼각형을 출력하도록 코드를 작성해보세요.`

- 입력 : 3
- 출력 :<br/> \*<br/>
  \*\*<br/>
  \*\*\*

> 내가 당황한건 기본으로 주어진 코드가 다음과 같았기 때문이다.

```javascript
const readline = require("readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

let input = [];

rl.on("line", function (line) {
  input = line.split(" ");
}).on("close", function () {
  console.log(Number(input[0]));
});
```

여기서 readline이란 뭘까?

# Readline

- 한 번에 한 줄씩 Readable 스트림(process.stdin 등)에서 데이터를 읽기 위한 인터페이스를 제공합니다.<br/>
  (라고는 하는데 이해가 잘 되지 않았다. 아마 값을 입력받는 인터페이스를 제공한다는 의미인 것 같다..)
- 위에 있는 코드를 뜯어보자.

`const readline = require("readline");`

- Node의 core modules중 하나인 readline을 import한다.
  - `core modules` : Node에서 자체적으로 제공하는 모듈

---

`const rl = readline.createInterface({});`

- 입력을 받을 인터페이스를 만든다.

---

`input: process.stdin` / `output: process.stdout`

- standard input/output의 약자
- 표준 입출력 처리를 위해 필수로 넣어야 한다.
- [process.stdin - nodejs.org](https://nodejs.org/api/process.html#processstdin)
- [process.stdout - nodejs.org](https://nodejs.org/api/process.html#processstdout)

---

`let input = [];`

- input을 출력해보면 문자열 요소를 가진 배열로 출력이 된다. 배열에 담길 각 값들을 담을 빈 배열을 선언한다.

---

`rl.on("line", function (line) {input = line.split(" ");})`

- `"line"` : 사용자가 엔터키(\n)를 누르거나 개행한 경우(\r) 입력받은 문자열을 받아온다. (한 줄만 입력받는다!)<br/>
  [event-line - nodejs.org](https://nodejs.org/api/readline.html#event-line)
- `input = line.split(" ");` : 입력받은 line을 input에 공백을 기준으로 나누어 넣는다.

---

`.on("close", function () {console.log(Number(input[0]));});`

- "close" : 사용자가 입력을 마치고 콜백 함수가 실행되고 나면 인터페이스를 종료한다. 종료하지 않으면 콜백 함수 실행 후에도 사용자가 계속 입력할 수 있게 된다!<br/>
  (위 프로그래머스 문제를 풀 때는 두번째 인자인 콜백함수의 실행 부분에 코드를 작성하면 된다.)<br/>
  [event-close - nodejs.org](https://nodejs.org/api/readline.html#event-close)

---

> 좋은 블로그가 많아서 나도 모르게 공식문서를 소홀히 하게 된다. 공식문서를 많이 찾아보는 습관을 길러야겠다.

출처)<br/>
[NodeJS 6강 - Core Module (readline) / Danny TWLC(Youtube)](https://youtu.be/VXTJFlCZoT8);<br/>
[Readline - nodejs.org](https://nodejs.org/api/readline.html)<br/>
[readline 모듈 사용하기 - leenzy](https://velog.io/@leenzy/readline-%EB%AA%A8%EB%93%88-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)<br/>
[Node.js로 입력값 받기 - onys](https://developeonmyown.tistory.com/172)<br/>

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
