---
title: "[ VC++ ] 빠른 실행창(Taskbar)의 프로그램 아이콘 클릭시 최소화 되지 않는 문제 해결"
sub: post
author: Kwangsoo Seo
date: 2023-09-12 00:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, taskbar, minimize, 빠른실행창, 최소화]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post36.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post36)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# Taskbar의 아이콘 클릭시 최소화가 되지 않는 문제 해결 방법 #  

다이얼로그 베이스에서 타이틀바를 제거하고 프로그램을 만들일이 있어서 개발하여 잘 사용하고 있었습니다. 그런데 어느날 보니 taskbar의 프로그램 아이콘을 클릭하면 프로그램이 상단으로 올라 오는 것은 잘되나 다시 한번 클릭하면 최소화가 되지 않고 멀똥 멀똥 저를 쳐다 보는 다이얼로그를 발견하게 되었습니다.   

> <span style="color:black">"아 무언가 잘못 되었구나"</span>   

바로 소스를 이리 저리 살펴 보아도 별로 문제되는 것이 없어 보였습니다.   

> <span style="color:black">'아무래도 타이틀바를 없앤 것과 무슨 연관이 있을텐데...'</span>   

여기까지 생각이 미치자 곧바로 한 곳이 생각이 났습니다.   

```cpp
void CMonoslabDlg::PreSubclassWindow()
{
	// borderless shadow
	DWORD dwStyle = WS_THICKFRAME | WS_CLIPSIBLINGS;
	ModifyStyle(WS_CAPTION, dwStyle, SWP_FRAMECHANGED);
	CDialogEx::PreSubclassWindow();
}
```

역시나 코드는 거짓이 없다라는 걸 깨닫는 순간이었습니다.   
원래의 다이얼로그의 기본 style을 완전히 무시하고 내가 원하는 style(WS_THICKFRAME, WS_CLIPSIBLINGS)만 집어 넣었던 것입니다.   
아래와 같이 최소화(WS_MINIMIZEBOX)와 최대화(WS_MAXIMIZEBOX)를 style에 추가한 이후에야 taskbar 클릭시 창이 앞으로 뜨고, 다시 눌렀을 때 비로소 최소화가 되었습니다.   

```cpp
void CMonoslabDlg::PreSubclassWindow()
{
	DWORD dwStyle = WS_THICKFRAME | WS_CLIPSIBLINGS | WS_MINIMIZEBOX | WS_MAXIMIZEBOX;

	// borderless shadow
	ModifyStyle(WS_CAPTION, dwStyle, SWP_FRAMECHANGED);
	CDialogEx::PreSubclassWindow();
}
```

<span style="background-color: #FFF5B1">**만약 윈도우의 기본 속성을 모두 상속하여 처리 하고 싶다면,**</span>   
<span style="color:red">GetStyle()</span> 함수를 이용하여 현재의 스타일을 가져온 이후에 스타일을 추가 또는 삭제하시고 ModifyStyle(...) 함수를 호출하여 주시면 됩니다.
