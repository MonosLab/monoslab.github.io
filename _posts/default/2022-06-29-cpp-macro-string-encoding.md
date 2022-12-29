---
title: "[ VC++ ] 문자열 인코딩 변환 매크로 주의 사항(W2A, A2W, T2A, A2T)" 
sub: post
author: Kwangsoo Seo
date: 2022-06-29 00:00:00 +0900
categories: [프로그래밍, C++]
tags: [VC++, macro, encoding, string]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post2.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post2)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **문자열 인코딩 변환 매크로 사용시 주의**  
유니코드(Unicode)와 아스키(Ascii) 상호간 문자열을 변활할때 ATL3.0에서 제공하는 문자열 변환 매크로를 많이 사용하게 되는데, <span style="background-color: #FFF5B1">W2A, A2W, T2A, A2T의 경우 반복문에서 사용을 하게 되면 스택오버플로우(Stack overflow)가 발생</span>하게되어 문자열이 종종 깨지게 됩니다. 이는 W2A, T2A, A2W, A2T가 <span style="background-color: #FFF5B1">변환 과정에서 내부 스택을 사용하여 변환하기 때문에 발생</span>이 됩니다.   
ATL7.0에서는 이를 개선하기 위해 CW2A, CA2W, CW2CA, CA2CW, CT2A, CA2T, CT2CA, CA2CT를 제공하는데, 이를 이용하는 것이 좋습니다. 이는 내부에 128 바이트의 버퍼를 가지고 있고 이를 넘어서는 경우에는 힙에 메모리를 할당하여 반복문안에 사용하더라도 스택오버플로우가 발생되지 않는다고 합니다.

---

# **ALT 3.0과 ATL 7.0 차이 비교**  

|기존 ATL 3.0 매크로 변환|새로운 ATL 7.0 변환 클래스|  
| --- | --- |  
|스택에 메모리를 할당.|작은 문자열은 스택, 큰 문자열은 힙에 메모리 할당.|  
|문자열은 함수가 종료 될 때 해제.|문자열은 변수가 범위를 벗어날 때 해제.|  
|예외 처리기에서 사용할 수 없음.|예외 처리기에서 사용할 수 있음.|  
|반복문에 사용하기가 적합하지 않음.<br>함수가 종료될 때까지 메모리 사용이 증가함.|반복문에 사용 가능.<br>반복문 내에서 반복될 때 메모리가 해제됨.|  
|큰 문자열에 대한 문제 발생.<br>(스택 공간에 제한적)|큰 문자열에도 문제가 없음.|  
|보통 USES_CONVERSION 정의가 필요.|USES_CONVERSION 정의가 필요 없음.|  
|OLE의 의미가 OLE2ANSI 정의에 따라 달라짐.|OLE는 W로 항상 동일.|  
