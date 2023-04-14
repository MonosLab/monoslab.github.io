---
title: "우당탕탕 타우리 #004💬 프로젝트의 설정 파일들"
sub: post
author: Kwangsoo Seo
date: 2023-04-12 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, Windows, Setting]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post29.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post29)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 프로젝트 설정   
오늘은 프로젝트의 설정 파일들(tauri.conf.json, cargo.toml)에 대해 알아 보겠습니다.

## cargo.toml   
이 매니페스트 파일은 프로젝트에서 어떤 Rust Crate에 의존하는지 정의하며, 앱에 대한 부가 정보를 설정할 수 있습니다. 크게 package, build-dependencies, dependencies, features로 구분이 되고, 라인의 첫 글자가 #으로 시작하면 주석으로 간주합니다.
```
[package]
name = "Monoslab"
version = "0.0.1"
description = "Premium application"
authors = ["monoslab<my@mail.com>"]
license = "MIT"
keywords = ["cargo", "toml", "monoslab"]
repository = "https://github.com/rust-lang/cargo/"
edition = "2021"

[build-dependencies]
tauri-build = { version = "1.2", features = [] }

[dependencies]
tauri = { version = "1.2", features = ["api-all", "icon-png", "system-tray"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"

[features]
# this feature is used for production builds or when `devPath` points to the filesystem
# DO NOT REMOVE!!
custom-protocol = ["tauri/custom-protocol"]
```
위에서 부터 하나씩 살펴 보면,   
**[package]**에는 프로젝트의 일반적인 정보들이 들어가 있습니다. 각각의 항목에 대한 내용은 아래와 같습니다.   
* name : 프로젝트 이름   
* version : 프로그램의 버전   
* description : 프로젝트에 대한 간략한 설명   
* keyword : crates.io에 라이브러리를 등록했을 때 다른 사용자들이 키워드로 쉽게 검색할 수 있도록 여기에 키워드를 설정합니다. 키워드는 최대 5개까지 가능하며, 키워드의 시작은 문자, 숫자 또는 _로 시작하여야 하며 최대 20자까지 가능합니다.   
* authors : 프로그램 저작자들의 리스트. (반드시 저작자 이름과 메일주소가 형식에 맞게 작성되어야 합니다.)
* license : 저작권 종류를 표시 (MIT, BSD, GPL, LGPL 등)   
* repository : 패키지 소스의 레파지토리 URL 입력(입력하지 않아도 상관없음)
* edition : 패키지가 컴파일되는 Rust 에디션에 영향을 미치는 선택적 값입니다. (예시 처럼 2021이면 Cargo.toml 버전과의 호환성을 2021버전으로 간주합니다.)   

**[build-dependencies]**는 빌드 스크립트의 빌드에 필요한 creates들의 이름과 버전을 등록합니다.   
**[dependencies]**는 프로젝트가 사용하는 crates들의 이름과 버전이 등록합니다. 여기에 crate를 등록해야지만 프로젝트에서 사용할 수 있습니다. 필요한 create은 https://crates.io 에서 찾을 수 있습니다.   

> 참고 : dependencies에 version에 =이 붙는 경우 항상 동일한 버전(정확한 버전)의 의존성 create가 다운로드 됩니다. 그렇지 않은 경우(=이 붙지 않는 경우)에는 1.2버전의 가장 최신 버전이 1.2.0.10일 경우 1.2.0.10 버전으로 다운로드가 이루어 집니다.    
>   예) version = "=1.2" 

## tauri.conf.json   
이 JSON 파일은 앱의 다양한 맞춤 설정을 할 수 있도록 돕습니다.   
```   
{
  "build": {
    "beforeDevCommand": "npm run build",
    "beforeBuildCommand": "npm run dev",
    "devPath": "../src",
    "distDir": "../src",
    "withGlobalTauri": true
  },
  "package": {
    "productName": "Monoslab",
    "version": "0.0.1"
  },
  "tauri": {
    "allowlist": {
      "all": true
      },
      "window": {
        "all": true
      }
    },
    "bundle": {
      "active": true,
      "icon": [
        "icons/32x32.png",
        "icons/128x128.png",
        "icons/128x128@2x.png",
        "icons/icon.icns",
        "icons/icon.ico"
      ],
      "identifier": "com.monoslab.test",
      "copyright": "Copyright (c) Monoslab 2023. All rights reserved."
      "targets": "all"
    }
  }
}
```   
tauri.conf.json은 아래의 3가지 오브젝트로 구성되어 있습니다.
* build : 타우리를 빌드시 필요한 경로 또는 명령어를 추가 할 수 있습니다.
* package : 앱의 이름과 버전 정보를 입력합니다.
* tauri : 프로젝트에서 사용할 API의 허용 범위를 지정하며, 리소스, 번들, 보안, 업데이트, 윈도우 설정 등을 지정할 수 있습니다.
* 참고 : 공홈에서는 plugins를 포함하여 4개로 정의하고 있는데, 어디에서도 샘플을 찾을 수가 없어서 향후 해당 정보를 알게 되면 내용을 수정하도록 하겠습니다.   

tauri.conf.json의 설정은 너무 자료가 방대하여 자세한 설명은 [여기](https://tauri.app/v1/api/config/){:target="_blank"}를 참조하기 바랍니다. 여기에서는 많이 사용하는 것 위주로 한해 대략적으로 설명하도록 하겠습니다.   
**build**의 beforeBuildCommand는 `tauri build`를 돌릴 때 이 명령어를 실행하도록 설정합니다. 마찬가지로 beforeDevCommand는 
`tauri dev`를 돌릴 때 이 명령어가 실행됩니다.   
**package**의 productName에는 앱의 이름을 지정할 수 있으며, version에는 앱의 버전을 지정합니다.   
**tauri**는 앱에서 사용할 허용 목록을 설정하고 어디까지 사용할지에 대한 권한을 부여합니다.  "allowlist"의 "all"의 값이 true인 경우 타우리의 모든 API에 대한 사용을 모두 허용한다는 뜻입니다. 만약 all이 false인 경우 허용할 목록을 직접 작성하야 합니다. 아래는 간단한 예입니다.   
```   
"allowlist": {
      "all": false,
      "fs": {
        "all": true,
      },
      ...
```   
모든 타우리 API에 대해 사용을 허용 하지 않았기 때문에 사용을 하고자 하는 API의 허용 목록을 따로 작성을 하여야 합니다. 위의 예시에서는 타우리에서 제공하는 모든 API 중에서 오직 파일 시스템(fs) API에 관련된 것만 모두 사용하겠다는 의미입니다.
"bundle"의 경우 앱의 식별 정보와 리소스 등을 지정할 때 사용합니다. 
