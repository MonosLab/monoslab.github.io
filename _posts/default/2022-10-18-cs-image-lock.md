---
title: "[ C# ] Image.FromFile()을 이용하여 이미지를 불러 올때 락(lock) 문제 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-10-18 18:00:00 +0900
categories: [프로그래밍, C#]
tags: [C#, Image.FromFile(), lock, PictureBox]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post16.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post16)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **Image.FromFile()을 이용하여 이미지를 불러 올때 락(lock) 문제 해결 방법**  
C#에서 이미지를 불러올때 Image.FromFile()를 자주 이용하실텐데, 이렇게 불러온 경우 객체를 Dispose() 함수를 호출하기 전까지는 파일에 대한 제어권을 넘겨 받을 수가 없습니다. 대표적인 예로 Picturebox에서 불러오고 해당 파일을 수정하고 저장하려 할때 ERROR_SHARING_VIOLATION(0x20) 오류가 발생할 것입니다.

이를 해결하기 위한 방법은 아래와 같습니다.

* 락(lock)   
```cpp   
pbxImage.Image = Image.FromFile("ImageFile.png");
```   

* 언락(unlock)   
```cpp   
using (FileStream fs = new FileStream(FileOpenDialog.FileName, FileMode.Open, FileAccess.Read))
{
   pbxImage.Image = FromStream(fs);
}
```   
또는
```cpp   
FileStream fs = new FileStream(FileOpenDialog.FileName, FileMode.Open, FileAccess.Read);
pbxImage.Image = FromStream(fs);
fs.Close();
fs.Dispose();
```  

와 같이 해주면 안전하게 PictureBox에 그린 이후 곧장 다른 작업을 하실 수 있습니다.

참고로 ico 파일인 경우에는 아래와 같이 작업을 해주시면 됩니다.   
```cpp   
using (FileStream fs = new FileStream(FileOpenDialog.FileName, FileMode.Open, FileAccess.Read))
{
   Icon myIcon = new System.Drawing.Icon(fs);
   pbxIcon.SizeMode = PictureBoxSizeMode.StretchImage;
   pbxIcon.Image = myIcon.ToBitmap();
}
``` 

(※ 여기서 pbxImage와 pbxIcon은 PictureBox 객체 입니다.)

