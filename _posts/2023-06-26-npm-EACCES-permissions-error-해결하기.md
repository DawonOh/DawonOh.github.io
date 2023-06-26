---
title: npm - EACCES permissins error 해결하기
date: 2023-06-26 17:41:00 +09:00
categories: [npm, error]
tags: [npm, error] # TAG names should always be lowercase
---

> npm 업데이트를 하려고 하니 다음과 같은 에러가 발생했다.<br/>
`Error: EACCES: permission denied, rename '/usr/lib/node_modules/npm' -> ~~~`<br/>

## npm 최신 버전으로 업데이트하기
`sudo npm i -g npm`

## 에러 해결하기
npm docs 페이지에서 보니 global 설치 시 저런 에러가 나는 것 같다. node 버전 관리자로 npm을 다시 설치하는 방법도 있고, 수동으로 직접 npm 디렉토리를 변경하는 방법이 있다.

### 해결 방법
1. 홈 디렉토리에서 전역 설치를 위한 디렉토리 만들기<br/>
`mkdir ~/.npm-global`

2. 새로 만든 디렉토리 경로를 사용하도록 npm을 구성한다.<br/>
`npm config set prefix '~/.npm-global'`

3. .profile 파일을 열어 맨 아래에 내용을 추가한다.<br/>
`export PATH=~/.npm-global/bin:$PATH`

4. 수정한 .profile 내용을 적용한다.<br/>
`source ~/.profile`

5. 다시 npm 업데이트를 진행하면 된다!<br/>

---

출처) <br/>
<a href="https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally" target="_blank">Resolving EACCES permissions errors when installing packages globally</a>

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