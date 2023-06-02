---
title: "코드사인 (CodeSign, EV CodeSign)"
sub: post
author: Kwangsoo Seo
date: 2023-06-02 06:00:00 +0900
categories: [정보, 유틸]
tags: [코드사인, CodeSign, EV-CodeSign]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post33.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post33)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 코드사인
개발된 프로그램을 온라인으로 배포할 경우 이 프로그램은 특정 회사에서 개발하여 안정성을 입증할 수 있다라는 것을 증명하기 위해 인증을 받는 디지털 서명 인증 방법입니다. 개발자가 유익한 프로그램을 만들 수 도 있고, 해킹 이나 바이러스를 만들 수 도 있으므로, 최종 사용자는 이것을 다운받아 실행하기 전에는 다운받아 사용해도 안전한지 알 수 없습니다. 그래서 코드사인을 하여, 다운받아 설치하게 될 프로그램이 어떤 회사에서 인증한 프로그램이다라는 것을 인지시켜주어 사용자가 안심하고 받을 수 있게 하는게 목적입니다. 온라인에서 다운받은 파일 실행시 "알 수 없는 게시자" 경고가 뜨는 경우는 코드사인이 되어 있지 않아 나타나는 메시지입니다.

## 코드사인 인증서의 종류
코드사인은 CodeSign과 Ev CodeSign 두 종류의 인증서로 인증을 할 수 있습니다.   
아래는 간단한 비교표입니다.   

||CodeSign|EV CodeSign|   
|:---|:---:|:---:|   
|암호화된 디지털 서명|✔|✔|   
|엄격한 심사 프로세스||✔|   
|스마트 스크린 필터 경고창 즉시 해제||✔|   
|보안토큰을 사용한 이중 인증||✔|   

## 코드사인을 위한 준비물   
코드 서명 작업을 위해서는 각 플랫폼에 해당하는 signing tool을 사용하여야 합니다.   

* 코드 서명을 위한 준비물(공통)
  * 서명툴
    * Microsoft 서명(signtool.exe)
      * [Microsoft SDK 다운로드](https://developer.microsoft.com/ko-kr/windows/downloads/sdk-archive){:target="_blank"}
      * [signtool.exe 설명](https://learn.microsoft.com/ko-kr/dotnet/framework/tools/signtool-exe){:target="_blank"}
    * Java 서명(jarsigner)
      * [Java JDK 다운로드](https://www.oracle.com/technetwork/java/javase/archive-139210.html){:target="_blank"}
      * [Jarsigner 설명](https://github.com/Jamling/jarsigner){:target="_blank"}
  * 코드서명 인증서 : .pfx 파일 또는 EV CodeSign 인증서가 설치된 USB Token
  * USB Token Driver : EV CodeSign인 경우 인증서 제공업체의 별도 클라이언트 툴<br>(예 : SafeNet Client Tool)
  * 서명 대상 Application

## 코드사인 하기   
### CodeSign   
* Command창 입력 예시   
  <span style="color:red">[signtool_path]</span>signtool.exe sign /f <span style="color:red">[pfx file]</span> /p <span style="color:red">[password]</span> /du "https://monoslab.github.io" /fd sha256 /tr http://sha256timestamp.ws.symantec.com/sha256/timestamp <span style="color:red">[target_file]</span>
  * <span style="color:red">[signtool_path]</span> : signtool.exe가 있는 폴더의 전체 경로
  * <span style="color:red">[pfx file]</span> : 인증서(pfx) 파일이 있는 전체 경로(pfx 파일 포함)
  * <span style="color:red">[password]</span> : 비밀번호 (/p [password] 옵션을 사용하지 않을시 수동으로 비밀번호 입력 필요)
  * <span style="color:red">[target_file]</span> : 서명할 파일의 전체 경로(서명 파일 포함)   
* InstallShield 설정 예시   
  * 설정 위치 : InstallShield > Installation Designer > PREPARE FOR RELEASE > Releases > Builds > SingleImage > Signing
  * Digital Certificate Information 항목을 선택하고 아래의 항목을 선택 또는 입력합니다.   
    ✔ Use a file (.pfx)   
    \<ISProjectFolder\>\prvkey\monoslab.pfx   
    * pfx 파일의 전체 경로 입력, 예제에서의 설정은 인스톨쉴드 프로젝트 폴더내에 prvkey 폴더 아래에 pfx 파일이 있는 경우입니다.

### EV CodeSign   
* Command창 입력 예시   
  <span style="color:red">[signtool_path]</span>signtool.exe sign /a /s my /du "http://www.naonsoft.com" /fd sha256 /tr http://timestamp.digicert.com <span style="color:red">[target_file]</span>
  * <span style="color:red">[signtool_path]</span> : signtool.exe가 있는 폴더의 전체 경로
  * <span style="color:red">[target_file]</span> : 서명할 파일의 전체 경로(서명 파일 포함)   
* InstallShield 설정 예시
  * 설정 위치 : InstallShield > Installation Designer > PREPARE FOR RELEASE > Releases > Builds > SingleImage > Signing
  * Digital Certificate Information 항목을 선택하고 아래의 항목을 선택 또는 입력합니다.   
    ✔ Use a certificate store   
    Certificate Store Name : Personal   
    Certificate Store Location : User   
    Certificate Subject : MONOSLAB CO.,LTD   

