---
title: "[ ComfyUI ] CuiBox 소개"
sub: post
author: Kwangsoo Seo
date: 2024-12-05 08:00:00 +0900
categories: [A.I., ComfyUI]
tags: [ComfyUI, AI, 인공지능, CuiBox, Windows, Desktop]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post49.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post49)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# CuiBox   
<span style="color:blue">CuiBox</span>는 윈도우(Windows) 환경 하에서 ComfyUI를 좀 더 편리하게 사용하기 위해 만들고 있는 프로그램입니다. 최근에 ComfyUI에서 일렉트론을 이용한 Desktop App을 출시하고 있는 것으로 알고 있습니다. 하지만 방향이 약간 다른 관계로 CuiBox 프로젝트는 계속 진행하려고 합니다. 그럼 CuiBox에 대해 간단하게 살펴 보겠습니다.   

# 설치 방법
1. [https://github.com/comfyanonymous/ComfyUI/releases](https://github.com/comfyanonymous/ComfyUI/releases){:target="_blank"}에서 최근 버전의 ComfyUI_windows_portable_nvidia.7z 파일을 다운로드 합니다.   
2. [https://github.com/MonosLab/CuiBox](https://github.com/MonosLab/CuiBox){:target="_blank"}에서 Code > Download Zip을 클릭하여 다운로드 합니다.   
3. ComfyUI_windows_portable_nvidia.7z 파일을 적당한 위치에 압축 해제 합니다.   
4. CuiBox에서 다운로드한 zip 파일을 압축 해제 후 ComfyUI_windows_portable 폴더 아래에 붙여 넣습니다.
5. CuiBox.exe 파일을 실행합니다.   
![image](https://raw.githubusercontent.com/MonosLab/CuiBox/master/images/cuibox.gif)   

# 설정   
환경 설정은 CuiBox.json 파일을 직접 수정하시거나, 트레이 메뉴를 이용하여 변경하실 수 있습니다. 먼저 CuiBox.json 파일을 이용하는 방법을 알려드리겠습니다.   
## CuiBox.json   
```json   
{
	"lang": "ko",
	"mode_comment": "h(horizontal) or v(vertical)",
	"mode": "horz",
	"run_comment": "cpu, gpu (* for Nvidia)",
	"run": "cpu",
	"auto_update": true
}
```   
* lang : 언어는 현재 한국어(ko), 영어(en), 중국어(zh), 일본어(ja), 베트남어(vi)를 제공하고 있습니다.
* mode : 콘솔 창의 배치 옵션입니다. 오른쪽(horz)이나 아래(vert)에 배치할 수 있습니다.
* run : NVIDIA 그래픽 카드를 사용하고 있는 경우 gpu를 그 외에는 cpu를 사용하여 ComfyUI를 이용할 수 있습니다.
* auto_update : CuiBox를 자동 업데이트 할지에 대한 옵션입니다. 기본은 true입니다. (이 부분은 트레이 메뉴에서 제공하지 않습니다.)

## 트레이 메뉴
작업표시줄 우측에 트레이 아이콘에서 ⓒ 모양의 아이콘(CuiBox 아이콘)에  마우스 커서를 위치시키고 우측 버튼을 누르면, 트레이 메뉴가 나타나며 구성은 아래와 같습니다.   
![run_mode](https://monoslab.github.io/assets/img/posts/comfyui/tray_menu_1.png)  ![lang_opt](https://monoslab.github.io/assets/img/posts/comfyui/tray_menu_2.png)   
* 열기 : 숨겨진 CuiBox를 다시 열기 위한 메뉴입니다. 트레이 아이콘을 더블 클릭하는 효과와 동일합니다.   
* 윈도우 시작시 자동 실행 : 윈도우가 시작될 때 CuiBox를 자동으로 실행되도록 하는 옵션입니다.   
* 초기화 : CuiBox를 초기 윈도우 사이즈로 변경합니다. (향후 제거 예정)   
* 변경된 설정 적용 : custom nodes 설치 후 cuibox(comfyui)를 재시작합니다.   
* 실행 모드 : CuiBox.json의 mode 설정을 CPU 또는 GPU로 변경합니다. (설정 후 변경된 설정 적용 필요)   
* 언어 선택 (Language) : CuiBox.json의 lang 설정을 ko, en, zh, ja, vi로 변경합니다. (설정 후 변경된 설정 적용이 필요하지 않음)   

# 마무리   
아직은 계속 개발을 진행하여 가는 과정이므로, 향후에는 어떠한 모습으로 변하게 될지는 모르지만 윈도우즈(Windows)에서 ComfyUI를 사용하는데 있어서 편리하게 이용할 수 있도록 기능들을 추가해 나가겠습니다.    

