---
title: "[ 사례 ] 바이러스 오진 사례" 
sub: post
author: Kwangsoo Seo
date: 2022-08-12 06:00:00 +0900
categories: [정보, 에러]
tags: [virus, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post11.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post11)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# <span style="color:BurlyWood">***알약(Alyak)에서의 바이러스 오진 사례***</span>   
> ***<span style="color:red">case 1 :</span>***   
<span style="color:#555555">Visual Studio 2017에서 컴파일 후 알약에서 바이러스로 오진을 함.</span>  

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경전</span> : 
* 구성 \> 구성 속성 \> 일반 \> MFC 사용 \> 공유 DLL에서 MFC 사용
* 구성 \> 구성 속성 \> C/C++ \> 코드 생성 \> 런타임 라이브러리 \> 다중 스레드DLL(/MD)  

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경후</span> :  
* 구성 \> 구성 속성 \> 일반 \> MFC 사용 \> 정적 라이브러리에서 MFC 사용
* 구성 \> 구성 속성 \> C/C++ \> 코드 생성 \> 런타임 라이브러리 \> 다중 스레드(/MT)  

---

> ***<span style="color:red">case 2 :</span>***   
<span style="color:#555555">Visual Studio 2022에서 컴파일 후 알약에서 바이러스로 오진을 함.</span>   
* 구성 \> 구성 속성 \> 일반 \> MFC 사용 \> 정적 라이브러리에서 MFC 사용
* 구성 \> 구성 속성 \> C/C++ \> 코드 생성 \> 런타임 라이브러리 \> 다중 스레드(/MT)    

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경전</span> : 
* Win32(x86) 플랫폼의 Release 버전으로 컴파일시 바이러스 감지 (탐지명 : Gen:Variant.Fragtor.533355)

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경후</span> :  
* x64 플랫폼의 Release 버전으로 컴파일 후 바이러스 감지되지 않음.

---

> ***<span style="color:red">case 3 :</span>***   
<span style="color:#555555">Visual Studio 2022에서 컴파일 후 알약에서 바이러스로 오진을 함.</span>   

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경전</span> : 
* Visual Studio 2022 version 17.10.4 사용   
* x64 플랫폼의 Release 버전으로 컴파일시 바이러스 감지 (탐지명 : Gen:Variant.Tedy.618274)

><span style="color:Orange;font-weight:bold;text-decoration:underline">변경후</span> :  
* Visual Studio 2022 version 17.10.5 업그레이드 후  x64 플랫폼의 Release 버전으로 컴파일시 발생하지 않음.   
  (참조 : 구글링시 Visual Studio tool의 버전에 따라 발생되는 경우가 있다는 글이 간혹 보임.)   

---
