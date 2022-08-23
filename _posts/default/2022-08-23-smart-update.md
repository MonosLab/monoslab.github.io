---
title: "[ Project ] Smart Update 설계" 
sub: post
author: Kwangsoo Seo
date: 2022-08-23 06:00:00 +0900
categories: [프로젝트]
tags: [프로젝트, 자동, 업데이트, smart, update]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post12.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post12)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---   
# Smart Update 설계   
<span style="color:red">( 작성 중 💦 )</span>   
프로그램 배포시 업데이트를 매번 프로그램에 맞게 커스트마이징하여 개발을 하였는데, 매번 불필요한 공수가 들어가는것 같아서 이번에 공용으로 사용 가능한 Smart한 Update 프로그램을 개발하려고 합니다.

---   

## Smart Update UI   

![SmartUpdateUI](https://monoslab.github.io/assets/img/posts/prj_update_ui.png)   

* 업데이트 목록 중 서비스 파일이 있는 경우 프로그레스바는 서비스 파일 * 4 만큼의 진행 구간이 생긴다.   
* 업데이트의 상단 이미지는 프로그램 실행시 이미지 파일에서 읽어 들여 처리한다.   
* 업데이트가 진행 중인 경우에는 우측 하단의 버튼은 취소로 표시되며, 완료된 경우엔느 확인 버튼으로 표시된다.   
  * 취소 : 업데이트를 진행 했던 파일들을 다시 복원하고 종료한다.   
  * 확인 : 업데이트를 위해 백업했던 파일들을 모두 삭제한다.   

---   

## Smart Update 흐름

![SmartUpdateFlow](https://monoslab.github.io/assets/img/posts/prj_update_flow.png)   

### 앱을 통한 실행   
1. Update용으로 제작된 동적 라이브러리를 이용하여 업데이트 서버의 버전 정보를 읽어와 업데이트 목록을 생성한다.   
1. 업데이트 프로그램을 호출하여 업데이트가 필요한 파일을 서버로 부터 다운로드한다.   
1. 다운로드 목록중 서비스 또는 업데이트 프로그램과 관련된 항목이 있는 경우 목록을 생성한다.   
3.1. 서비스 : 서비스 중지 -> 서비스 업데이트 -> 서비스 시작   
3.2. 업데이트 프로그램 관련 항목이 존재 : 업데이트 에이전트 실행   

### 직접 실행   
1. 업데이트 프로그램에서 지정된 경로의 버전 정보와 비교 하여 업데이트가 필요한 파일을 다운로드한다.   
1. 다운로드 목록중 서비스 또는 업데이트 프로그램과 관련된 항목이 있는 경우 목록을 생성한다.   
2.1. 서비스 : 서비스 중지 -> 서비스 업데이트 -> 서비스 시작   
2.1. 업데이트 프로그램 관련 항목이 존재 : 업데이트 에이전트 실행   

---   