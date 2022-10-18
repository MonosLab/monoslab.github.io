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
# <span style="color:red">***ISO C++ 오류***</span>   
> ***<span style="color:red">error : \<LanguageStandard\> 요소에 잘못된 값 "stdcpp20"이(가) 있습니다</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 언어</span> 탭에서 <span style="background-color: #FFF5B1">C++ 언어 표준</span>을 <span style="background-color: #FFF5B1">ISO C++ 최신 초안 표준(/std:c++latest)</span>로 선택하면 해결이 됩니다. 이렇게 하면 현 Visual Studio에서 사용하고 있는 가장 최근의 C++ 표준 라이브러리를 사용할 수 있게 됩니다.</span>   

# <span style="color:red">***C2760***</span>   
> ***<span style="color:red">구문 오류: '식별자'은(는) 예기치 않은 토큰입니다. 필요한 토큰은 '형식 지정자'입니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 언어</span> 탭에서 <span style="background-color: #FFF5B1">준수 모드(Conformance mode)</span>를 <span style="background-color: #FFF5B1">아니오(No)</span>로 선택하면 해결이 됩니다.</span>   

---
# <span style="color:red">***C2850***</span>   
> ***<span style="color:red">'PCH 헤더 파일': 파일 범위에서만 사용할 수 있습니다. 중첩된 구문에는 사용할 수 없습니다.</span>***   
<span style="color:black">추가 메시지 : PCH 경고: 전역 범위에 헤더 중지가 있어야 합니다. IntelliSense PCH 파일이 생성되지 않았습니다.   
해결방법 : stdafx.h 또는 pch.h파일의 선언 중 세미콜론(;)이 빠져 있는지 확인합니다.(예를 들면 구조체, 배열 등 선언후 세미콜론이 올바르게 사용되었나 확인합니다.)</span>  

---
# <span style="color:red">***C4996***</span>   
> ***<span style="color:red">'GetVersionExW': deprecated로 선언되었습니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 전처리기</span> 탭에서 <span style="background-color: #FFF5B1">전처리기 정의</span>에 <span style="background-color: #FFF5B1">_CRT_SECURE_NO_DEPRECATE</span>을 추가하거나 소스코드에 #progma warning(disable:4996)을 선언하면 됩니다.</span>   

---
# <span style="color:red">***LNK2026***</span>   
> ***<span style="color:red">LNK2026: 모듈이 SAFESEH 이미지에 대해 안전하지 않습니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > 링커 > 고급</span> 탭에서 <span style="background-color: #FFF5B1">이미지에 안전한 예외 처리기 포함</span>을 <span style="background-color: #FFF5B1">아니요(/SAFESEH:NO)</span>로 선택하면 해결이 됩니다.</span>     

---
# <span style="color:red">***LNK4022***</span>   
> ***<span style="color:red">LNK4022: 'xxx' 기호에만 일치하는 것을 찾을 수 없습니다.</span>***   
<span style="color:black">해결방법 : xxx 함수가 사용하는 라이브러리(예 MFC 라이브러리 등) 중에도 존재하는 경우 발생되는 오류입니다. 예를 들면 GetResult가 MFC의 기본라이브러리에도 존재한다고 가정하면, 소스상에 GetResult라는 함수를 재정의하면 문제가 발생됩니다. 이 경우 함수명을 다른 이름으로 바꾸어주면 문제가 해결됩니다.</span>   

---   
# <span style="color:red">***LNK4070***</span>   
> ***<span style="color:red">warning LNK4070: /OUT: .EXP의 xxx.dll 지시문이 '~yyy.dll' 출력 파일 이름과 다릅니다. 지시문이 무시됩니다.</span>***   
<span style="color:black">해결방법 : .def 파일의 LIBRARY : "xxx"를 LIBRARY : "yyy"로 변경해주면 해결됩니다.</span>   

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
