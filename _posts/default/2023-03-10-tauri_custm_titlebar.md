---
title: "[ RUST ] Tauri 타이틀바 커스트마이징" 
sub: post
author: Kwangsoo Seo
date: 2023-03-10 06:00:00 +0900
categories: [정보, 에러, 경고]
tags: [RUST, WebView, Tauri]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post26.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post26)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 타이틀바 커스트마이징(🚑 작업중)   

## tauri.conf.json

타이틀바를 커스트마이징 하기 위해서는 타우리 설정 파일에서 윈도우에 관련된 몇가지 권한을 허용해줘야 합니다.   
```   
"tauri": {
  "allowList": {
    ...
    "window": {
        "all": false,
        "close": true,
        "hide": true,
        "show": true,
        "maximize": true,
        "minimize": true,
        "unmaximize": true,
        "unminimize": true,
        "startDragging": true
      }
  }
  ...
  "windows": [
      {
        ...
        "decorations": false
      }
    ]
}
```   
---

