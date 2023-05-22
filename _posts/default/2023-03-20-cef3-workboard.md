---
title: "[ Project ] Workboard 설계 및 개발" 
sub: post
author: Kwangsoo Seo
date: 2023-03-20 06:00:00 +0900
categories: [프로젝트]
tags: [프로젝트, CEF3, 워크보드, Workboard]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post27.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post27)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---   
# Workboard 설계   
회사에서 웹 서비스를 하나의 브라우저에 함께 보여주는 프로젝트를 진행하게 되었고, 방대한 웹 소스를 병합하여 처리하는 것보다 브라우저에서 웹 서비스간 연계 작업을 진행하는 것이 비용면에서 절감할 수 있을 것으로 판단되어 워크보드를 개발하게 되었습니다. 워크보드에는 톡, 그룹웨어, 메뉴를 3단 구성하고 이들간에 데이터 연동을 할 수 있도록 개발하고 있습니다.

---   

## Workboard UI   

![WorkboardUI](https://monoslab.github.io/assets/img/posts/prj_workboard_ui.png)   

* Workboard는 Chromium Embedded Framework의 기술을 이용합니다.   
* 좌측부터 톡(채팅), 그룹웨어, 메뉴 순으로 구성이 됩니다.   
  * 톡(채팅)의 경우는 고정이며, 그룹웨어의 경우는 때에 따라서 기간계 시스템으로 대체될 수 있습니다.   
  * 메뉴는 서버의 설정된 메뉴로 유동적으로 구성이 가능하며, 톡과 그룹웨어(또는 기간계 시스템)에 명령을 전달하거나 특정 액션을 취할 수 있습니다. (예 : 프로그램 실행, 자동화 작업 등)   

---   

## Workboard 주요 기능   
* Javascript Event 처리   
  * 알림창(대화, 쪽지, 메일, 기타 기간계 시스템 알림 등) 표시 및 링크   
* 웹 캐시 삭제   
* 단축키 지원   
* 원격제어   
* 화면캡쳐   
* 파일 업/다운로드   
* 로컬의 특정 프로그램 실행   
* Workboard 업데이트   

---
