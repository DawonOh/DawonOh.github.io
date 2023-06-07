---
title: JavaScript - toString(),parseInt() 진수 변환
date: 2023-06-08 00:12:00 +09:00
categories: [JavaScript]
tags: [javascript] # TAG names should always be lowercase
---

> 지금까지 toString()을 데이터를 문자열로 바꾸기 위해서만 사용했는데, 진수 변환도 가능하다고 한다.

## toString()

### 문자열 반환

- 객체에 toString()을 사용하면 `"[object Object]"`를 반환한다.
- 배열에 toString()을 사용하면 배열 안의 요소들을 하나의 문자열로 반환한다.

```javascript
let num = 11;
console.log(num.toString()); // '11'

const emptyObj = {};
console.log(emptyObj.toString()); // "[object Object]"

const myObj = { id: 1, name: "kim" };
console.log(myObj.toString()); // "[object Object]"

const emptyArr = [];
console.log(emptyArr.toString()); // ""

const strArr = ["a", "b", "c"];
console.log(strArr.toString()); // "a,b,c"

const numArr = [1, 2, 3];
console.log(numArr.toString()); // "1,2,3"

const objArr = [
  { id: 1, name: "kim" },
  { id: 2, name: "lee" }
];
console.log(objArr.toString()); // "[object Object],[object Object]"
```

### 진수 변환

- 10진수를 지정한 진수로 변환하여 반환한다.

```javascript
let baseTenInt = 10;
// 10진수인 10을 2진수로 변환
console.log(baseTenInt.toString(2)); // "1010"
```

---

## parseInt()

- 문자열 인자를 파싱하여 특정 진수의 정수를 반환한다
  ⇒ 특정 진수의 값을 10진수 정수로 반환한다.

```javascript
let binaryNum = 1010;
console.log(parseInt(binaryNum, 2)); // 10
```

---

출처)<br/>
<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/toString" target="_blank">mdn - Object.prototype.toString()</a>
<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt" target="_blank">mdn - parseInt()</a>

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
