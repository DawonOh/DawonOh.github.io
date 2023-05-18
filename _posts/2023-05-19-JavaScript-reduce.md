---
title: JavaScript-reduce메소드
date: 2023-05-19 06:01:00 +09:00
categories: [JavaScript]
tags: [javascript] # TAG names should always be lowercase
---

> 프로그래머스 LV.0을 풀다보니 reduce메소드를 사용한 코드를 많이 보게 되었다.

## 사용방법

```javascript
// 사용방법
array.reduce((누적값, 현재값, 현재 인덱스, 원본 배열) => {
  return 결과;
}, 초기값);
```

reduce 함수의 return값은 누적값에 계속 누적된다. → 최종적으로 하나의 값이 된다.

- 누적값(accumulator) : 콜백함수의 반환값을 누적한다. 첫 번째 호출인데 초기값이 있으면 초기값과 같은 값을 가진다.
- 현재값(currentValue) : 처리할 현재 요소
- 현재 인덱스(currentIndex) : 처리할 현재 요소의 인덱스. 초기값이 있으면 0부터 시작하고, 없으면 1부터 시작한다.
- 원본 배열(array) : reduce()를 호출한 배열
- 초기값(initialValue) : 콜백함수의 첫 번째 호출에서 첫 번째 인수에 해당하는 값. 만약 원본 배열이 빈 배열이라면 초기값이 있어야한다.
- 결과(return) : 누적 계산의 결과 값

---

## 예시1. 초기값이 없는 경우

```javascript
[0, 1, 2, 3, 4].reduce(function (
  누적값,
  현재값,
  현재 인덱스,
  원본 배열
) {
  return 누적값 + 현재값;
});

// 또는

[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr );

```

|callback|누적값|현재값|현재 인덱스|원본 배열|반환 값|
|1번째 호출|0|1|1|[0, 1, 2, 3, 4]|1|
|2번째 호출|1|2|2|[0, 1, 2, 3, 4]|3|
|3번째 호출|3|3|3|[0, 1, 2, 3, 4]|6|
|4번째 호출|6|4|4|[0, 1, 2, 3, 4]|10|

→ 최종 반환 값 : 10

## 예시2. 초기값이 10인 경우

```javascript
[0, 1, 2, 3, 4].reduce(function (
  accumulator,
  currentValue,
  currentIndex,
  array
) {
  return accumulator + currentValue;
},
10);

// 또는

[0, 1, 2, 3, 4].reduce((prev, curr) => prev + curr, 10);
```

|callback|누적값|현재값|현재 인덱스|원본 배열|반환 값|
|1번째 호출|10|0|0|[0, 1, 2, 3, 4]|10|
|2번째 호출|10|1|1|[0, 1, 2, 3, 4]|11|
|3번째 호출|11|2|2|[0, 1, 2, 3, 4]|13|
|4번째 호출|13|3|3|[0, 1, 2, 3, 4]|16|
|5번째 호출|16|4|4|[0, 1, 2, 3, 4]|20|

→ 최종 반환 값 : 20

---

출처 ) <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce" target="_blank">mdn - Array.prototype.reduce()</a>

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
