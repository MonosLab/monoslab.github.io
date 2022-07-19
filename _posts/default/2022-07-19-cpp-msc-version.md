---
title: "[ VC++ ] MSVC 버전 (_MSC_VER, _MSC_FULL_VER)" 
sub: post
author: Kwangsoo Seo
date: 2022-07-19 02:00:00 +0900
categories: [정보, 포맷]
tags: [VC++, _MSC_VER, _MSC_FULL_VER, MSVC, Version]
---
[![HitCount](https://hits.dwyl.com/MonosLab/post6.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post6)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **Visual Studio 버전 체크**  
Visual Studio의 버전을 체크하여 버전에 맞게 컴파일 할 수 있도록 적용할 수 있습니다.   
_MSC_VER는 버전의 주 번호 및 부 번호 요소를 변하지 않는 정수값(리터럴)으로 정의합니다. 주 번호는 마침표로 구분된 버전 번호의 첫 번째 요소이며, 부 번호는 두 번째 요소입니다. 예를 들어 Microsoft C/C++ 컴파일러의 버전 번호가 19.00.24234.1인 경우 _MSC_VER 매크로를 실행하면 1900이 나옵니다. 컴파일러의 버전 번호를 보려면 "명령 프롬프트"에서 cl 명령어를 입력하면 확인할 수 있습니다. 참고로 _MS_FULL_VER을 이용하여 빌드 버전까지 포함된 버전값도 얻어 올수도 있습니다. 이 두 값은 항상 정의되어져 있습니다.

아래와 같이 컴파일러에 맞게 소스를 적용하여 컴파일 할 수 있습니다.

```cpp
#if _MSC_VER >= 1910
// . . .
#elif _MSC_VER >= 1900
// . . .
#else
// . . .
#endif
```

|표기|Visual Studio version|VC ++<br>version|_MSC_VER|_MSC_FULL_VER|   
| :---: | :---: | :---: | :---: | :---: |   
|2022|Visual Studio 2022 version 17.0.2|14.30|1930|193030706|   
|2022|Visual Studio 2022 version 17.0.1|14.30|1930|193030705|   
|2019 Update 11|Visual Studio 2019 version 16.11.2|14.28|1929|192930133|   
|2019 Update 9|Visual Studio 2019 version 16.9.2|14.28|1928|192829913|   
|2019 Update 8|Visual Studio 2019 version 16.8.2|14.28|1928|192829334|   
|2019 Update 8|Visual Studio 2019 version 16.8.1|14.28|1928|192829333|   
|2019 Update 7|Visual Studio 2019 version 16.7|14.27|1927|192729112|   
|2019 Update 6|Visual Studio 2019 version 16.6.2|14.26|1926|192628806|   
|2019 Update 5|Visual Studio 2019 version 16.5.1|14.25|1925|192528611|   
|2019 Update 4|Visual Studio 2019 version 16.4.0|14.24|1924|192428314|   
|2019 Update 3|Visual Studio 2019 version 16.3.2|14.23|1923|192328105|   
|2019 Update 2|Visual Studio 2019 version 16.2.3|14.22|1922|192227905|   
|2019 Update 1|Visual Studio 2019 version 16.1.2|14.21|1921|192127702|   
|2019|Visual Studio 2019 version 16.0.0|14.20|1920|192027508|   
|2017 Update 9|Visual Studio 2017 version 15.9.11|14.16|1916|191627030|   
|2017 Update 9|Visual Studio 2017 version 15.9.7|14.16|1916|191627027|   
|2017 Update 9|Visual Studio 2017 version 15.9.5|14.16|1916|191627026|   
|2017 Update 9|Visual Studio 2017 version 15.9.4|14.16|1916|191627025|   
|2017 Update 9|Visual Studio 2017 version 15.9.1|14.16|1916|191627023|   
|2017 Update 9|Visual Studio 2017 version 15.9.0|14.16|1916||   
|2017 Update 8|Visual Studio 2017 version 15.8.0|14.15|1915||   
|2017 Update 7|Visual Studio 2017 version 15.7.5|14.14|1914|191426433|   
|2017 Update 7|Visual Studio 2017 version 15.7.3|14.14|1914|191426430|   
|2017 Update 7|Visual Studio 2017 version 15.7.2|14.14|1914|191426429|   
|2017 Update 7|Visual Studio 2017 version 15.7.1|14.14|1914|191426428|   
|2017 Update 6|Visual Studio 2017 version 15.6.7|14.13|1913|191326132|   
|2017 Update 6|Visual Studio 2017 version 15.6.6|14.13|1913|191326131|   
|2017 Update 6|Visual Studio 2017 version 15.6.4|14.13|1913|191326129|   
|2017 Update 6|Visual Studio 2017 version 15.6.3|14.13|1913|191326129|   
|2017 Update 6|Visual Studio 2017 version 15.6.2|14.13|1913|191326128|   
|2017 Update 6|Visual Studio 2017 version 15.6.1|14.13|1913|191326128|   
|2017 Update 6|Visual Studio 2017 version 15.6.0|14.13|1913|191326128|   
|2017 Update 5|Visual Studio 2017 version 15.5.7|14.12|1912|191225835|   
|2017 Update 5|Visual Studio 2017 version 15.5.6|14.12|1912|191225835|   
|2017 Update 5|Visual Studio 2017 version 15.5.4|14.12|1912|191225834|   
|2017 Update 5|Visual Studio 2017 version 15.5.3|14.12|1912|191225834|   
|2017 Update 5|Visual Studio 2017 version 15.5.2|14.12|1912|191225831|   
|2017 Update 4|Visual Studio 2017 version 15.4.5|14.11|1911|191125547|   
|2017 Update 4|Visual Studio 2017 version 15.4.4|14.11|1911|191125542|   
|2017 Update 3|Visual Studio 2017 version 15.3.3|14.11|1911|191125507|   
|2017 Update 2|Visual Studio 2017 version 15.2|14.10|1910|191025017|   
|2017 Update 1|Visual Studio 2017 version 15.1|14.10|1910|191025017|   
|2017|Visual Studio 2017 version 15.0|14.10|1910|191025017|   
|2015 Update 3|Visual Studio 2015 Update 3 [14.0]|14.0|1900|190024210|   
|2015 Update 2|Visual Studio 2015 Update 2 [14.0]|14.0|1900|190023918|   
|2015 Update 1|Visual Studio 2015 Update 1 [14.0]|14.0|1900|190023506|   
|2015|Visual Studio 2015 [14.0]	14.0|1900|190023026|   
|2013 Nobemver CTP|Visual Studio 2013 Nobemver CTP [12.0]|12.0|1800|180021114|   
|2013 Update 5|Visual Studio 2013 Update 5 [12.0]|12.0|1800|180040629|   
|2013 Update 4|Visual Studio 2013 Update 4 [12.0]|12.0|1800|180031101|   
|2013 Update 3|Visual Studio 2013 Update 3 [12.0]|12.0|1800|180030723|   
|2013 Update 2|Visual Studio 2013 Update 2 [12.0]|12.0|1800|180030501|   
|2013 Update2 RC|Visual Studio 2013 Update2 RC [12.0]|12.0|1800|180030324|   
|2013 Update 1|Visual Studio 2013 Update 1 [12.0]|12.0|1800|180021005|   
|2013|Visual Studio 2013 [12.0]|12.0|1800|180021005|   
|2013 RC|Visual Studio 2013 RC [12.0]|12.0|1800|180020827|   
|2013 Preview|Visual Studio 2013 Preview [12.0]|12.0|1800|180020617|   
|2012 November CTP|Visual Studio 2012 November CTP [11.0]|11.0|1700|170051025|   
|2012 Update 4|Visual Studio 2012 Update 4 [11.0]|11.0|1700|170061030|   
|2012 Update 3|Visual Studio 2012 Update 3 [11.0]|11.0|1700|170060610|   
|2012 Update 2|Visual Studio 2012 Update 2 [11.0]|11.0|1700|170060315|   
|2012 Update 1|Visual Studio 2012 Update 1 [11.0]|11.0|1700|170051106|   
|2012|Visual Studio 2012 [11.0]|11.0|1700|170050727|   
|2010 SP1|Visual Studio 2010 SP1 [10.0]<br>Visual C++ 2010 SP1 [10.0]|10.0|1600|160040219|   
|2010|Visual Studio 2010 [10.0]<br>Visual C++ 2010 [10.0]|10.0|1600|160030319|   
|2010 Beta 2|Visual Studio 2010 Beta 2 [10.0]|10.0|1600|160021003|   
|2010 Beta 1|Visual Studio 2010 Beta 1 [10.0]|10.0|1600|160020506|   
|2008 SP1|Visual Studio 2008 SP1 [9.0]<br>Visual C++ 2008 SP1 [9.0]|9.0|1500|150030729|   
|2008|Visual Studio 2008 [9.0]<br>Visual C++ 2008 [9.0]|9.0|1500|150021022|   
|2008 Beta 2|Visual Studio 2008 Beta 2 [9.0]|9.0|1500|150020706|   
|2005 SP1|Visual Studio 2005 SP1 [8.0]<br>Visual C++ 2005 SP1 [8.0]|8.0|1400|140050727|   
|2005|Visual Studio 2005 [8.0]<br>Visual C++ 2005 [8.0]|8.0|1400|140050320|   
|2005 Beta 2|Visual Studio 2005 Beta 2 [8.0]|8.0|1400|140050215|   
|2005 Beta 1|Visual Studio 2005 Beta 1 [8.0]|8.0|1400|140040607|   
||Windows Server 2003 SP1 DDK (for AMD64)||1400|140040310|   
|2003 SP1|Visual Studio .NET 2003 SP1 [7.1]<br>Visual C++ .NET 2003 SP1 [7.1]|7.1|1310|13106030|   
||Windows Server 2003 SP1 DDK||1310|13104035|   
|2003|Visual Studio .NET 2003 [7.1]<br>Visual C++ .NET 2003 [7.1]|7.1|1310|13103077|   
||Visual Studio Toolkit 2003 [7.1]|7.1|1310|13103052|   
|2003 Beta|Visual Studio .NET 2003 Beta [7.1]|7.1|1310|13102292|   
||Windows Server 2003 DDK||1310|13102179|   
|2002|Visual Studio .NET 2002 [7.0]<br>Visual C++ .NET 2002 [7.0]|7.0|1300|13009466|   
||Windows XP SP1 DDK||1300|13009176|   
|6.0 SP6|Visual Studio 6.0 SP6<br>Visual C++ 6.0 SP6|6.0|1200|12008804|   
|6.0 SP5|Visual Studio 6.0 SP5<br>Visual C++ 6.0 SP5|6.0|1200|12008804|   
||Visual Studio 97 [5.0]<br>Visual C++ 5.0|5.0|1100|   
||Visual C++ 4.2|4.2|1020|   
||Visual C++ 4.1|4.1|1010|   
||Visual C++ 4.0|4.0|1000|   
||Visual C++ 2.0|2.0|900|   
||Visual C++ 1.0|1.0|800|   
||Microsoft C/C++ 7.0||700|   
||Microsoft C 6.0||600|   


> Visual Studio 버전 참고 :
   [https://en.wikipedia.org/wiki/Microsoft_Visual_Studio#Version_history](https://en.wikipedia.org/wiki/Microsoft_Visual_Studio#Version_history){:target="_blank"}
{: .prompt-tip }
