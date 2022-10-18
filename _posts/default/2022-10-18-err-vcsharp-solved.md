---
title: "[ Error ] Visual Studio C# Error 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-10-18 06:00:00 +0900
categories: [정보, 에러]
tags: [Error, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post15.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post15)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# <span style="color:red">***cs1617***</span>   
> ***<span style="color:red">csc : error cs1617: '8.0'은(는) /langversion의 유효한 옵션이 아닙니다. '/ langversion:?'를 사용하여 지원되는 값을 나열하세요.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 빌드 > 고급</span>에서 <span style="background-color: #FFF5B1">언어 버전</span>을 <span style="background-color: #FFF5B1">C# 최신 부 버전(최신)</span>으로 선택하면 해결이 됩니다. 이렇게 하면 현 Visual Studio에서 사용하고 있는 가장 최근의 C# 언어 버전을 사용할 수 있게 됩니다.</span>   
