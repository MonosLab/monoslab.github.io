---
title: "[ Format ] 커서(Cursor; .cur)" 
sub: post
author: Kwangsoo Seo
date: 2022-07-07 00:00:00 +0900
categories: [정보, 포맷]
tags: [Format, cur, cursor, .cur]
---
[![HitCount](https://hits.dwyl.com/MonosLab/post3.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post3)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **기본 정보**  
최초의 CUR 파일 형식은 1981년 Microsoft의 Windows 1.0 운영 체제에 적용되었으며, 윈도우95부터는 애니메이션이 가능한 ani 파일도 사용이 가능해졌습니다. CUR 파일은 ICO 파일과 동일한 정지 이미지이며, 두개 모두 Device-Independent Bitmap DIB (Device-Independent Bitmap) 사양을 기반으로 만들어집니다. 참고로 cur 파일은 하나 이상의 이미지 파일을 포함할 수 있습니다.   

---

# **기본 파일 포맷**  

| Name | size | Description |   
| :--- | :--- | :--- |   
| Reserved | 2 byte | 항상 0 |   
| Type | 2byte | 항상 2 (*참고 : ico는 1) |   
| Count | 2byte | 파일안에 커서의 갯수 |   
| Entries | Count * 2byte | 커서들의 리스트 |   
| &nbsp;&nbsp;&nbsp;&nbsp;Width | 1byte  | 커서의 넓이 |   
| &nbsp;&nbsp;&nbsp;&nbsp;Height | 1byte  | 커서의 높이 |   
| &nbsp;&nbsp;&nbsp;&nbsp;ColorCount | 1byte  | = 0 ! |   
| &nbsp;&nbsp;&nbsp;&nbsp;Reserved | 1byte  | 항상 0 |   
| &nbsp;&nbsp;&nbsp;&nbsp;XHotspot | 2byte  | 핫스팟(클릭)의 X 좌표 |   
| &nbsp;&nbsp;&nbsp;&nbsp;YHotspot | 2byte  | 핫스팟(클릭)의 Y 좌표 |   
| &nbsp;&nbsp;&nbsp;&nbsp;SizeInBytes | 4byte  | 크기(InfoHeader + ANDBitmap + XORBitmap) |   
| &nbsp;&nbsp;&nbsp;&nbsp;FileOffset | 4byte  | InfoHeader의 시작 위치 |   
| &nbsp;&nbsp;갯수 만큼 반복 |  |  |   
| InfoHeader | 40 byte | Variant of BMP InfoHeader |   
| &nbsp;&nbsp;&nbsp;&nbsp;Size | 4byte  | InfoHeader 구조체 사이즈(항상 40) |   
| &nbsp;&nbsp;&nbsp;&nbsp;Width | 4byte  |  커서 넓이 |   
| &nbsp;&nbsp;&nbsp;&nbsp;Height | 4byte  |  커서 높이 (XORbitmap와 ANDbitmap의 높이도 더함) |   
| &nbsp;&nbsp;&nbsp;&nbsp;Planes | 2byte  |  planes의 수(항상 1)|   
| &nbsp;&nbsp;&nbsp;&nbsp;BitCount | 2byte  | 픽셀당 비트수(항상 1) |   
| &nbsp;&nbsp;&nbsp;&nbsp;Compression | 4byte  |  압축 타입(항상 0) |   
| &nbsp;&nbsp;&nbsp;&nbsp;ImageSize | 4byte  | 픽셀의 갯수(Byte 안의 이미지 크기) = 0 (uncompressed) |   
| &nbsp;&nbsp;&nbsp;&nbsp;XpixelsPerM | 4byte  | 사용하지 않음(0)  |    
| &nbsp;&nbsp;&nbsp;&nbsp;YpixelsPerM | 4byte  | 사용하지 않음(0) |   
| &nbsp;&nbsp;&nbsp;&nbsp;ColorsUsed | 4byte  | 사용하지 않음(0) |   
| &nbsp;&nbsp;&nbsp;&nbsp;ColorsImportant | 4byte  | 사용하지 않음(0) |   
| Colors| 8 byte |픽셀당 비트수가 항상 1이므로 항상 2개의 항목을 가짐. |
| &nbsp;&nbsp;&nbsp;&nbsp;Color 0 R | 1byte  | 배경 R = 0 |  
| &nbsp;&nbsp;&nbsp;&nbsp;Color 0 G | 1byte  | 배경 G = 0 |  
| &nbsp;&nbsp;&nbsp;&nbsp;Color 0 B | 1byte  | 배경 B = 0 |  
| &nbsp;&nbsp;&nbsp;&nbsp;reserved | 1byte  | 항상 0 |  
| &nbsp;&nbsp;&nbsp;&nbsp;Color 1 R | 1byte  | 전경 R = 255 |  
| &nbsp;&nbsp;&nbsp;&nbsp;Color 1 G | 1byte  | 전경 G = 255 |  
| &nbsp;&nbsp;&nbsp;&nbsp;Color 1 B | 1byte  | 전경 B = 255 |  
| &nbsp;&nbsp;&nbsp;&nbsp;reserved | 1byte  | 항상 0 |  
| XORbitmap | | '래스터 데이터 인코딩' 참고 |  
| ANDbitmap | | '래스터 데이터 인코딩' 참고 |  

----

**래스터 데이터 인코딩(Raster Data encoding)**  
픽셀은 흑백 BMP와 같은 방식으로 아래에서 위, 왼쪽에서 오른쪽으로 저장됩니다. 픽셀 라인은 32비트(4바이트) 내에서 0으로 채워집니다. 모든 라인은 동일한 바이트 수를 갖습니다. 모든 바이트는 8개의 픽셀을 갖으며, 최상위 비트는 그 중 가장 왼쪽에 있는 픽셀을 나타냅니다. 최대 32비트 경계까지 0으로 채우는 것을 기억하십시오(최대 31개까지 픽셀에 0이 채워질 수 있습니다.). 비록 2개의 색상 테이블이 있지만 Windows는 이를 무시하는 것 같습니다. Windows가 스크린에 커서를 그릴 때마다 ANDbitmap이 적용됩니다. 그 이후에 XORBitmap이 적용됩니다. 두 비트맵 모두 흑백 BMP와 동일한 방식으로 인코딩됩니다. 파일에 둘 이상의 커서가 있는 경우 Windows는 시스템 설정과 일치하는 커서를 사용합니다.

| AND | XOR | 결과 |   
| :---: | :---: | :---: |   
| 0 | 0 | 검은색 픽셀 |
| 0 | 1 | 흰색 픽셀 |
| 1 | 0 | 투명한 배경 픽셀 |
| 1 | 1 | 반전된 배경 픽셀 |

----

**Structure**
```cpp
namespace cur_file
{
#pragma pack(push,1)
	typedef struct _CURSOR_ENTRIES
	{
		unsigned char width;
		unsigned char height;
		unsigned char color_count;
		unsigned char reserved = 0;
		unsigned short x_hotspot;
		unsigned short y_hotspot;
		unsigned int size_in_bytes;
		unsigned int file_offset;
	} CURSOR_ENTRIES;

	typedef struct _CURSOR_INFO_HEADER
	{
		unsigned int size = 40;
		unsigned int width;
		unsigned int height;
		unsigned short planes = 1;
		unsigned short bit_count = 1;
		unsigned int compression = 0;
		unsigned int image_size = 0;	/* uncompressed */
		unsigned int x_pixels_per_M = 0;
		unsigned int y_pixels_per_M = 0;
		unsigned int colors_used = 0;
		unsigned int colors_important = 0;
	} CURSOR_INFO_HEADER;

	typedef struct _CURSOR_HEADER
	{
		short reserved = 0;
		/* type ==> ico : 1, cur : 2 */
		short type = 2;
		short count;
		CURSOR_ENTRIES entires;
		CURSOR_INFO_HEADER info_header;
	};
#pragma pack(pop)
}
```

> 참고   
https://www.daubnet.com/en/file-format-cur   
https://www.daubnet.com/en/file-format-ico   