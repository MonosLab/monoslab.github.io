---
title: "[ Error ] Visual Studio C# Error 해결 방법" 
sub: post
author: Kwangsoo Seo
date: 2022-10-18 06:00:00 +0900
categories: [정보, 에러]
tags: [Error, solved]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post15.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post15)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# <span style="color:red">***cs1617***</span>   
> ***<span style="color:red">csc : error cs1617: '8.0'은(는) /langversion의 유효한 옵션이 아닙니다. '/ langversion:?'를 사용하여 지원되는 값을 나열하세요.</span>***   
<span style="color:black">해결방법 : <span style="background-color: #FFF5B1">프로젝트 > 속성 > 빌드 > 고급</span>에서 <span style="background-color: #FFF5B1">언어 버전</span>을 <span style="background-color: #FFF5B1">C# 최신 부 버전(최신)</span>으로 선택하면 해결이 됩니다. 이렇게 하면 현 Visual Studio에서 사용하고 있는 가장 최근의 C# 언어 버전을 사용할 수 있게 됩니다.</span>   

---
# <span style="color:red">***MSB3323***</span>   
> ***<span style="color:red">error MSB3323: Unable to find manifest signing certificate in the certificate store.</span>***   
> <span style="color:red">warning MSB3327: Unable to find code signing certificate in the current user’s Windows certificate store. To correct this, either disable signing of the ClickOnce manifest or install the certificate into the certificate store.
</span>   
> <span style="color:black">해결방법 : 툴(Visual Studio C#)로 프로젝트를 열고 <span style="background-color: #FFF5B1">프로젝트의 ".csproj" 파일</span>을 에디터로 열어서 아래의 항목들을 찾아 제거 합니다.</span>   

```   
<PropertyGroup>
  <ManifestCertificateThumbprint>UUID(헥사코드 값)</ManifestCertificateThumbprint>
</PropertyGroup>
<PropertyGroup>
  <SignManifests>true</SignManifests>
</PropertyGroup>
<PropertyGroup>
  <ManifestKeyFile>monoslab.pfx</ManifestKeyFile>
</PropertyGroup>
```   
> <span style="color:black">저장을 하고 툴로 돌아오면 아래와 같은 메시지창이 출력되며, "예(Y)"를 눌러 기존 pfx 파일을 갱신해줍니다.</span>   
> <span style="color:red">A file with the name 'D:\Git\Monoslab\VistoApp_TemporaryKey.pfx' already exists. Do you want to replace it?</span>   
> ✳️ 참고   
> SignManifests와 ManifestKeyFile를 먼저 삭제하고, 콘솔창에서 해당 프로젝트 이동 후   
> $> msbuild /fl1 /t:rebuild   
> $> msbuild /fl2 /t:publishonly   
> 를 입력하여 빌드를 진행한 후 툴로 프로젝트를 열고 에디터창에서 ManifestCertificateThumbprint를 삭제하였습니다.
> 혹시나 정상작동 되지 않을 경우 이 "참조" 내용을 참고하여 진행하시기 바랍니다.   

