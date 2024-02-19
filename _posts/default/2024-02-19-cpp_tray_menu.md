---
title: "[ VC++ ] 트레이 팝업 메뉴의 외부 클릭시 메뉴가 사라지지 않는 문제 해결"
sub: post
author: Kwangsoo Seo
date: 2024-02-19 00:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, MFC, tray, menu, outside, click]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post40.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post40)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 트레이 팝업 메뉴의 외부 클릭시 메뉴가 사라지지 않는 문제 해결 #  

윈도우 프로그래밍시 트레이 아이콘을 자주 사용하곤 하는데, 이때 트레이 아이콘에서 TrackPopupMenu 함수를 이용하여 팝업 메뉴를 띄웠을때 메뉴 영역 밖을 클릭 했을 경우 사라지는 경우도 있고 그렇지 않은 경우도 심심찮게 발생되는 것을 확인 할 수 있습니다.     

보통 트레이 아이콘 개발시 마우스 클릭 이벤트가 발생할 경우 아래와 같이 많이 코딩을 합니다.
```cpp
LRESULT CXXXXDlg::OnTrayIcon(WPARAM wParam, LPARAM lParam)
{
	HWND hWnd = GetSafeHwnd();
	switch (lParam)
	{
		... [ 생략 ] ...
		case WM_RBUTTONDOWN:
		{
			CPoint pt;
			HMENU hMenu = CreatePopupMenu();
			HMENU hSubMenu = CreatePopupMenu();
			GetCursorPos(&pt);
			AppendMenu(hSubMenu, MF_STRING, UDM_LANG_KO, CResourceEx::GetInstance()->LoadStringEx(IDS_KOREAN));
			...
			AppendMenu(hSubMenu, MF_STRING, UDM_LANG_VI, CResourceEx::GetInstance()->LoadStringEx(IDS_VIETNAMESE));
			CheckMenuItem(hSubMenu, UDM_LANG_KO, MF_CHECKED);
			EnableMenuItem(hSubMenu, UDM_LANG_KO, MF_BYCOMMAND | MF_DISABLED | MF_GRAYED);
			AppendMenu(hMenu, MF_POPUP | MF_STRING, (UINT_PTR)hSubMenu, CResourceEx::GetInstance()->LoadStringEx(IDS_SELECT_LANGUAGE));
			AppendMenu(hMenu, MF_SEPARATOR, NULL, NULL);
			AppendMenu(hMenu, MF_STRING, UDM_CLOSE, CResourceEx::GetInstance()->LoadStringEx(IDT_EXIT));
			TrackPopupMenu(hMenu, TPM_LEFTALIGN | TPM_RIGHTBUTTON, pt.x, pt.y, 0, hWnd, NULL);
			DestroyMenu(hSubMenu);
			DestroyMenu(hMenu);
		}
		break;
	}

	return 0L;
}
```
위와 같은 방법으로 팝업 메뉴를 띄운 경우 hWnd가 최상단에 위치한 경우에는 정상적으로 Popup menu가 사라지겠지만, 아닌 경우에는 메뉴의 외부를 클릭하더라도 그대로 존재함을 확인 할 수 있습니다. 아래와 같이 <span style="color:red">::SetForegroundWindow(hWnd);</span> 한 줄만 추가 하면 이런 문제를 해결 할 수 있습니다. 구글링을 해보면 질문은 많이 있는데, 솔루션을 제시한 글이 없는것 같네요.  

```cpp
LRESULT CXXXXDlg::OnTrayIcon(WPARAM wParam, LPARAM lParam)
{
	HWND hWnd = GetSafeHwnd();
	switch (lParam)
	{
		... [ 생략 ] ...
		case WM_RBUTTONDOWN:
		{
			CPoint pt;
			HMENU hMenu = CreatePopupMenu();
			HMENU hSubMenu = CreatePopupMenu();
			GetCursorPos(&pt);
			AppendMenu(hSubMenu, MF_STRING, UDM_LANG_KO, CResourceEx::GetInstance()->LoadStringEx(IDS_KOREAN));
			...
			AppendMenu(hSubMenu, MF_STRING, UDM_LANG_VI, CResourceEx::GetInstance()->LoadStringEx(IDS_VIETNAMESE));
			CheckMenuItem(hSubMenu, UDM_LANG_KO, MF_CHECKED);
			EnableMenuItem(hSubMenu, UDM_LANG_KO, MF_BYCOMMAND | MF_DISABLED | MF_GRAYED);
			AppendMenu(hMenu, MF_POPUP | MF_STRING, (UINT_PTR)hSubMenu, CResourceEx::GetInstance()->LoadStringEx(IDS_SELECT_LANGUAGE));
			AppendMenu(hMenu, MF_SEPARATOR, NULL, NULL);
			AppendMenu(hMenu, MF_STRING, UDM_CLOSE, CResourceEx::GetInstance()->LoadStringEx(IDT_EXIT));
================================================================
>>			::SetForegroundWindow(hWnd);
================================================================
			TrackPopupMenu(hMenu, TPM_LEFTALIGN | TPM_RIGHTBUTTON, pt.x, pt.y, 0, hWnd, NULL);
			DestroyMenu(hSubMenu);
			DestroyMenu(hMenu);
		}
		break;
	}

	return 0L;
}
```

