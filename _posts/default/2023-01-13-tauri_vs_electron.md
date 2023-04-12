---
title: "우당탕탕 타우리 #000💬 Tauri와 Electron 비교" 
sub: post
author: Kwangsoo Seo
date: 2023-01-13 06:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, 타우리, Electron, 일렉트론, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post21.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post21)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  
# Tauri vs Electron   

Tauri와 Electron은 모두 웹 기술을 기반으로 크로스 플랫폼을 지원하고 있으며,  Desktop Application을 구축하기 위한 훌륭한 프레임워크입니다. HTML, CSS 및 Javascript를 이용하여 Windows, MacOS, Linux에서 실행할 수 있는 애플리케이션을 구축할 수 있습니다. 하지만 이 두 프레임워크에는 아래와 같은 차이점이 존재합니다.   
**<span style="color:black">Tauri</span>**는 프론트엔드는 거의 모든 프론트엔드 프레임워크와 호환이 되고, 백엔드에서는 Rust 기반 엔진을 사용하고 CLI(Command line interface)은 Node.js를 사용합니다.  추후에는 백엔드로 Rust뿐만 아니라 Python, C++, Deno도 지원할 예정이라고 하니 점점 더 각광받는 프레임워크가 될 것 같습니다.    
**<span style="color:black">Electron</span>**는 Node.js를 기반으로 Chromium엔진을 사용하여 사용자 인터페이스를 렌더링하고 기본 API에 대한 액세스를 제공하여 데스크탑 애플리케이션 구축에 대한 전통적인 접근 방식을 제공하고 있습니다. 이를 통해 애플리케이션을 빠르고 쉽게 구축할 수는 있지만 Tauri 애플리케이션보다 느리고 안전하지 못한 단점이 있습니다.   
아직 두 프레임워크 모두 모바일은 지원하고 있지 않으나, Tauri의 경우는 모바일 빌더를 지원하겠다고 언급하였기 때문에 조만간 그 실체를 볼 수 있을 것 같습니다.    

## 두 프레임워크의 주요 장/단점 비교   

>**<span style="color:black">Tauri</span>**는 Rust 엔진을 이용하여 각 플랫폼의 WebView를 사용합니다. 이때문에 Tauri 애플리케이션은 기존의 웹 기반 애플리케이션보다 더 작고 빠르며 보안적으로 더 안전합니다.   
>**<span style="color:black">Electron</span>**은 Chromium 엔진을 이용하여 사용자 인터페이스를 렌더링하고 시스템의 기본 API에 대한 액세스를 제공합니다. 이를 통해 Electron은 웹개발 기술을 토대로 애플리케이션을 빠르고 쉽게 구축할 수 있습니다.   

>**<span style="color:black">Tauri</span>**는 Rust를 이용하여 웹에서 시스템의 네이티브 API와 기능을 이용할 수 있는 플러그인을 개발할 수 있으며, 기존의 웹앱이 가진것보다 더 강력하고 유연하게 개발을 할 수 있습니다.    
>**<span style="color:black">Electron</span>**은 C/C++을 이용하여 웹에서 시스템의 네티이브 API와 기능을 이용할 수 있는 플러그인을 개발할 수 있으나 기본 플러그인을 구축하는 것은 웹 기반 기술을 사용하는 것보다 더 어렵습니다.   

>**<span style="color:black">Tauri</span>**는 다양한 플랫폼을 지원하는 내장 빌드 시스템으로 외부 도구나 서비스 없이도 사용자에게 배포할 설치 프로그램을 손쉽게 만들 수 있습니다.   
>**<span style="color:black">Electron</span>**도 다양한 플랫폼용 설치 프로그램을 만드는 데 사용할 수 있는 내장 빌드 시스템을 제공하지만, Tauri에서 제공하는 것만큼 유연하지 않으며 외부 도구나 서비스를 사용해야 할 수도 있습니다.   

>**<span style="color:black">Tauri</span>**는 2019년 12월 Pre-Alpha로 처음 출시되었고 2022년 6월에 정식 버전이 출시되었습니다. 이렇다 보니 커뮤니티가  적으며, 국내에선 잘 알려지지 않은 편입니다.    
>**<span style="color:black">Electron</span>**은 2013년 Atom의 프레임워크로 시작하여 역사가 길고 많은 사람이 사용하다 보니 커뮤니티 및 오픈소스가 많습니다.   

>**<span style="color:black">Tauri</span>**는 설치 프로그램 용량이 3Mb 이상으로 크기가 적습니다.     
>**<span style="color:black">Electron</span>**은 설치 프로그램 용량이 최소 120Mb 이상으로 큽니다. 또한 시스템 리소스 사용율이 Tauri 보다 높습니다.   

>**<span style="color:black">Tauri</span>**는 Rust 언어를 사용하여 애플리케이션을 만들기 때문에 보안적으로 안정적입니다.   
>**<span style="color:black">Electron</span>**은 Node.js 기반의 애플리케이션이다 보니 보안에 취약합니다.  웹앱 소스의 노출이 쉽기 때문에 이를 통한 공격이 용이하며, 보안에 더 각별히 신경을 써야합니다.   

>**<span style="color:black">Tauri</span>**는 백엔드 작업을 위해서는 Rust라는 언어에 대한 이해도가 있어야 하며, 이로 인해 높은 진입 장벽이 있습니다. 하지만 앞서 언급했듯이  Python, C++, Deno도 지원할 예정이어서 향후에는 진입 장벽이 낮아질 수 있을것 같습니다.   
>**<span style="color:black">Electron</span>**은 웹 개발자가 새로운 언어를 익힐 필요없이 데스크탑 프로그램을 만들 수 있고, 진입 장벽이 낮으며, 그만큼 개발 속도가 빠른 장점을 가집니다.   

간단하게 두 프레임워크의 장/단점을 윈도우 기준으로 요약하자면...

|  |Tauri|Electron|   
|---|---|---|
|실행파일 크기|3Mb 이상|120Mb 이상|
|메모리 사용량|80Mb 이상|120Mb 이상|
|AppBackend|Rust|Node.js 런타임|
|랜더링|윈도우 : WebView2(Edge)<br>리눅스 : WebKitGTK<br>MacOS : WebKit|윈도우 : Chromium<br>리눅스  : Chromium<br>MacOS  : Chromium|
|보안|안전|앱 소스 코드를 쉽게 탈취할 수 있어 보안에 매우 취약|
|업데이트|내장 Updater 사용<br>(업데이트 서버 운영 필요)|electron-updater 패키지 사용<br>(GitHub 릴리즈에서 직접 바이너리를 업데이트)|
|커뮤니티|국내 : 없음<br>해외 : 적음|커뮤니티와 오픈소스가 많음|

---

# 결론   
Tauri와 Electron 모두 웹 기술을 이용하여 크로스 플랫폼 애플리케이션을 구축하기에 좋은 프레임워크입니다. 하지만 Tauri는 Electron 보다 더 나은 보안성과 다양한 프레임워크들과의 호환성 및 배포 파일 용량과 리소스 점유율 등 다방면에서 좋은 면모를 보여주고 있습니다. 다만 현재까지의 Tauri의 가장 큰 걸림돌은 Rust언어에 대한 이해도가 있어야 한다는 점입니다.    
지금까지의 내용을 근거로 가까운 미래에는 Electron을 Tauri가 밀어내고 더 좋은 위치에 있지 않을까 조심스레 점쳐봅니다.