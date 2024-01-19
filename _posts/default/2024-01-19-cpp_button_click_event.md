---
title: "[ VC++ ] 부모가 비활성인 경우, 버튼 클릭 이벤트 문제 해결"
sub: post
author: Kwangsoo Seo
date: 2024-01-19 00:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, MFC, 커스텀 버튼, 이벤트, 마우스, Custom button, event, mouse, LButtonDown]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post39.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post39)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 커스텀 버튼(Cutsomizing button)의 마우스 클릭 문제 해결 #  

CButton을 상속받아 커스텀 버튼을 만든 이후에 버튼의 부모 윈도우가 비활성(Lost focus, Deactive) 상태에 있는 경우 버튼을 클릭하면 포커스만 되고 클릭 이벤트가 발생<span style="color:Teal">(WM_LBUTTONDOWN/WM_LBUTTONUP는 커스텀 버튼에서 정상적으로 이벤트가 발생됨)</span>하였으나 버튼 클릭시 연동된 함수는 호출되지 않는 현상이 발생하였습니다. 원인은 마우스 이동을 감지하기 위해 OnMouseMove() 함수에서 사용한 <span style="color:red">SetCapture/ReleaseCapture</span> 함수가 원인인 것으로 추정이 됩니다.<span style="color:Teal">(SetCapture/ReleaseCapture 함수를 사용하지 않은 경우 정상적으로 원클릭으로 작동됨을 확인함)</span>
하지만 버튼 위에 마우스 오버시 버튼을 그려주어야 하기 때문에 사용하지 않을 수는 없는 상황이어서 아래와 같은 방법처럼 마우스가 버튼 내에 있는 경우라면 포커스를 주도록 설정하였습니다.   

> 윈도우가 비활성된 상태에서 버튼을 클릭하면 이벤트는 발생이 되나 버튼과 연결된 함수는 호출되지 않고, 활성화 된 상태에서는 정상적으로 함수가 호출 되는 문제를 해결한 방법입니다. 이와 유사한 경우가 발생되는 경우에도 Set focus와 Release focus를 적절히 활용하여 해결을 할 수 있을 것으로 판단됩니다.   

**<U>변경 전</U>**   

```cpp
void CXRoundedButton::OnMouseMove(UINT nFlags, CPoint point)   
{   
   CRect rcClient;   
   GetClientRect(rcClient);	
   // Check, if Mouse is on Control   
   if (rcClient.PtInRect(point))   
   {   
      bool bRedrawNeeded = !m_bMouseOnButton;	
      // Mouse is on Control   
      m_bMouseOnButton = true;   
      // Set Capture to recognize, when the mouse leaves the control   
      SetCapture();	   
      // Redraw Control, if Button is hot   
      if (m_bIsHotButton) Invalidate();   
   }   
   else   
   {   
      // We have lost the mouse-capture, so the mouse has left the buttons face   
      m_bMouseOnButton = false;
      // Mouse has left the button
      ReleaseCapture();    
      // Redraw Control, if Button is hot   
      if (m_bIsHotButton) Invalidate();   
   }   
   CButton::OnMouseMove(nFlags, point);   
}   
```

**<U>변경 후</U>**   

```cpp
void CXRoundedButton::OnMouseMove(UINT nFlags, CPoint point)   
{   
   CRect rcClient;   
   GetClientRect(rcClient);	
   // Check, if Mouse is on Control   
   if (rcClient.PtInRect(point))   
   {   
      bool bRedrawNeeded = !m_bMouseOnButton;	
      // Mouse is on Control   
      m_bMouseOnButton = true;   
      if (GetCapture() != this)   
      {   
         // Set Capture to recognize, when the mouse leaves the control
         SetCapture();   
         // Redraw Control, if Button is hot
         if (m_bIsHotButton) Invalidate();   
         // Set focus   
         SendMessage(WM_SETFOCUS);   
      }   
   }   
   else   
   {   
      // We have lost the mouse-capture, so the mouse has left the buttons face   
      m_bMouseOnButton = false;
      // Mouse has left the button
      ReleaseCapture();    
      // Redraw Control, if Button is hot   
      if (m_bIsHotButton) Invalidate();   
      // Kill focus   
      SendMessage(WM_KILLFOCUS);   
   }   
   CButton::OnMouseMove(nFlags, point);   
}   
```
