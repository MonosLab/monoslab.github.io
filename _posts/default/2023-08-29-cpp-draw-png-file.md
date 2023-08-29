---
title: "[ VC++ ] CImage 클래스를 이용하여 PNG 파일을 그릴때 깨지는 문제 해결"
sub: post
author: Kwangsoo Seo
date: 2023-08-29 00:00:00 +0900
categories: [프로그래밍, C++ (CPP)]
tags: [VC++, png, alpha, draw]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post35.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post35)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# CImage 클래스를 이용한 Draw  

CImage 클래스를 이용하여 화면에 이미지를 출력할 경우 투명 영역을 처리할 때 아래와 같은 "일반적으로 이미지를 로딩하고 그려주는 경우" 방법으로 이미지를 로드하고 알파값을 처리해주면 이미지 깨짐 현상이 발생됩니다. 이미지의 깨짐없이 처리하기 위해서는 약간의 작업을 해주어야 하는데, 아래 이미지는 처리 전과 처리 후의 모습을 보여주는 이미지입니다.

![CImage_PNG](https://monoslab.github.io/assets/img/posts/cimage_png_transparent.png)

----

## 일반적으로 이미지를 로딩하고 그려주는 경우 ##

```cpp
void CMonosoftDlg::OnPaint()
{
    CClientDC dc(this);
    CDC  dcMem;
    dcMem.CreateCompatibleDC(&dc);

    int nX = 0;
    int nY = 0;

    CBitmap bitmap;
    CImage img;
    if(img.Load(_T("C:\\img.png") == S_OK)
    {
        nX = img.GetWidth();
        nY = img.GetHeight();
        bitmap.Attach(img.Detach());

        CBitmap *pOldBmp = dcMem.SelectObject(&bitmap);
    
        BLENDFUNCTION bfImage = { AC_SRC_OVER, 0x00, 0xFF, AC_SRC_ALPHA };
        AlphaBlend(dc.GetSafeHdc(), 100, 100, nX, nY, dcMem.GetSafeHdc(), 0, 0,nX, nY, bfImage); 

        dcMem.SelectObject(pOldBitmap);
    }
    
    CDialog::OnPaint();
}
```

----

## 이미지를 읽어와서 보정 후 그려주는 경우 ##

```cpp
inline void SetBitmapAlpha(HDC hDC, HBITMAP hBmp)
{
    BITMAP bm = { 0x00, };
    GetObject(hBmp, sizeof(bm), &bm);
    BITMAPINFO *pBi = (BITMAPINFO*)_malloca(sizeof(BITMAPINFOHEADER) + (256 * sizeof(RGBQUAD)));
    if (pBi)
    {
        ::ZeroMemory(pBi, sizeof(BITMAPINFOHEADER) + (256 * sizeof(RGBQUAD)));
        pBi->bmiHeader.biSize = sizeof(BITMAPINFOHEADER);
        BOOL bRes = ::GetDIBits(hDC, hBmp, 0, bm.bmHeight, NULL, pBi, DIB_RGB_COLORS);
        if (!bRes || pBi->bmiHeader.biBitCount != 32) return;
        LPBYTE pBitData = (LPBYTE) ::LocalAlloc(LPTR, bm.bmWidth * bm.bmHeight * sizeof(DWORD));
        if (pBitData == NULL) return;
        LPBYTE pData = pBitData;
        ::GetDIBits(hDC, hBmp, 0, bm.bmHeight, pData, pBi, DIB_RGB_COLORS);
        for (int y = 0; y < bm.bmHeight; y++) {
            for (int x = 0; x < bm.bmWidth; x++) {
                pData[0] = (BYTE)((DWORD)pData[0] * pData[3] / 255);
                pData[1] = (BYTE)((DWORD)pData[1] * pData[3] / 255);
                pData[2] = (BYTE)((DWORD)pData[2] * pData[3] / 255);
                pData += 4;
            }
        }
        ::SetDIBits(hDC, hBmp, 0, bm.bmHeight, pBitData, pBi, DIB_RGB_COLORS);
        ::LocalFree(pBitData);
    }
}

void CMonosoftDlg::OnPaint()
{
    CClientDC dc(this);
    CDC  dcMem;
    dcMem.CreateCompatibleDC(&dc);

    int nX = 0;
    int nY = 0;

    CImage img;
    if(img.Load(_T("C:\\img.png") == S_OK)
    {
        nX = img.GetWidth();
        nY = img.GetHeight();
        HDC hdc = img.GetDC();
        SetBitmapAlpha(dcMem, img);
    
        BLENDFUNCTION bfImage = { AC_SRC_OVER, 0x00, 0xFF, AC_SRC_ALPHA };
        AlphaBlend(dc.GetSafeHdc(), 100, 100, nX, nY, hdc, 0, 0,nX, nY, bfImage); 

        img.ReleaseDC();
    }

    CDialog::OnPaint();
}
```
