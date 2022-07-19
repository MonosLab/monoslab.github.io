---
title: "[ Error ] Visual Studio C++ Error 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-07-15 06:00:00 +0900
categories: [정보, 에러]
tags: [Error, solved]
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
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 언어</span> 탭에서 <span style="background-color: #FFF5B1">준수 모드</span>를 <span style="background-color: #FFF5B1">아니오</span>로 선택하면 해결이 됩니다.</span>   

---
# <span style="color:red">***C4996***</span>   
> ***<span style="color:red">'GetVersionExW': deprecated로 선언되었습니다.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 구성 속성 > C/C++ > 전처리기</span> 탭에서 <span style="background-color: #FFF5B1">전처리기 정의</span>에 <span style="background-color: #FFF5B1">_CRT_SECURE_NO_DEPRECATE</span>을 추가하거나 소스코드에 #progma warning(disable:4996)을 선언하면 됩니다.</span>   

---
