---
title: "[ Error ] Jekyll Error 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-08-11 06:00:00 +0900
categories: [정보, 에러]
tags: [Error, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post10.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post10)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# <span style="color:red">***ERROR: Invalid first code point of tag name U+B9AC***</span>   
> ***<span style="color:red">ERROR: Invalid first code point of tag name U+B9AC</span>***   
<span style="color:black">해결방법 : md 파일 내에 잘못 사용된 tag로 인하여 발생되는 에러 이며 tag가 아닌 부분은 명시적으로 tag가 아님을 확인 시켜주어야 합니다. <span style="color:red">\<와 \>의 앞에 \ </span>를 넣어주시면 해결이 됩니다. 아래는 해당 오류의 예입니다.   
> 오류 발생 : https://raw.githubusercontent.com/\<사용자 이름\>/\<리포지토리 이름\>/master/\<파일명\>   
> 화면 표시 : https://raw.githubusercontent.com/\<사용자 이름=""\>/\<리포지토리 이름=""\>/master/\<파일명\>   
> 오류 해결 : https://raw.githubusercontent.com/\ \<사용자 이름\>/\ \<리포지토리 이름\ \>/master/\ \<파일명\ \>   
</span>  

---  
# <span style="color:red">***Liquid Exception: undefined method 'gsub' for 3.0***</span>   
> ***<span style="color:red">Liquid Exception: undefined method 'gsub' for 3.0:Float string.gsub(replaceable_char, "-")</span>***    

```   
Liquid Exception: undefined method `gsub' for 3.0:Float string.gsub(replaceable_char, "-") ^^^^^ in E:/Github/monoslab.github.io/_layouts/post.html   
   ------------------------------------------------   
   Jekyll 4.2.2   Please append `--trace` to the `serve` command   
                  for any additional information or backtrace.   
   ------------------------------------------------   
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/utils.rb:364:in `replace_character_sequence_with_hyphen':
 undefined method `gsub' for 3.0:Float (NoMethodError)   

   string.gsub(replaceable_char, "-")
          ^^^^^
```   
> <span style="color:black">해결방법 : tags의 배열에 숫자값이 먼저 나오는 경우 해당 오류를 발생시킵니다. 아래는 해당 오류의 예입니다.   
> 오류 발생 : tags: [Error, TBC, 3.0]   
> 오류 해결 : tags: [Error, TBC 3.0] 
</span>
---
