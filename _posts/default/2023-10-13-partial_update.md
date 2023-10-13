---
title: "[ Project ] 사전 업데이트(Pre-patch) 설계 및 개발" 
sub: post
author: Kwangsoo Seo
date: 2023-10-13 06:00:00 +0900
categories: [프로젝트]
tags: [프로젝트, pre-patch, 업데이트, update, partial, download, 분할 다운로드]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post38.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post38)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---   
# <b>사전 업데이트(Pre-patch) 설계</b>   
대용량의 파일을 업데이트 하는 경우에 한번에 접속이 몰려 네트워크의 트래픽을 유발할 수 있어 백그라운드에서 사전에 업데이트 파일을 다운로드 하여 처리하기 위해 2가지의 방안을 가지고 설계하였으며 두 방안 모두 분할 다운로드(Partial download) 방법을 사용하였습니다.
2가지 방안(다중 다운로드와 순차적 다운로드)의 장/단점은 아래와 같으며, 프로그램의 복잡도를 최소화하고 이어받기가 용이한 (2안)을 채택하여 프로젝트에 적용하였습니다.

---   

## <b>장/단점</b>   
### <b>⭕ (1안) 다중 다운로드</b>   
1. 전체 파일 사이즈를 얻어와서 빈 파일을 생성하고 분할 다운로드한 데이터를 씀   
2. 실패한 경우 실패 목록에 저장하고 다음 범위의 데이터를 요청함.   
3. 마지막까지 데이터 요청이 끝난 경우 다시 실패한 목록을 요청하여 다운로드 함.   
4. 모두 다운로드가 된 경우 요청한 프로그램에 통지.   

<b>[장점]</b>   
✅ 다중 다운로드가 가능(멀티쓰레드 이용)   

<b>[단점]</b>   
✅ 분할 데이터 중 완료된 데이터에 대한 관리가 필요함.   
✅ 완료되지 않고 중단된 파일은 재 시작시 다시 다운로드 하여야 함.(이를 관리하게 되면 프로그램 복잡도가 상승함)   

![PartailDownload_001](https://monoslab.github.io/assets/img/posts/prj_mpdn_001.png)   

### <b>⭕ (2안) 순차적 다운로드 (선택)</b>   
1. 임시 폴더에 파일을 생성하고, 분할 다운로드한 데이터를 씀.   
2. 실패한 경우 일정 시간 경과 후 다시 재 요청을 함. (http에러에 대한 처리 필요; 4xx, 5xx 에러의 경우 설정에 따라 중지 유무 판단)   
3. 모두 다운로드가 된 경우 요청한 프로그램에 통지.   

<b>[장점]</b>   
✅ 분할 데이터에 대한 관리가 필요 없음.   
✅ 완료되지 않고 중단된 파일을 이어 받기에 용이함.   

<b>[단점]</b>   
✅ 순차적으로 다운로드해야 함. (멀티쓰레드 사용 불가)   

![PartailDownload_002](https://monoslab.github.io/assets/img/posts/prj_mpdn_002.png)   

---   

## <b>버전 비교 및 업데이트</b>   
![PartailDownload_003](https://monoslab.github.io/assets/img/posts/prj_mpdn_003.png)   
업데이트는 사전 업데이트(백그라운드에서 진행)와 일반적인 업데이트(즉시 진행) 두가지를 병행하여 작동합니다. 사전 업데이트가 있는 경우에는 사용자가 인지하지 못하게 백그라운드에서 다운로드를 진행하고 일반적인 업데이트는 즉시 서버 또는 사전 다운로드 된 파일로 업데이트가 진행됩니다. 

위의 그림을 살펴보면 그룹웨어 서버에는 Pre-patch용과 patch용 버전 파일 및 업데이트 파일이 존재하며 두 파일의 용도는 아래와 같습니다.   

<span style="font-size:20px; background-color:#fcf9e1"> Pre-patch </span>
* ver.msp : 사전 다운로드를 위한 버전 정보 파일입니다.   
* files : 사전에 업데이트 될 파일을 로컬의 임시 폴더에 저장해 두기 위한 파일입니다.   

<span style="font-size:20px; background-color:#fcf9e1"> Patch </span>   
* ver.ms : 업데이트 버전 정보를 담고 있습니다.   
* files : 현재의 버전 정보(ver.ms)와 일치하는 업데이트 파일입니다. 사전 업데이트 파일이 있는 경우 업데이트 시점에 사전 업데이트 파일로 교체됩니다. (수동으로 작업)   

프로그램 실행시 서버의 업데이트 폴더의 버전 파일(ver.ms)과 로컬에 있는 버전 파일(ver.ms)을 먼저 비교를 하게 됩니다. 버전이 동일한 경우에는 프로그램을 실행하고 일치하지 않는 경우에는 사전 다운로드된 파일의 유무에 따라 아래의 방법으로 업데이트를 진행하게 됩니다.   

<span style="font-size:20px; background-color:#fcf9e1"> 사전 다운로드 된 파일이 있는 경우 </span>   
* 사전 다운로드된 파일이 있는 경우 완료가 된 파일에 한해 toolbox 폴더 아래로 이동   
* 사전 다운로드가 완료되지 않은 파일은 서버에서 직접 다운로드 진행   

<span style="font-size:20px; background-color:#fcf9e1"> 사전 다운로드 된 파일이 없는 경우 </span>   
* 서버에서 직접 다운로드 진행

프로그램이 정상적으로 실행된 이후에는 주기적으로 서버에 사전 업데이트가 있는지 체크하고 사전 업데이트가 필요한 경우에는   
* 사전 다운로드 관리 파일(dn.json)을 생성하고 관리   
  * 파일 명   
  * 버전 정보   
  * 총 파일의 크기 및 현재 다운로드된 파일의 크기   
  * 다운로드 완료 유무   
* Pre-patch 파일 다운로드   
를 진행하게 됩니다.   

<span style="font-size:20px; background-color:#fcf9e1"> 전체 흐름 요약 </span>   
![PartailDownload_004](https://monoslab.github.io/assets/img/posts/prj_mpdn_004.png)   

---

## <b>다운로드 실패시 처리 방안</b>  
분할 다운로드(Partial download)시 실패한 경우에는 dn.json파일의 현재 다운로드 완료한 크기로 부터 정보를 얻어 이전에 다운로드 완료한 부분(S<sub>0</sub>)에서 부터 S<sub>1</sub>까지  재 요청하여 다운로드를 수행합니다. 이러한 방법을 통해 프로그램이 재시작하더라도 이어받기가 가능해 집니다.
![PartailDownload_005](https://monoslab.github.io/assets/img/posts/prj_mpdn_005.png)   
