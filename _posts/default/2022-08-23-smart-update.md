---
title: "[ Project ] Smart Update 설계 및 개발" 
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
프로그램 배포시 업데이트를 매번 프로그램에 맞게 커스트마이징하여 개발을 하였는데, 매번 불필요한 공수가 들어가게 되어 공용으로 사용 가능한 Smart한 Update 프로그램을 개발하였습니다.

---   

## Smart Update UI   

![SmartUpdateUI](https://monoslab.github.io/assets/img/posts/prj_update_ui.png)   

* 업데이트의 상단 이미지는 프로그램 실행시 이미지 파일에서 읽어 들여 처리합니다.   
* 업데이트가 진행 중인 경우에는 우측 하단의 버튼은 취소로 표시되며, 완료된 경우에는 확인 버튼으로 표시됩니다.   
  * 취소 : 업데이트를 진행 했던 파일들을 다시 복원하고 종료합니다.   
  * 확인 : 업데이트를 위해 백업했던 파일들을 모두 삭제합니다.   

---   

## Smart Update 흐름

![SmartUpdateFlow](https://monoslab.github.io/assets/img/posts/prj_update_flow.png)   

업데이트 방법은 크게 두가지로 구분이 되는데, 업데이트 프로그램을 직접 실행하여 처리하는 방법과 프로그램에 의해 실행이 되는 방법이 있습니다

### 직접 실행   
설정 파일(smartupdate.json)에서 도메인 정보와 버전 정보를 얻어올 수 있는 경로를 획득하여 직접 업데이트를 수행합니다.  업데이트시 업데이트 항목 중 현재 실행중인 프로세스가 있는 경우 해당 프로세스의 정보를 저장하고 프로세스를 종료시킵니다. 업데이트할 파일을 모두 다운로드 한 경우 업데이트시에 종료되었던 프로세스를 되살리고 업데이트 프로그램을 종료합니다. 만약 업데이트할 파일에 업데이트와 관련된 파일이 존재할 경우 업데이트 에이전트를 실행시켜서 업데이트의 마지막 작업(사용중이던 프로세스 재실행)을 위임합니다.    

### 앱을 통한 실행   
앱에서 버전 정보를 비교하고 SmartUCore.dll에서 업데이트 목록을 생성(udi.dat)하고 업데이트 파일(SmartUpdate.exe)을 실행합니다. 이후의 작업은 "직접 실행" 작업과 동일한 작업을 수행합니다.

---   

## 업데이트 STEP

![SmartUpdateStep](https://monoslab.github.io/assets/img/posts/prj_update_step.png)   

---   

## 설정 파일 및 업데이트 정보 파일   

### SmartUpdate.json 예시   

```json
{
    "lang": "ko",
    "preferFromFile": false,
    "preferFile": "MonoToolbox.json",
    "protocolDesc": "http(0), https(1)",
    "protocol": 0,
    "domain": "monoslab.github.io",
    "urlPath": "/component/install/{DOMAIN}/toolbox/",
    "dnlPath": "Not used. Prepare for future customization.",
    "port": 80,
    "attachPort": false,
    "versionFile": {
        "server": "ver.txt",
        "client": "ver.txt"
    },
    "serviceList": [{
            "name": "MonoService",
            "file": "MonoServiceNew.exe",
            "comment": "Monosoft 제품군에 대한 관리 프로그램입니다."
        },
        {
            "name": "MonoHwpDraft",
            "file": "MonoHwpDraftNew.exe",
            "comment": "Monosoft 한글실행기 서비스 프로그램입니다."
        }
    ]
}
```   
* lang : 기본 언어(SmartUpdate를 직접 실행할 경우 사용)
* preferFromFile : 환경정보를 다른 파일로 부터 얻어오는지에 대한 여부
* preferFile : preferFromFile이 true값을 가질때 환경정보를 얻어 올 파일명
* protocol : 프로토콜 (http의 경우 0, https의 경우 1을 설정)
* domain : 도메인
* urlPath : 버전 파일 또는 업데이트 파일을 다운로드할 경로
* dnlPath : 현재에는 사용하지 않음. 로컬의 특정 위치에 파일을 저장해야할 경우 사용.
* port : 사용 포트(http 기본 : 80, https 기본 : 443)
* attachPort : URL에 포트를 붙여서 사용할지 여부
* versionFile : 버전 파일
  * server : 서버의 버전 파일명
  * client : 로컬의 버전 파일명
* serviceList : 서비스 파일 정보
  * name : 서비스명
  * file : 서비스 파일명
  * comment : 서비스 설명

### udi.dat 예시   

```json
{
    "attachPort" : false,
    "domain" : "monoslab.github.io",
    "lang" : "ko",
    "list" : 
    [
        "MonoBrowser.exe",
        "MonoCapture.exe",
        "MonoCubro.exe",
        "MonoToolBox.exe"
    ],
    "locfile" : "D:\\00. Project\\git\\SmartUpdate2\\Release\\ver.txt",
    "port" : 80,
    "protocol" : 0,
    "raw" : "",
    "serviceList" : null,
    "tmpfile" : "C:\\Users\\mono\\AppData\\Roaming\\Monosoft\\Update\\ver.txt",
    "urlPath" : "/component/install/monoslab_github_io/toolbox"
}
```   
* attachPort : URL에 포트를 붙여서 사용할지 여부
* domain : 도메인
* lang : 설정 언어
* list : 업데이트할 파일의 목록
* locfile : 로컬 버전 파일
* port : 사용 포트(http 기본 : 80, https 기본 : 443)
* protocol : 프로토콜 (http의 경우 0, https의 경우 1을 설정)
* raw : 서버로 부터 다운 받은 버전 데이터 (raw 값이 null인 경우 아래의 tmpfile을 이용)
* serviceList : 업데이트 할 서비스 리스트
* tmpfile : 서버로 부터 다운 받은 버전 파일
* urlPath : 업데이트 파일을 다운로드할 경로

---
