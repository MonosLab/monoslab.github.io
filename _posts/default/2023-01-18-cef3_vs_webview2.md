---
title: "[ VC++ ] CEF3와 WebView2 비교" 
sub: post
author: Kwangsoo Seo
date: 2023-01-18 06:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, CEF3, WebView2, Chromium, Edge, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post22.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post22)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  
# CEF3와 WebView2 비교   

CEF3와 WebView2는 모두 Chromium 기반이지만 두 프레임워크는 약간의 차이를 보이고 있습니다.

## 통신 방법

> **<span style="color:black">CEF3</span>**    
> * "Javascript binding"을 통한 애플리케이션과 통신.   
> * MessageRouters를 이용하여 cefQuery로 통신.   
> * OnWebKitInitialized에서 javascript 함수(window.chrome.webview.postMessage() 함수 생성)를 구현하여 통신.   

> **<span style="color:black">WebView2</span>**   
> * "Javascript binding" 후 호출로 애플리케이션과의 통신을 위한 방법은 제시되어 있으나, 현재까지는 문제가 있어 사용하지 않는것 같음.   
> * window.chrome.webview.postMessage()로 애플리케이션 통신.(JSON, 문자열 등)

---

## Javascript Injection 및 실행   

CEF3와 WebView2 모두 가능합니다.

---

## 유지 보수적인 측면   

> **<span style="color:black">CEF3</span>**    
> * 장점 : 윈도우에 설치된 크롬 엔진과는 별개로 엔진을 운용함.<br>웹과 통신하기 위한 인터페이스를 자유롭게 만들 수 있음.   
> * 단점 : Chromium 엔진 업데이트시 많은 비용이 소요됨. 배포파일 용량이 큼.(120Mb 이상)   

> **<span style="color:black">WebView2</span>**   
> * 장점 : Nuget을 이용하여 엔진 업데이트시 적은 비용으로 처리가 가능.<br>MS Edge를 렌더링 엔진으로 사용하기 때문에 배포파일의 용량이 적음.    
> * 단점 : 정형화된 통신 방법으로만 통신이 가능 함. (인터페이스를 자유롭게 만들 수 있는지는 확인이 필요.)<br>윈도우에 설치된 Edge를 렌더링 엔진으로 사용하고 있어 영향을 받음.<br>Edge 업데이트 유무에 따라 기능의 차이 발생 가능.   

---

# 결론   
커스트마이징이 많이 필요한 경우에는 WebView2를 이용하기 보다는 CEF3를 이용하는 것이 자유도 측면에서는 훨씬 유리한 것으로 판단이 됩니다. 모든 것이 그렇듯 상황에 따라 장/단점을 따져 적절하게 프로젝트에 이용하는 것이 답입니다.