---
title: "[ RUST ] 타우리(Tauri) #1" 
sub: post
author: Kwangsoo Seo
date: 2023-02-21 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, WebView, Tauri]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post23.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post23)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  

# 타우리(Tauri)란?   

타우리는 Rust 와 HTML을 이용하여 Webview를 통해 데스크탑용 애플리케이션을 구축하는데 사용되는 툴킷입니다. 데스크탑에 설치되어 있는 OS의 기본 Webview를 이용하기 때문에 배포파일의 크기가 다른 웹앱(크로미엄, 일렉트론 등)에 비해 매우 작으며, OS에서 Webview를 관리하기 때문에 별도로 업데이트 및 유지 관리가 필요하지 않습니다. 또한 Rust에 의해 컴파일되어 별도의 런타임을 제공하지 않아도 독립적으로 실행이 가능합니다.

## <span style="color:red">NOT</span>

> **<span style="color:black">VM이나 가상화 환경이 아니다.</span>**    
> Webview OS 애플리케이션을 만들기 위한 어플리케이션 툴킷입니다.   

> **<span style="color:black">단순한 커널 랩퍼(Wrapper)가 아니다.</span>**   
> 직접적으로 OS에 대한 시스템 호출을 위해 WRY와 TAO를 이용합니다.   

## <span style="color:red">라이선스</span>
> **<span style="color:black">MIT 또는 Apache-2.0 라이선스 적용.</span>**

---

# 아키텍쳐   

![Tauri_Architecture](https://monoslab.github.io/assets/img/posts/tauri_architecture.png)   
타우리는 런타임, 매크로, 유틸리티 및 API를 포함하는 툴킷으로  컴파일시에 tauri.conf.json 파일에서 필요한 기능들을 확인하고 앱 생성시에 기능들을 추가하게 됩니다.
* **tauri-runtime**   
하위 수준의 webview 라이브러리와 타우리 사이의 런타임 라이브러리입니다.   
* **tauri-macros**   
Tauri-codegen 크레이트를 활용하여 컨텍스트, 핸들러 및 명령을 위한 매크로를 만듭니다.   
* **tauri-utils**   
많은 곳에서 재사용되고, 환경 파일 구문 분석, 플랫폼 트리플(Platform triples) 감지, CSP(Content Security Policy) 와 Assets 관리 등과 같은 유용한 유틸리티를 제공하는 공통 코드입니다.   
* **tauri-runtime-wry**   
이 라이브러리는 인쇄, 모니터 감지와 윈도우 관련 작업과 같은 WRY를 위한 시스템 수준의 특별한 상호작용을 할 수 있게 합니다.   
* **tauri-build**   
cargo에 필요한 어떤 특수 기능을 조작하기 위해 빌드시 매크로를 적용합니다.   
* **tauri-codegen**   
앱과 시스템 트레이를 위한 아이콘을 포함하여 Assets을 삽입, 해시 및 압축한다. 컴파일시 tauri.con.json을 분석하고 Config 구조체를 생성합니다.   
* **[업스트림(Upstream)](#f1) 라이브러리**   
  * **WRY** <span style="color:green;font-weight:bold">Cross-platform Webview rendering library</span> : Webview와의 인터페이스를 유지하고 관리합니다.   
  * **TAO** <span style="color:green;font-weight:bold">Cross-platform application window creation library</span> : 애플리케이션 윈도우를 만들고 관리합니다.   

---

# 타우리 도구   

## 기본 도구   
* **API (JavaScript / TypeScript)**   
Webview가 백엔드 활동을 위해 호출/수신 하도록 프론트엔드 프레임워크에 임포트하기 위해 CommonJS(cjs)와 ECMAScript Module(esm) JavaScript 엔드포인트(요청을 받아 응답을 제공하는 서비스를 사용할 수 있는 지점)을 생성하는 TypeScript 라이브러리 입니다. 호스트에 웹뷰의 메시지 전달을 위해 사용합니다.   
* **Bundler (Rust / Shell)**   
MacOS, Windows 및 Linux 등의 플랫폼용 타우리 앱을 빌드하기 위한 라이브러리입니다. (모바일 플랫폼도 지원하기 위해 준비중에 있습니다.)   
* **cli.rs (Rust)**   
이 Rust 실행 파일은 MacOS, Windows 및 Linux의 CLI가 필요로 하는 요구 활동에 대한 모든 인터페이스를 제공합니다.   
* **cli.js (JavaScript)**   
각 플랫폼의 npm 패키지를 생성하기 위해 napi-rs를 사용하여 cli.rs를 래핑한 래퍼입니다.   
* **create-tauri-app (JavaScript)**   
선택한 프론트엔드 프레임워크의 새 프로젝트를 신속하게 구축할 수 있도록 해주는 툴킷입니다.

## 추가 도구
* **tauri-action**   
어떠한 설정도 없이 모든 플랫폼(MacOS, Linux 및 Windows)을 위한 아주 기본적인 타우리 앱을 생성할 수 있습니다. 만약 프로젝트에 Tauri 파일이 포함되어 있지 않으면 컴파일시에 파일을 생성합니다.
* **tauri-vscode**   
이 프로젝트는 몇 가지 유용한 기능으로 Visual Studio Code 인터페이스를 향상시킵니다.   
* **vue-cli-plugin-tauri**   
vue-cli 프로젝트에 Tauri를 매우 빠르게 설치할 수 있습니다.   

---

<i class="fas fa-tape" aria-hidden="true"></i> <span style="color:black;font-weight:bold">본문 내용 출처</span> : https://tauri.app/v1/references/architecture/   



---

<i class="fas fa-comment-dots" aria-hidden="true"></i> <span style="color:black;font-weight:bold">각주</span>   
<b id="f1">업스트림(Upstream) :</b> 클라이언트나 로컬기기(일반적으로 컴퓨터나 모바일기기)에서 서버나 원격 호스트(서버)로 전송되는 데이터 또는 보내는 것을 의미합니다. [↩](#a1)