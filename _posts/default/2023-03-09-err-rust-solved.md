---
title: "[ Error ] Rust Error/Warning 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2023-03-09 06:00:00 +0900
categories: [정보, 에러, 경고]
tags: [Error, Warning, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post25.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post25)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# <span style="color:red">***A snake case name***</span>   
> ***<span style="color:red">warning: crate `xxx` should have a snake case name</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">Cargo.toml</span> 파일에서 name을 <span style="background-color: #FFF5B1">스네이크 케이스(소문자 + '_' 조합)</span>로 변경하거나, <span style="background-color: #FFF5B1">main.rs</span>의 상단 부분에 <span style="background-color: #FFF5B1">#![allow(non_snake_case)]</span> 한 줄을 추가하면 해결이 됩니다.    

---

