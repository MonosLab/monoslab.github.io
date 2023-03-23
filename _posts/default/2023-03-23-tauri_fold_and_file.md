---
title: "[ RUST ] Tauri 프로젝트의 폴더 및 파일 구성  - 타우리(Tauri) #3"
sub: post
author: Kwangsoo Seo
date: 2023-03-23 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post28.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post28)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# Tauri 구성   
프로젝트의 설정(tauri.conf.json, cargo.toml)에 대해 알아보기 전에 폴더와 파일 구성을 먼저 알고 가는 것이 좋을 듯 하여 강좌 순서를 조금 바꾸어봤습니다. 먼저 폴더 및 파일의 기본 구성을 알아 보고 다음 강좌에서 설정 파일에 대해 알아보도록 하겠습니다.

## 폴더/파일 구성   
create-tauri-app 도구를 이용하여 프로젝트를 생성하면 기본적으로 아래의 구성으로 프로젝트가 생성되게 됩니다.   

```   
..\Tauri_Project
|   package.json
|   README.md
|
+---src
|   |   index.html
|   |   main.js
|   |   styles.css
|   |
|   \---assets
|           javascript.svg
|           tauri.svg
|
\---src-tauri
    |   build.rs
    |   Cargo.lock
    |   Cargo.toml
    |   tauri.conf.json
    |
    +---icons
    |       128x128.png
    |       128x128@2x.png
    |       32x32.png
    |       icon.icns
    |       icon.ico
    |       icon.png
    |       Square107x107Logo.png
    |       Square142x142Logo.png
    |       Square150x150Logo.png
    |       Square284x284Logo.png
    |       Square30x30Logo.png
    |       Square310x310Logo.png
    |       Square44x44Logo.png
    |       Square71x71Logo.png
    |       Square89x89Logo.png
    |       StoreLogo.png
    |
    +---src
    |       main.rs
    |
    \---target
```   
---
폴더들에 대해 먼저 알아보겠습니다.   
* **src** : **<span style="color:blue">Front-end</span>** 소스들이 위치하게 됩니다. 웹과 관련된 소스 및 리소스를 포함하는 폴더입니다.   
* **src/assets** : 웹에서 사용할 에셋들을 저장할 수 있는 폴더입니다.   
* **src-tauri** : **<span style="color:blue">Back-end</span>** 소스들이 위치하게 됩니다. rust와 관련된 설정, 소스, 리소스등을 포함하는 폴더입니다.   
* **src-tauri/icons** : 각 OS에 아이콘들을 포함하고 있는 폴더입니다. 여기에 이미지들은 각 앱의 아이콘으로 사용이 됩니다.   
* **src-tauri/src** : 러스트의 소스 파일이 있는 폴더입니다.   
* **src-tauri/target** : 빌드시 debug/release 바이너리 파일이 생성되는 폴더입니다.   

---

주요 파일들에 대해 살펴보면,   
> **<span style="color:black">루트</span>**   

* **package.json** : 프로젝트 루트에 위치하고 있으며, 프로젝트에 필요한 패키지에 대한 정보를 담고 있습니다.

> **<span style="color:black">src 폴더</span>**   

* **index.html** : tauri에서 가장 먼저 호출하는 page입니다.   
* **main.js** : 여기에 html에서 사용할 javascript를 작성합니다.   
* **styles.css** : html에서 사용할 스타일을 정의합니다.   

> **<span style="color:black">src-tauri 폴더</span>**   

* **build.rs** : 프로젝트에 필요한 소스 파일을 컴파일하고 패키지를 빌드하기 직전에 실행합니다. (C라이브러리 구축, 호스트 시스템에서 C라이브러리 찾기, Rust 모듈 생성하기, 크레이트에 필요한 플랫폼 구성등의 작업 수행) [작업예시](https://doc.rust-lang.org/cargo/reference/build-script-examples.html)   
* **cargo.lock** : 프로젝트에 필요한 의존하고 있는 크레이트의 정확한 버전을 트랙킹하기 위한 파일입니다. (수정 금지)    
* **cargo.toml** : cargo로 만든 프로젝트의 설정 파일이며 패키지에 관한 정보 및 크레이트들을 관리합니다.    
* **tauri.conf.json** : tauri의 정보 및 권한 등을 설정할 수 있는 파일입니다.   

> **<span style="color:black">src-tauri/src 폴더</span>**   

* **main.rs** : 프로그램의 시작점이 되는 main 함수가 있는 파일입니다.     