---
title: prettier - Error loading node module 해결하기
date: 2023-06-29 13:32:00 +09:00
categories: [prettier, error]
tags: [prettier, error] # TAG names should always be lowercase
---
> CRA로 프로젝트를 생성했는데 prettier가 먹히지 않고 다음과 같은 에러가 생겼다.
```txt
["ERROR" - 1:23:19 PM] Error handling text editor change
["ERROR" - 1:23:19 PM] Error loading node module '/home/~/node_modules/prettier.'
Error: Error loading node module '/home/~/node_modules/prettier.'
	at t.loadNodeModule (/home/~/.vscode-server/extensions/esbenp.prettier-vscode-9.16.0/dist/extension.js:1:2619)
	at t.PrettierMainThreadInstance.import (/home/~/.vscode-server/extensions/esbenp.prettier-vscode-9.16.0/dist/extension.js:1:17236)
	at t.PrettierMainThreadInstance.getSupportInfo (/home/~/.vscode-server/extensions/esbenp.prettier-vscode-9.16.0/dist/extension.js:1:17687)
	at t.default.getSelectors (/home/~/.vscode-server/extensions/esbenp.prettier-vscode-9.16.0/dist/extension.js:1:11370)
	at t.default.handleActiveTextEditorChanged (/home/~/.vscode-server/extensions/esbenp.prettier-vscode-9.16.0/dist/extension.js:1:10790)
	at processTicksAndRejections (node:internal/process/task_queues:96:5)
```
## 해결하기
- 위 에러를 잘 보면 node modules에서 prettier. 를 불러오지 못한 것 같다.
실제로 node modules에서 prettier.를 찾아보니 prettier. 이 아니라 그냥 prettier로 되어있었다.
![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/c851c67e-59bc-421b-ba9f-f7183aa7784a)
- vscode의 settings.json에서 `prettier.prettierPath`를 다음과 같이 수정한다.
![image](https://github.com/DawonOh/DawonOh.github.io/assets/89020079/43f1d1c8-b8df-4391-8e41-a9373666b0e8)<br/>
⇒ 다시 파일을 저장해보면 prettier가 잘 적용되는 걸 볼 수 있다!
```txt
["INFO" - 1:41:36 PM] Formatting completed in 181ms.
```

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