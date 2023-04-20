---
title: "🏆 C++의 왕좌를 이어받을 차세대 언어는?"
sub: post
author: Kwangsoo Seo
date: 2023-04-20 12:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, 차세대, 언어]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post30.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post30)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 화려하게 한 세대를 풍미했던 C++의 왕좌를 이을 다음 언어는?   
20여년 넘게 C++ 개발자로 살아오다보니, 다음에 C++의 자리를 차지할 언어에 대한 관심이 많습니다. 그래서 오늘은 C++ 왕좌를 이어받을 차세대 언어들에 대해 정리 해 보고자 합니다.

아래의 3가지 차세대 언어가 그 후보들인데요. 아직까지는 먼저 세상에 나온 러스트가 다른 언어에 비해 우위를 차지하고 있습니다. 하지만 구글의 힘(Go와 Dart 언어의 성공적인 개발 노하우)을 업은 카본과 정통 C++에 현대적인 언어의 장점을 점진적으로 추가하여 진화하고 있는 Cpp프론트도 그 영향력을 무시할 수 없을 것으로 판단이 됩니다.

> **<span style="color:black">러스트 (Rust)</span>**
> * 개발 : 러스트 재단 (Graydon Hoare; 그레이든 호아레)   
> * 목표 : C/C++와 동등한 수준의 속도를 달성하면서 안전성, 동시성을 목표로 함.   
> * 깃허브 : https://github.com/rust-lang
> * 버전 : Rust 1.68.2 (2023.04.20 현재)

> **<span style="color:black">카본 (Carbon)</span>**
> * 개발 : 구글   
> * 목표 : C++을 대체하거나 기존 레거시 C++코드와의 상호 운용성 달성을 목표로 함.   
> * 깃허브 : https://github.com/carbon-language
> * 버전 : 2024년(또는 2025년) 1.0 출시 예상

> **<span style="color:black">Cpp프론트 (Cppfront)</span>**
> * 개발 : ISO C++ (Herb Sutter; 허브 셔터)
> * 목표 : C++의 10배 더 간단하고, 안전하며, 도구를 사용하기 쉽게 구현하는 것을 목표로 함.   
> * 깃허브 : https://github.com/hsutter
> * 버전 : 미정

## 러스트 (Rust)   
[✔러스트](https://monoslab.github.io/posts/rust/){:target="_blank"}는 기존 자료를 참고해주세요.

## 카본 (Carbon)  
구글에서 C++ 생태계 위에 구축을 하고 있는 카본은 C++과 유사하게 설계하고 있습니다.
* 성능을 중요시하는 소프트웨어.
* 실용적이고 안전한 메커니즘.
* 최신 플랫폼, 하드웨어 아키텍처 환경을 고려.
* 완만한 학습 곡선.
* 비슷한 표현식.
* 기존 C++ 코드와의 원활한 상호 운용성 및 마이그레이션.

아직까지는 컴파일러(또는 툴체인)가 없고 인터프리터를 사용해서 맛보기만 가능합니다.   
[맛보기](https://carbon.compiler-explorer.com/){:target="_blank"}   

## Cpp프론트 (Cppfront)   
C++은 1985년 발표된 이래 최근의 C++20 버전까지 매우 오랫동안 전세계적으로 많이 사랑받고 있는 언어입니다. 20여년 전까지만 해도 프로그래밍의 기초를 다지기 위해 C/C++ 언어를 배웠었습니다. 이제는 어쩌면 그 역활을 Cpp프론트가 대신할지도 모르겠네요.
* 기존 C++과 완전히 호환되는 새로운 구문(syntax)으로 간결하고 안전하게 코딩.
* 메모리 안전을 기본으로 지원.
* C++20 모듈과 C++23 import std를 기본으로 탑재.
* 현대화된 언어로 탈바꿈. (아래의 예제 참조)

```cpp   
main: () -> int = {  
    vec: std::vector<std::string> = ("hello", "cppfront");  
    view: std::span = vec;  
  
    for view do :(inout str: _) = {  
        len := decorate(str);  
        println(str, len);  
    }  
}
```   
