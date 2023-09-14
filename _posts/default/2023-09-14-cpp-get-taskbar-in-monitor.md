---
title: "[ VC++ ] 타이틀바가 없는 윈도우 최대화"
sub: post
author: Kwangsoo Seo
date: 2023-09-14 00:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, taskbar, maximize, 작업표시줄, 최대화]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post37.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post37)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# Taskbar의 위치에 따른 윈도우의 최대화 #  

다이얼로그 베이스에서 타이틀바를 제거하고 윈도우의 최대화시 하단에 taskbar가 있는 경우에는 문제가 발생하지 않지만, 위쪽 또는 왼쪽에 위치하는 경우 문제가 발생됨을 확인하였습니다. Windows OS에서 알아서 해주면 참 좋을텐데, 그렇지 못한 것이 참 아쉽습니다. 그리고 또 멀티 모니터의 경우에도 또 다른 문제가 발생되어 수동으로 윈도우의 최대화시 위치를 보정하고 사이즈를 잡아주기 위해 아래와 같이 작업을 하였습니다. 이래 저래 손이 많이 가는 타이틀바가 없는 경우의 윈도우 최대화 작업이었네요. ^^   

```cpp
#define PX_CALIBRATE		-7
#define PX_TASKBAR_HEIGHT	32

void CMonoslabDlg::PreSubclassWindow()
{
	DWORD dwStyle = /*WS_OVERLAPPED |*/ WS_THICKFRAME | WS_CLIPSIBLINGS | WS_MINIMIZEBOX | WS_MAXIMIZEBOX | WS_SIZEBOX;
	ModifyStyle(WS_CAPTION, dwStyle, SWP_FRAMECHANGED);
	CDialogEx::PreSubclassWindow();
}

void CMonoslabDlg::OnGetMinMaxInfo(MINMAXINFO* lpMMI)
{
	HMONITOR hMon = MonitorFromWindow(m_hWnd, MONITOR_DEFAULTTONEAREST);
	MONITORINFOEX mi;
	mi.cbSize = sizeof(MONITORINFOEX);
	GetMonitorInfo(hMon, &mi);
	
	RECT rc			= mi.rcWork;		// workarea rectangle
	RECT rcm		= mi.rcMonitor;		// monitor rectangle
	UINT nWorkAreaHeight	= rc.bottom - rc.top;
	UINT nWorkAreaWidth	= rc.right - rc.left;
	UINT nMonitorHeight	= rcm.bottom - rcm.top;
	UINT nMonitorWidth	= rcm.right - rcm.left;

	// Get taskbar info.
	if ((nMonitorHeight - nWorkAreaHeight) >= PX_TASKBAR_HEIGHT)
	{
		lpMMI->ptMaxPosition.x = PX_CALIBRATE;
		lpMMI->ptMaxPosition.y = rc.top + PX_CALIBRATE;
	}
	else if (nMonitorWidth - nWorkAreaWidth >= PX_TASKBAR_HEIGHT)
	{
		lpMMI->ptMaxPosition.x = rc.left + PX_CALIBRATE;
		lpMMI->ptMaxPosition.y = PX_CALIBRATE;
	}

	lpMMI->ptMaxSize.x = rc.right - rc.left + 15;	// 그림자 여백(20) 보정
	lpMMI->ptMaxSize.y = rc.bottom - rc.top + 17;	// 37 - 그림자 여백(20) 보정
	lpMMI->ptMinTrackSize.x = WND_MIN_X;		// 윈도우 최소 넓이
	lpMMI->ptMinTrackSize.y = WND_MIN_Y;		// 윈도우 최소 높이

	CDialogEx::OnGetMinMaxInfo(lpMMI);
}
```


윈도우의 최대화시 lpMMI->ptMaxPosition.x와 lpMMI->ptMaxPosition.y의 값은 기본 -7 입니다. 이를 보정하기 위해 PX_CALIBRATE(-7)을 정의하고 각 위치에 반영하였습니다.    
OnGetMinMaxInfo() 함수의 경우 윈도우의 변동이 발생될 경우 매번 호출이 되기 때문에 윈도우가 어느 모니터에 있던지 최대화시 작업영역(Workarea)을 얻어와 정확한 위치를 찾아 최대화 할 수 있었습니다. 이때  PX_TASKBAR_HEIGHT(32) 값으로 작업표시줄의 위치 유무를 판단한 근거는 작업표시줄의 높이는 최소 32여서 그 이상이면 작업표시줄이 있는 것으로 판단하였습니다.
