---
title: axios - interceptors
date: 2023-09-23 22:47:00 +09:00
categories: [axios]
tags: [axios] # TAG names should always be lowercase
---

> 최근 본 면접에서 axios의 interceptors에 대해 질문을 받았다. 이 질문을 받고 대답을 잘 하지 못한 나를 떠올리며 정리해보려 한다.

## Interceptors란

- Intercept 뜻 : 가로채다, 빼앗다
- axios의 Interceptors는 요청과 응답의 then이나 catch가 실행되기 전에 가로챌 수 있게 한다.

## Interceptors 추가하기

```javascript
// 요청 가로채기
axios.interceptors.request.use(
  function (config) {
    // 요청이 전달되기 전에 작업을 추가한다.
    return config;
  },
  function (error) {
    // 요청 오류가 있는 작업을 실행한다.
    return Promise.reject(error);
  }
);

// 응답 가로채기
axios.interceptors.response.use(
  function (response) {
    // 200번 대 상태코드가 이 함수를 트리거한다.(상태 코드가 200번대이어야 실행)
    // 응답받은 데이터를 사용하는 작업 추가
    return response;
  },
  function (error) {
    // 200번대 이외의 상태코드가 이 함수를 트리거한다. (상태 코드가 200번대가 아니어야 실행)
    // 응답 오류를 사용하는 작업 추가
    return Promise.reject(error);
  }
);
```

## Interceptors 제거하기

- `axios.interceptors.request.eject()` 사용

```javascript
const myInterceptor = axios.interceptors.request.use(function () {
  /*...*/
});
axios.interceptors.request.eject(myInterceptor);
```

## 커스텀한 Interceptors 추가하기

- `axios.create()`로 새로운 인스턴스를 만들어서 해당 인스턴스에 interceptors를 추가할 수 있다.

```javascript
const instance = axios.create();
instance.interceptors.request.use(function () {
  /*...*/
});
```

## 어디에 사용할 수 있나?

- JWT를 사용하여 로그인한 정보를 서버에서 가져올 때 사용할 수 있다.<br/>
  (면접에서도 면접관이 의아하게 생각한 부분이었다. JWT를 사용했는데 그럼 매번 요청할 때 마다 로컬 스토리지에서 토큰을 가져와서 넣은 건가요?? 네..)

---

출처) <a href='https://axios-http.com/docs/interceptors' target="\_blank">axios 공식문서 - Interceptors</a><br/>

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
