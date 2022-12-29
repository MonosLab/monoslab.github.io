---
title: "[ Flutter ] Electron과 비교" 
sub: post
author: Kwangsoo Seo
date: 2022-12-16 06:00:00 +0900
categories: [프로그래밍, Flutter, Electron]
tags: [Flutter 3.0, 플러터 3.0, Electron, 일렉트론, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post20.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post20)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  
# Flutter vs Electron   

## 두 프레임워크의 주요 장/단점 비교   
데스크탑 애플리케이션 개발시 두 프레임워크의 장/단점은 아래의 표와 같습니다.   

|  |Flutter|Electron|   
|---|---|---|
|장점|모바일 개발을 위한 훌륭한 솔루션|데스크탑에서 웹 애플리케이션을 개발하기 위한 훌륭한 솔루션|
|단점|데스크탑용 위젯과 자료는 아직까지는 미흡함|어플리케이션의 실행 파일이 큼<br>많은 메모리/리소스를 사용함<br>경우에 따라서는 긴 시작 시간과 버벅임이 발생.|

## 구동하는 언어와 기술   
> **<span style="color:black">Flutter</span>**   
> * 언어 : Dart, C/C++ 등   
> * 기술   
>   * Null에 대한 안전한 지원.   
>   * C 라이브러리를 이용하여 이식성을 높임.   
>   * Dart VM을 통한 JIT(Just-in-time; 개발 모드에서만 사용)과 네이티브 앱을 위한 AOT(ahead-of-time) 컴파일러 지원.   
>   * 컴파일러를 통해 기계어 코드를 생성.   

> **<span style="color:black">Electron</span>**
> * 언어 : Javascript, HTML 등
> * 기술
>   * 자바스크립트 V8 엔진 사용.
>   * 노드JS(Node.js)를 기반으로 자바스크립트·HTML·CSS를 사용.
>   * V8은 JIT(Just -In-Time Compilation )로 JavaScript를 내부적으로 컴파일 하여 실행 속도를 높임.

## 진입 장벽   
> **<span style="color:black">Flutter</span>** : 높음   
> * Dart 언어를 배워야하며, 플러그인을 개발하기 위해서는 C/C++언어를 배울 필요가 있음.   

> **<span style="color:black">Electron</span>** : 낮음
> * 자바스크립트를 이용한 웹 개발자가 데스크탑 애플리케이션을 만들기 위해 새로운 기술이나 언어를 배울 필요가 없음.   

## 개발 속도   
> **<span style="color:black">Flutter</span>** : 빠름   
> * 다양한 플러그인을 활용하여 빠르게 개발을 할 수 있음.   

> **<span style="color:black">Electron</span>** : 빠름
> * 인터프리터 방식을 사용하는 자바스크립트를 이용하기 때문에 디버깅이 쉽고 애플리케이션을 빠르게 구현할 수 있음.   
> * 일반적으로 웹 애플리케이션의 디자인, 비즈니스 로직등을 재사용할 수 있기 때문에 개발 시간이 줄어듦.   

## 크로스 플랫폼 지원   
> **<span style="color:black">Flutter</span>** : 지원   
> * 각 OS별 네이티브 API 이용시 기능 제약이나 별도의 버전을 가져가야할 수도 있음.   

> **<span style="color:black">Electron</span>** : 부분 지원
> * 각 OS별 기능 제약이나 별도의 버전을 유지할 필요 없이 하나의 애플리케이션으로 개발 가능. (모바일용은 지원하지 않음)   

## Native API 지원   
> **<span style="color:black">Flutter</span>** : 지원   
> * 데스크탑의 시스템 자원 활용시 지원되지 않는 것은 plugin 개발을 통해 지원 가능.   

> **<span style="color:black">Electron</span>** : 지원
> * 각 OS별 런타임을 제공해서 시스템의 자원을 손쉽게 활용 가능함.   

## Third-Party 지원   
> **<span style="color:black">Flutter</span>** : 지원   
> * pub.dev 사이트를 통해 패키지를 제공하고 있으며 각 패키지들의 샘플 소스가 잘되어 있어 활용하기 용이함.   
> * 패키지에 원하는 기능이 "올인원"인 경우는 드물어서 다수개의 패키지를 설치하여 사용.   
> * 패키지의 기능 수정을 위해서는 C/C++을 알아야 하며, plugin 개발 방법도 함께 숙지하여야 함.   
> * 간단한 프로그램을 만들더라도 다수개의 패키지를 설치/관리하여야 하는 상황이 발생함.   

> **<span style="color:black">Electron</span>** : 지원
> * Node JS의 빌트인 모듈과 써드파티 모듈의 이용 가능.   
> * Node JS를 통해 파일시스템에 접근하고 네트워크 리소스도 제약없이 이용.   
> * NPM에 등록된 대부분의 모듈도 이용 가능.   

## 웹어셈블리(Wasm)
브라우저를 염두에 두고 특별히 설계된 기계 코드를 위한 새로운 이진 형식입니다. WebAssembly[^footnote_1]로 컴파일된 앱은 성능 저하 없이 JavaScript와 함께 실행할 수 있습니다.   
> **<span style="color:black">Flutter</span>** : 지원   
> **<span style="color:black">Electron</span>** : 지원

## DevTools(디버깅 툴) 지원
> **<span style="color:black">Flutter</span>** : 지원   
> **<span style="color:black">Electron</span>** : 지원

## Third-Party 지원   
> **<span style="color:black">Flutter</span>** : 안전   
> * 데스크톱의 경우 컴파일러를 통해 애플리케이션을 생성하여 안전함. 모바일의 경우 보안 및 인증 플러그인을 제공.   

