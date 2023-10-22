---
title: Redux Toolkit - thunk
date: 2023-10-22 16:22:00 +09:00
categories: [Redux, React, Redux-Toolkit]
tags: [redux, react, redux-toolkit] # TAG names should always be lowercase
---

> 생활코딩의 <a href='https://www.youtube.com/watch?v=K-3sBc2pUJ4' target="\_blank">redux toolkit - thunk 를 이용해서 비동기 작업을 처리하는 방법</a>을 듣고 정리한 내용입니다.

서버와 통신하는 경우처럼 redux로 비동기 작업을 어떻게 처리해야 할까?

## createAsynkThunk

- 비동기 작업을 처리하는 action을 만들어준다.
- 인자로 타입(string)과 promise를 반환하는 함수, 옵션을 받는다.

```jsx
// counterSlice.js
const asynkUpFetch = createAsynkThunk("counterSlice/asyncUpFetch", async () => {
  const resp = await fetch(url);
  const data = await resp.json();
  // 여기서 return한 value는 payload로 reducer에 전달된다.
  return data.value;
});

// 다른 파일에서
<button
  onClick={() => {
    dispatch(asyncUpFetch());
  }}
>
  + async fetch
</button>;
```

### extraReducers

{: .prompt-info }

> createAsyncThunk를 썼을 때 action creator가 같는 3가지 상태가 있다.<br/>pending : 대기 상태<br/>fulfilled : 완료 상태<br/>rejected : 오류 상태

- reducer는 createSlice 안에 extraReducers로 정의한다.
- 이때 builder.addCase()를 사용한다.<br/>
  (fulfilled일 때만 정의해도 된다.)

```jsx
const counterSlice = createSlice({
  name: "counterSlice",
  initialState: {
    value: 0,
    status: "Welcome"
  },
  reducers: {
    up: (state, action) => {
      state.value = state.value + action.payload;
    }
  },
  // 여기!
  extraReducers: (builder) => {
    // pending 상태일 때 reducer를 두 번째 인자로 전달한다.
    builder.addCase(asyncUpFetch.pending, (state, action) => {
      // 여기서 작성한 status는 useSelector의 status로 전달된다.
      state.status = "Loading";
    });
    // fulfilled 상태일 때 reducer를 두 번째 인자로 전달한다.
    builder.addCase(asyncUpFetch.fulfilled, (state, action) => {
      // 여기서 작성한 value는 useSelector의 value로 전달된다.
      state.value = action.payload;
      state.status = "complete";
    });
    // rejected 상태일 때 reducer를 두 번째 인자로 전달한다.
    builder.addCase(asyncUpFetch.rejected, (state, action) => {
      state.status = "fail";
    });
  }
});
```

왜 비동기 작업은 extraReducers로 reducer를 따로 만들까?

- reducers를 사용하면 `action creator`를 toolkit이 자동으로 만들어주지만, 비동기 작업에서는 그렇지 않기 때문이다. 그렇기 때문에 extraReducers 내부에 action creator를 정의한다.

---

출처)
<a href='https://www.youtube.com/watch?v=K-3sBc2pUJ4' target="\_blank">생활코딩 : redux toolkit - thunk 를 이용해서 비동기 작업을 처리하는 방법</a>

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
