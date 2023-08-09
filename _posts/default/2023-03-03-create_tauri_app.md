---
title: "우당탕탕 타우리 #002💬 프로젝트 생성하기" 
sub: post
author: Kwangsoo Seo
date: 2023-03-03 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, WebView, Tauri]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post24.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post24)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  

# ✔ create-tauri-app 도구로 Tauri 프로젝트 생성하기   

Tauri는 거의 모든 프론트엔드 스택과 호환이 됩니다. 프로젝트를 생성하기 가장 쉬운 방법은 create-tauri-app을 이용하는 것입니다. 이 도구는 vanilla HTML/CSS/JavaScript나 React, Svelte, Yew 등의 다양한 프론트엔드 프레임워크를 위한 독자적인 템플릿을 제공합니다.
여기서는 윈도우에는 기본적으로 설치되어 있는 PowerShell과 개발을 위해 설치해야하는 Cargo를 이용하는 방법으로 진행을 해보도록 하겠습니다. 이외의 다양한 방법은 tauri 공홈 문서(https://tauri.app/v1/guides/getting-started/setup/)에서 확인하시기 바랍니다. 아래의 2가지 방법중 하나를 이용해서 기본 골격의 tauri 프로젝트를 생성합니다.

## ⛓ PowerShell 이용하기   

파워쉘을 오픈하고 프로젝트 폴더를 생성할 폴더로 이동 후 아래의 명령을 입력합니다.   
> PS C:\\>**<span style="color:black">$env:CTA_ARGS="--alpha";iwr -useb https://create.tauri.app/ps | iex</span>**   
> info: downloading create-tauri-app   
> <span style="color:limegreen">✔</span> <span style="color:black">Project name · Workboard</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose which language to use for your frontend · Rust - (cargo)</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose your UI template · Vanilla</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Would you like to setup the project for mobile as well? · no</span>   
>    
> Please follow https://tauri.app/v1/guides/getting-started/prerequisites to install the needed prerequisites, if you haven't already.   
> You also need to install tauri-cli (cargo install tauri-cli --version 2.0.0-alpha.2)   
>    
> Done, now run:   
>   cd Workboard   
>   cargo tauri dev   

위에 프로젝트 생성시 프론트엔드를 Rust로 선택하였으며,   
> Choose which language to use for your frontend · Rust - (cargo)   

Rust로 선택하는 경우에는 패키지를 관리하기 위한 패키지 매니저를 선택할 수 없습니다. 아래에 다시 설명을 하겠지만, 프론트엔드를 예시처럼 TypeScript / JavaScript를 선택한 경우에는 패키지 매니저를 선택하여야 합니다. 
Rust를 선택한 경우에 src-tauri 폴더 아래의 Cargo.toml을 살펴 보면, 아래와 같이 라이브러리 항목이 생성된 것을 확인 하실수 있습니다.

> [lib]   
> name = "Workboard_lib"   
> crate-type = ["staticlib", "cdylib", "rlib"]   

프론트엔드로 Rust를 설정하였기에 라이브러리를 생성해주어야 오류가 발생되지 않습니다. 일단 우리는 기본 화면을 띄울 것이기 때문에 이 부분을  주석(#) 처리 해줍니다.    

> #[lib]   
> #name = "Workboard_lib"   
> #crate-type = ["staticlib", "cdylib", "rlib"]   

작업이 모두 끝났다면, 아래와 같이 cargo run 명령을 입력합니다.   

> PS C:\\Workboard\\src-tauri>**<span style="color:black"> cargo run</span>**   

오류가 발생하지 않았다면 아래와 같이 브라우저가 정상적으로 출력이 될 것입니다.

![Tauri_create_tauri_app](https://monoslab.github.io/assets/img/posts/create_tauri_app.png)   

---

## ⛓ Cargo 이용하기   

우선 윈도우 Command 창을 열고 프로젝트를 생성할 폴더로 이동 후 아래의 명령을 입력하여 create-tauri-app을 설치합니다.   

> C:\\> **<span style="color:black">cargo install create-tauri-app</span>**   
>     <span style="color:limegreen">Updating</span> crates.io index   
>   <span style="color:limegreen">Downloaded</span> create-tauri-app v3.0.2   
>   <span style="color:limegreen">Downloaded</span> 1 crate (254.6 KB) in 1.27s   
>   <span style="color:limegreen">Installing</span> create-tauri-app v3.0.2   
>   <span style="color:limegreen">Downloaded</span> libflate v1.3.0   
>   <span style="color:limegreen">Downloaded</span> libflate_lz77 v1.2.0   
>   <span style="color:limegreen">Downloaded</span> dialoguer v0.10.3   
>   <span style="color:limegreen">Downloaded</span> syn v1.0.109   
>   <span style="color:limegreen">Downloaded</span> shell-words v1.1.0   
>   <span style="color:limegreen">Downloaded</span> tempfile v3.4.0   
>   <span style="color:limegreen">Downloaded</span> console v0.15.5   
>   <span style="color:limegreen">Downloaded</span> 7 crates (389.6 KB) in 1.10s   
>    <span style="color:limegreen">Compiling</span> typenum v1.16.0   
>    ... 중략 ...   
>    <span style="color:limegreen">Compiling</span> create-tauri-app v3.0.2   
>     <span style="color:limegreen">Finished</span> release [optimized] target(s) in 1m 51s   
>    <span style="color:limegreen">Replacing</span> C:\Users\mono\.cargo\bin\cargo-create-tauri-app.exe   
>     <span style="color:limegreen">Replaced</span> package `create-tauri-app v2.7.6` with `create-tauri-app v3.0.2` (executable `cargo-create-tauri-app.exe`)   

위의 설치 과정이 끝났다면 cargo를 이용하여 create-tauri-app을 실행합니다.   

> C:\\> **<span style="color:black">cargo create-tauri-app</span>**   
> <span style="color:limegreen">✔</span> <span style="color:black">Project name · Workboard2</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose which language to use for your frontend · TypeScript / JavaScript - (pnpm, yarn, npm)</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose your package manager · pnpm</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose your UI template · Vanilla</span>   
> <span style="color:limegreen">✔</span> <span style="color:black">Choose your UI flavor · JavaScript</span>   
>    
> Please follow https://tauri.app/v1/guides/getting-started/prerequisites to install the needed prerequisites, if you haven't already.   
>    
> Done, now run:   
>   cd Workboard2   
>   pnpm install   
>   pnpm tauri dev   

프로젝트 생성시 프론트엔드를 TypeScript / JavaScript로 선택한 예시입니다.   

> Choose which language to use for your frontend · TypeScript / JavaScript - (pnpm, yarn, npm)   

이 경우에는 패키지를 관리해줄 패키지 매니저를 지정해줘야 합니다. 필자는 이미 PC에 설치되어 있는 pnpm을 선택하여 지정해줬습니다.   

위와 마찬가지로 작업이 모두 끝났다면, 아래와 같이 cargo run 명령을 이용하여 앱을 실행시킬 수도 있고, 패키지 매니저인 pnpm을 이용하여 앱을 실행할 수도 있습니다.   

> C:\\Workboard2\\src-tauri>**<span style="color:black"> cargo run</span>**   

pnpm은 cargo create-tauri-app 명령 수행후 아래의 실행 방법 예시처럼   

> C:\\>**<span style="color:black"> cd Workboard2</span>**   
> C:\\Workboard2>**<span style="color:black"> pnpm install</span>**   
> C:\\Workboard2>**<span style="color:black"> pnpm tauri dev</span>**   

명령을 차례대로 입력하시면 완성된 브라우저가 뜨게 될 것입니다.   

다음 강좌에서는 프로젝트의 설정(tauri.conf.json, cargo.toml)에 대해 알아보는 시간을 갖도록 하겠습니다.  

> ※ 참고
> pnpm 설치 및 버전 업데이트 방법
> pnpm 버전 확인 : pnpm -v
> npm 이용 : npm install -g pnpm
> 윈도우 파워쉘 이용 : iwr https://get.pnpm.io/install.ps1 -useb | iex