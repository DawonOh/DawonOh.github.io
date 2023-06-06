---
title: Javascript - lastIndexOf()
date: 2023-06-07 01:33:00 +09:00
categories: [JavaScript, algorithm]
tags: [javascript, algorithm] # TAG names should always be lowercase
---

> 프로그래머스 0단계 '한 번만 등장한 문자'

## 내 풀이

```javascript
function solution(s) {
  // s가 'hello'라면

  // 주어진 문자를 set을 사용해 중복이 없는 배열로 만든다.
  let set = new Set([...s]); // [h,e,l,o]

  // set에 있는 각 요소가 몇 번씩 나왔는지 횟수를 cnt 배열에 저장
  let cnt = [];
  set.forEach((i) => {
    cnt.push([...s].filter((num) => num === i).length);
  });
  // cnt = [1,1,2,1]

  // cnt에서 값이 1인 요소의 index로 set에서 찾아 oneStr에 push
  let oneStr = [];
  for (let i = 0; i < cnt.length; i++) {
    if (cnt[i] === 1) oneStr.push(Array.from(set)[i]);
  }
  // 정렬하고 문자열로 만들면 끝!
  return oneStr.sort().join("");
}
```

## 다른 사람 풀이 중 lastIndexOf를 사용한 코드

- `문자열.lastIndexOf(값)` : 주어진 값과 일치하는 부분을 문자열에서 역순으로 탐색하여, 최초로 마주치는 인덱스를 반환한다.

```javascript
function solution(s) {
  let res = [];
  // s문자열을 돌면서 c에 해당하는 인덱스와 뒤에서부터 c에 해당하는 인덱스를 찾는다.
  // c문자열이 하나밖에 없으면 s.indexOf(c)와 s.lastIndexOf(c)가 같을 것이다.
  // 같다면 res에 push하고 정렬해서 join하면 끝
  for (let c of s) if (s.indexOf(c) === s.lastIndexOf(c)) res.push(c);
  return res.sort().join("");
}
```

---

출처) <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf" target="_blank">mdn - String.prototype.lastIndexOf()</a>

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
