---
title: "[ Error ] Visual Studio C++ Error 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-07-15 06:00:00 +0900
categories: [정보, 에러]
tags: [Error, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post4.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post4)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# <span style="color:red">***C2760***</span>   
> ***<span style="color:red">구문 오류: '식별자'은(는) 예기치 않은 토큰입니다. 필요한 토큰은 '형식 지정자'입니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 언어</span> 탭에서 <span style="background-color: #FFF5B1">준수 모드(Conformance mode)</span>를 <span style="background-color: #FFF5B1">아니오(No)</span>로 선택하면 해결이 됩니다.</span>   

---
# <span style="color:red">***C4996***</span>   
> ***<span style="color:red">'GetVersionExW': deprecated로 선언되었습니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 전처리기</span> 탭에서 <span style="background-color: #FFF5B1">전처리기 정의</span>에 <span style="background-color: #FFF5B1">_CRT_SECURE_NO_DEPRECATE</span>을 추가하거나 소스코드에 #progma warning(disable:4996)을 선언하면 됩니다.</span>   

---
# <span style="color:red">***LNK4022***</span>   
> ***<span style="color:red">LNK4022: 'xxx' 기호에만 일치하는 것을 찾을 수 없습니다.</span>***   
<span style="color:black">해결방법 : xxx 함수가 사용하는 라이브러리(예 MFC 라이브러리 등) 중에도 존재하는 경우 발생되는 오류입니다. 예를 들면 GetResult가 MFC의 기본라이브러리에도 존재한다고 가정하면, 소스상에 GetResult라는 함수를 재정의하면 문제가 발생됩니다. 이 경우 함수명을 다른 이름으로 바꾸어주면 문제가 해결됩니다.</span>   

---   
# <span style="color:red">***MIDL2311***</span>   
> ***<span style="color:red">error MIDL2311: statements outside library block are illegal in mktyplib compatability mode : [ ]</span>***   
<span style="color:black">해결방법 : 이전의 ocx 소스 코드를 불러 올 경우 odl 파일에서 #include 헤더 구문이 library 바깥에 위치하면 해당 오류가 발생됩니다. #include 헤더 구문을 library 안쪽으로 이동시키면 해결이 됩니다.</span>   

```cpp   
#include <olectl.h>
#include <idispids.h>

[ uuid(18C89C39-B40A-410B-9659-064000FCCCDB), version(1.0),
  helpfile("Avata.hlp"),
  helpstring("Avata ActiveX Control module"),
  control ]
library AVATALib
{
...[생략]...
```   
에서   
```cpp   
[ uuid(18C89C39-B40A-410B-9659-064000FCCCDB), version(1.0),
  helpfile("Avata.hlp"),
  helpstring("Avata ActiveX Control module"),
  control ]
library AVATALib
{
	#include <olectl.h>
	#include <idispids.h>
...[생략]...
```   
으로 #include의 위치를 이동시키면 됩니다.