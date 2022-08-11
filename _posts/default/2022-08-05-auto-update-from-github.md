---
title: "[ VC++ ] GitHub을 이용하여 애플리케이션 자동 업데이트" 
sub: post
author: Kwangsoo Seo
date: 2022-08-05 06:00:00 +0900
categories: [VC++, 코딩]
tags: [GitHub, 자동, 업데이트, auto, update]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post9.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post9)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---   
# GitHub을 이용하여 애플리케이션 자동 업데이트   
   🗞 <span style="color:red">작성 중 💦<span>   
많은 개발자들로 부터 널리 사용되고 있는 리포지토리인 GitHub을 이용하여 애플리케이션의 릴리스 버전을 자동으로 관리하는 방법에 대해 알아보려고 합니다.

## ■ GitHub api를 이용하는 방법
GitHub에 저장소를 생성하고 나면 저장소 우측에 Release 링크가 있는 것을 확인하실수 있습니다.(아래의 이미지 참조)   
![CreateNewRelease](https://monoslab.github.io/assets/img/posts/create_a_new_release.png)   
Create a new release를 클릭한 후 아래와 같은 방법으로 적절하게 내용을 작성하고 파일을 추가해 줍니다.
파일은 업데이트 파일 외에도 버전을 관리할 수 있는 파일(version.dat)도 함께 올려 줍니다.
- https://api.github.com/repos/<사용자 이름>/<리포지토리 이름>/releases
- json 데이터에서 버전정보 확인 후 다운로드 방식

1. api를 이용하여 버전 정보 확인
2. raw 데이터를 읽어 파일을 저장

## ■ RAW 데이터를 이용하는 방법
생성한 저장소에 파일을 직접 호출 또는 다운로드 하는 방법입니다.
- https://raw.githubusercontent.com/<사용자 이름>/<리포지토리 이름>/master/<파일명>
- 버전 파일을 먼저 다운로드하여 업그레이드가 필요한 파일만 다운로드 하는 방식.

업데이트 파일을 관리할 서버는 위의 방법으로 구축하였으면, 절반의 작업은 끝났습니다. 다음은 업데이트를 적용할 수 있도록 애플리케이션에 업데이트를 위한 코드를 작성하여야 합니다.

1. GitHub 서버로 부터 version.dat 파일을 다운로드
2. 로컬 버전 정보와 비교
3. 다운로드 목록 생성 및 다운로드

---   