> **<span style="color:black">Electron</span>** : 취약   
> * 자바스크립트를 통해 파일 시스템 및 사용자 쉘 등에 접근이 가능하기 때문에 보안에는 취약할 수 있으며, 이에 대한 대책이 필요함.   

## 종속성   
> **<span style="color:black">Flutter</span>** : 낮음   
> * 불필요한 코드와 종속성 제거 작업이 원활함.   

> **<span style="color:black">Electron</span>** : 높음   
> * 좀 더 복잡한 작업을 위해서는 Angular, React 또는 Vue와 같은 타사 JavaScript 프레임워크를 활용해야 함.(이러한 프로임워크에도 종속성이 있어서 webpack을 구성하여 모든 종속성을 번들로 묶어주어야 함)   

## Isolate(격리)
> **<span style="color:black">Flutter</span>** : 지원   
> * Dart에서 각 스레드의 자체 메모리로 격리되어 있음.
> * 서로 다른 격리간 통신은 포트를 통한 값을 전달하는 방식.   

> **<span style="color:black">Electron</span>** : 지원   
> * 작업자 스레드를 사용하여 JavaScript를 병렬로 실행.

## 빌드 툴 및 인스톨러 제공   
> **<span style="color:black">Flutter</span>** : 일부 제공   
> * 빌드 툴은 제공하나 인스톨러는 제공하지 않음.   

> **<span style="color:black">Electron</span>** : 제공   
> * 데스크톱 애플리케이션을 손쉽게 패키징 가능. 또한 autoUpdater라는 구성 요소를 이용하면 애플리케이션 자동 업데이트 기능도 사용할 수 있음.   
> * 각 플랫폼 빌드시 해당 플랫폼에서만 컴파일 가능.

## 성능   
> **<span style="color:black">Flutter</span>**   
> * 실행 속도 : 빠름   
> * 실행 파일 크기 : 38Mb 이상   
> * 리소스(CPU, GPU) 및 메모리 사용 : 낮음   

> **<span style="color:black">Electron</span>**   
> * 실행 속도 : 느림   
> * 실행 파일 크기 : 100Mb 이상   
> * 리소스(CPU, GPU) 및 메모리 사용 : 높음   

---

# 결론

## Flutter
아직까지 Flutter의 Desktop은 걸음마 단계이며, 조금 더 시간이 흐른 후 다시 검증이 되어야 할 것입니다. 현재까지의 검증 결과로 보면 Flutter는 메모리 공간과 설치 크기가 더 작습니다. 이러한 수치는 특히 Dart가 계속해서 최적화됨에 따라 개선될 가능성이 있습니다. 
필요한 최소한의 개발자 구성으로 여러 플랫폼을 쉽게 타겟팅할 수 있습니다. Flutter의 플랫폼 통합 및 FFI(Foreign Function Interface)[^footnote_2]는 환상적인 솔루션입니다. 
Dart의 null에 대한 안전성과 Wasm의 잠재적 통합은 Flutter를 더 낫은 프레임워크로 만들어 갈 것입니다. 

## Electron
Electron은 배포 파일의 크기와 메모리 사용량이 큽니다. 또한 배포 파일 생성시 포함하는 모듈, 로드되는 시기 및 애플리케이션 패키징 방법에 대해 더 주의를 기울여야 합니다. 
Flutter와 마찬가지로 Wasm 통합은 특정 작업에서의 성능을 크게 향상시킬 수 있습니다. 

---

위의 내용을 참고하여 내가 하고자 하는 프로젝트에 어떠한 것이 더 적합할지 선택하여야합니다. 데스크탑용 프로그램을 개발하고자 할 경우 두 프레임워크 모두 좋은 도구가 될 것입니다. 🙂
위에서 언급했듯이 Flutter 데스크탑 지원은 아직까지는 안정적이지 않습니다. MacOS의 경우 iOS와 유사하여 현재로서는 MacOS 데스크탑에서는 최고의 경험을 제공하지만 Windows와 Linux도 그리 뒤지지는 않습니다. 그리고 아직까지는 Electron의 일부 기본 기능(알림, 작업 표시줄 아이콘 등)은 데스크탑용 Flutter에서는 사용할 수 없습니다. 하지만 곧 Flutter에서도 이용이 가능하게 될 것입니다. 

---   

[^footnote_1]: WebAssembly는 최신 웹 브라우저에서 실행할 수 있는 새로운 유형의 코드입니다. 기본 성능에 가깝게 실행되고 C/C++, C# 및 Rust와 같은 언어를 제공하는 압축 바이너리 형식의 저수준 어셈블리 유사 언어입니다. 웹에서 실행할 수 있도록 컴파일 대상이 있습니다. 또한 JavaScript와 함께 실행되도록 설계되어 둘 다 함께 작동할 수 있습니다. WebAssembly JavaScript API를 사용하여 WebAssembly 모듈을 JavaScript 앱에 로드하고 둘 사이에서 기능을 공유할 수 있습니다. 이를 통해 WebAssembly 코드를 작성하는 방법을 모르더라도 동일한 앱에서 WebAssembly의 성능과 기능, JavaScript의 표현력과 유연성을 활용할 수 있습니다.
[^footnote_2]: 한 프로그래밍 언어에서 다른 프로그래밍 언어의 함수를 호출하기 위한 인터페이스를 말합니다. C언어는 역사가 오래 되었을 뿐만 아니라 그 동안 축적된 코드베이스(code base)가 풍부하므로 많은 언어들이 C언어로 작성한 코드를 사용하기 위한 FFI를 가지고 있습니다.