---
title: "[ RUST ] Tauri 타이틀바 커스트마이징" 
sub: post
author: Kwangsoo Seo
date: 2023-03-10 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post26.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post26)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 타이틀바 커스트마이징   

## tauri.conf.json

타이틀바를 커스트마이징 하기 위해서는 타우리 설정 파일에서 윈도우에 관련된 몇가지 항목을 설정해줘야 합니다.   
아래는 tauri.conf.json의 내용중 타이틀바 커스트마이징시 필요한 내용중 일부분입니다.
```   
"tauri": {
  "allowList": {
    ...
    "window": {
        "all": false,
        "close": true,
        "hide": true,
        "show": true,
        "maximize": true,
        "minimize": true,
        "unmaximize": true,
        "unminimize": true,
        "startDragging": true
      }
  }
  ...
  "windows": [
      {
        ...
        "decorations": false
      }
    ]
}
```   
---

먼저 tauri > windows 항목에 decorations 키를 false로 설정합니다. decorations 옵션이 적용되면 타이틀바가 사라지게되고, 드래그를 할 수 있는 영역이 사라져서 드래그를 할 수 없는 상태가 됩니다.   
다음으로 tauri > allowList > window 항목에서 타이틀바가 없을 경우 필요한 시스템 메뉴의 key/value를 설정합니다. 여기서 startDragging은 반드시 사용함(true)로 설정해주셔야 생성할 타이틀바 영역을 드래그하여 윈도우를 이동시킬 수 있으니 유의해야합니다. 그 외의 시스템 버튼들은 필요에 따라 사용함(true)으로 설정해주시면 됩니다.   

## index.html
tauri.conf.json 설정이 완료되면 프로젝트 폴더의 src/index.html 파일을 아래와 같이 수정해줍니다.

```
// titlebar.css
.titlebar {
    height: 30px;
    background: #0077ff;
    user-select: none;
    display: flex;
    justify-content: flex-end;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
  }
  .titlebar-button {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    height: 30px;
  }
  .titlebar-button:hover {
    background: #bed3f8;
  }
```
```html
<html>
   <head>
...
      <link rel="stylesheet" href="titlebar.css" />
...
   </head>
   <body>
...
      <div data-tauri-drag-region class="titlebar">
        <div class="titlebar-button" id="titlebar-minimize">
          <img
            src="https://api.iconify.design/mdi:window-minimize.svg"
            alt="minimize"
          />
        </div>
        <div class="titlebar-button" id="titlebar-maximize">
          <img
           src="https://api.iconify.design/mdi:window-maximize.svg"
           alt="maximize"
           />
         </div>
         <div class="titlebar-button" id="titlebar-close">
           <img src="https://api.iconify.design/mdi:close.svg" alt="close" />
         </div>
      </div>
...
   </body>
</html>
```

위와 같이 잘 적용되었다면, 빌드 후 정상적으로 타이틀바가 나타나고 드래그 및 우리가 원했던 시스템 버튼들이 보여지게 될 것입니다. 마지막으로 보여지는 버튼들에게 살아 숨쉴수 있도록 숨을 불어 넣어 주어야겠죠.


## main.js

 src/main.js 파일을 아래와 같이 수정해줍니다.
```js
const { invoke } = window.__TAURI__.tauri;
...
window.addEventListener("DOMContentLoaded", () => {
  ... 중략 ...
  document
    .querySelector('#titlebar-minimize')
    .addEventListener('click', () => window.__TAURI__.window.appWindow.minimize());
  document
    .querySelector('#titlebar-maximize')
    .addEventListener('click', () => window.__TAURI__.window.appWindow.toggleMaximize());
  document
    .querySelector('#titlebar-close')
    .addEventListener('click', () => window.__TAURI__.window.appWindow.close());
});
```

이제 모든 작업이 완료되었습니다. 버튼들을 눌러보면 모두 정상적으로 작동 됨을 확인하실 수 있을 것입니다.