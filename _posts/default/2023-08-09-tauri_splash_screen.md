---
title: "우당탕탕 타우리 #007💬 Splash screen 만들기"
sub: post
author: Kwangsoo Seo
date: 2023-08-09 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, Windows, Splash, screen]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post34.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post34)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# Splash screen   
윈도우 어플리케이션에서는 Splash window라고도 불리는 Splash screen(이하 스플래쉬 스크린)을 만들어 보려고 합니다. 스플래쉬 스크린은 어플리케이션이 로딩되는 동안에 잠깐 나타났다가 사라지는 윈도우입니다. 따라서 로딩 시간이 긴 경우에는 스플래쉬 스크린을 통해 "지금 실행되고 있어요"라고 사용자가 인지할 수 있게 해주는 유용한 윈도우 입니다.   

## 웹 페이지를 이용한 방법

### tauri.conf.json 설정   
타우리 프로젝트를 만드시고 tauri.conf.json에 스플래쉬 스크린으로 사용할 윈도우를 아래와 같이 추가해 줍니다.   
```json
    "windows": [
      {
        "fullscreen": false,
        "resizable": true,
        "title": "SplashWindow",
        "width": 800,
        "height": 600,
+      "visible": false,
+      "center": true
+    },
+    {
+      "fullscreen": false,
+      "resizable": false,
+      "width": 250,
+      "height": 250,
+      "visible": true,
+      "center": true,
+      "label": "splashscreen",
+      "url": "splash.html",
+      "alwaysOnTop": true,        
+      "decorations": false
+    }
    ]
```

'+'로 표기된 부분은 수정 또는 추가된 부분이며, 몇가지 중요한 부분을 집고 넘어 가겠습니다. 메인 윈도우의 경우에는 visible 값을 false로 변경하여 주시고, 새로 추가한 윈도우에 visible 값은 true를 입력합니다. 그리고 label에 적절한 값을 입력해줍니다. 이 label 속성은 향후 윈도우를 찾는데 사용될 것입니다. 그리고 url 속성의 값에는 로딩 페이지를 입력해줍니다. 마지막으로 decorations의 값을 false로 입력하여 윈도우의 타이틀바를 제거해줍니다.   

### main.rs

이제 main.rs의 소스를 열어,   
```rust
use tauri::{ Window, Manager };

#[tauri::command]
async fn close_splashscreen(window: Window) {
    if let Some(splashscreen) = window.get_window("splashscreen") {
        std::thread::sleep(std::time::Duration::from_secs(2));
        splashscreen.close().unwrap();
    }

    window.get_window("main").unwrap().show().unwrap();    
}

fn main() {
    tauri::Builder::default()
        .invoke_handler(tauri::generate_handler![close_splashscreen])
        .run(tauri::generate_context!())
        .expect("Failed to run app.");
}
```
로딩이 완료되면 호출될 close_splashscreen 함수를 생성해줍니다. 함수 내의 내용을 살펴보면, splashscreen이라는 label 값을 갖는 윈도우인지 체크해서 splashscreen 윈도우라면 종료하도록 합니다. 그리고 프로젝트 생성시 자동으로 생성되었던 메인 윈도우를 보여지도록 합니다. main() 함수에는 이렇게 만들어진 close_splashscreen 함수를 자바스크립을 통해 접근할 수 있도록 invoke_handler 메소드를 이용하여 핸들을 등록시켜 줍니다.   

### splash.js, splash.css 및 splash.html

[ splash.js ]   
```js
const { invoke } = window.__TAURI__.tauri;

window.addEventListener("DOMContentLoaded", () => {
  invoke('close_splashscreen');
});
```

[ splash.html ]   
```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="splash.css">
    <title>Splash window</title>
    <script type="module" src="splash.js" defer></script>
  </head>
  <body>
    <div class="loader">
      <div class="face">
        <div class="circle"></div>
      </div>
      <div class="face">
        <div class="circle"></div>
      </div>
    </div>
  </body>
</html>
````
splash.css는 Comehope이 만드신 'Comet Rotating Loader'를 사용하였습니다. free 코드라고는 하는데 상용으로 사용하실때 한번 더 체크해보세요. splash.css는 아래의 링크를 참고해주세요.

**✏ codepen** : [Comet Rotating Loader](https://codepen.io/comehope/pen/YLRLaM){:target="_blank"}   

## Rust 코드를 이용한 방법

tauri.conf.json, splash.js, splash.css, splash.html의 작업은 동일하고, main.rs의 소스코드만 아래와 같이 변경하시면 됩니다.

### main.rs

```rust
use tauri::{ Window, Manager };

fn main() {
    tauri::Builder::default()
        .setup(|app| {
            let splash_window = app.get_window("splashscreen").unwrap();
            let main_window = app.get_window("main").unwrap();

            tauri::async_runtime::spawn(async move {
                println!("Initializing..."); 
                std::thread::sleep(std::time::Duration::from_secs(5));
                println!("Done init...");

                splash_window.close().unwrap();
                main_window.show().unwrap();
            });
            Ok(())
        })
        .run(tauri::generate_context!())
        .expect("Failed to run app.");
}
```

간단히 내용을 요약하자면, 스플래쉬 스크린과 메인 윈도우의 객체를 얻어오고 로딩 작업(tauri::async_runtime::spawn)이 끝나면 스플래쉬 스크린을 닫고(close()) 메인 윈도우를 보이게(show()) Rust 코드로도 구현이 가능합니다.

> **<span style="color:black">참고 : 위의 내용은 타우리 공식 가이드문서(https://tauri.app/v1/guides/features/splashscreen/)를 참고하여 만들었습니다.</span>**