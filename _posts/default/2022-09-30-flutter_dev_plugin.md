---
title: "[ Flutter ] 플러터를 이용한 윈도우용 앱 개발 (1)" 
sub: post
author: Kwangsoo Seo
date: 2022-09-30 06:00:00 +0900
categories: [Flutter]
tags: [Flutter 3.0, 플러터 3.0, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post14.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post14)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  
# Windows 네이티브 앱 개발   
Flutter(플러터)는 여러 웹앱과 마찬가지로 웹 컨텐츠를 네이티브 컨테이너(Native container)로 감싸고 있습니다. 내부적으로 네이티브 플랫폼과의 브릿지를 제공합니다.  

---

## Windows UI 지원   
Microsoft의 Fluent design system 규칙을 따르는 앱을 개발할 수 있도록 하기 위해 fluent_ui 패키지를 제공하고 있습니다. fluent_ui 패키지에는 네비게이션 뷰, 컨텐츠 다이얼로그, 플라이아웃, 날짜 선택, 트리뷰 위젯등(navigation views, content dialogs, flyouts, date pickers, and tree view widgets)을 제공합니다.
또한 Flutter 앱에서 사용할 수 있도록 수천 개의 Fluennt 아이콘을 fluentui_system_icons 패키지를 통해 제공합니다. 이외에도 여러가지 유용한 패키지들이 많지만 마지막으로 bitsdogo_window 패키지를 이용하시면 제목 표시줄을 바꾸는데 용이 합니다. 더 많은 패키지 정보는 아래의 "데스크탑 패키지 및 프로젝트들"의 링크를 참고하세요.   

> [데스크탑 패키지 및 프로젝트들 (Github)](https://github.com/leanflutter/awesome-flutter-desktop){:target="_blank"}  

---

## 다양한 샘플들   
플러터 공식 사이트에서 제공하는 갤러리 및 Github 샘플입니다.
* [갤러리](https://gallery.flutter.dev/#/){:target="_blank"}  
* [샘플(Repository)](https://github.com/flutter/gallery){:target="_blank"}  
* [샘플](https://flutter.github.io/samples/#){:target="_blank"}  

아래는 데스크탑 앱으로 개발된 앱들입니다.   
* [Top flutter desktop apps(#1)](https://medium.com/flutter-africa/flutter-desktop-inspiration-1-cc7234f57159){:target="_blank"}  
* [Top flutter desktop apps(#2)](https://medium.com/flutter-africa/top-flutter-desktop-apps-2-ac8e0f6da997){:target="_blank"}  

---   

## Plugin 개발   
아래는 플러그인을 프로젝트에 추가하는 방법과 윈도우의 버전 정보를  플러터에서 호출하는 예제입니다.
아래와 같은 명령을 입력하면 자동적으로 버전 정보를 얻어 오는 플러그인을 샘플 코드로 생성해줍니다.

flutter create -t plugin --platforms=windows,macos example_plugin

```   
> flutter create -t plugin --platforms=windows,macos example_plugin
Creating project example_plugin...
Running "flutter pub get" in example_plugin...                   1,919ms
Running "flutter pub get" in example...                          2,337ms
Wrote 74 files.

All done!
```   

pubspec.yaml에서

```   
  plugin:
    platforms:
      macos:
        pluginClass: ExamplePlugin
      windows:
        pluginClass: ExamplePluginCApi
```   

와 같이 잘 적용되었는지 확인 합니다.   
lib 폴더 아래의 example_plugin.dar 파일을 열어 보시면, 아래와 같이 샘플로 만들어둔 getPlatformVersion 함수가 보일것입니다.   
코드 중에 Future[^footnote_1]는 싱글스레드 환경에서 비동기 처리를 위해 존재합니다.

```   
class ExamplePlugin {   
  Future<String?> getPlatformVersion() {   
    return ExamplePluginPlatform.instance.getPlatformVersion();   
  }   
}   
```   

이 getPlatformVersion()는 각 플랫폼(각 개발언어)의 plugin에 HandleMethodCall의 getPlatformVersion 함수와 연결이 됩니다.   
예를 들어 C++ plugin의 getPlatformVersion 진입점은 아래와 같이 example_plugin.cpp 파일에 정의되어 있습니다.   

```cpp   
void ExamplePlugin::HandleMethodCall(   
    const flutter::MethodCall<flutter::EncodableValue> &method_call,   
    std::unique_ptr<flutter::MethodResult<flutter::EncodableValue>> result) {   
  if (method_call.method_name().compare("getPlatformVersion") == 0) {   
    std::ostringstream version_stream;   
    version_stream << "Windows ";   
    if (IsWindows10OrGreater()) {   
      version_stream << "10+";   
    } else if (IsWindows8OrGreater()) {   
      version_stream << "8";   
    } else if (IsWindows7OrGreater()) {   
      version_stream << "7";   
    }   
    result->Success(flutter::EncodableValue(version_stream.str()));   
  } else {   
    result->NotImplemented();   
  }   
}   
```   

excample_plugin.cpp에는 호출된 함수가 getPlatformVersion 함수인지 비교하여 같으면 버전 정보를 리턴하여 주도록 하는 간단한 예제가 샘플로 들어가 있는것을 보실 수가 있습니다. dart와 각 플랫폼별 HandleMethodCall에서 getPlatformVersion 샘플을 참고하여 함수들을 생성하여 필요한 정보들을 획득하거나 어떤 특정 행동을 수행할 수 있습니다.


---   

[^footnote_1]: Future : 어떤 동작이 완료되지 않았더라도 다음 동작을 수행할 수 있도록 해줍니다.