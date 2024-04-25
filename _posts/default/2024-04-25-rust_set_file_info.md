---
title: "[ RUST ] 앱 정보 추가 및 관리자 실행 권한 부여"
sub: post
author: Kwangsoo Seo
date: 2024-04-25 08:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, 버전, 라이선스, 앱 정보, 실행 권한, UAC]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post42.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post42)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# 앱 정보 추가하기   
winres 크레잇을 이용하여 앱의 속성에 표시되는 정보들을 입력하는 방법에 대해 알아 보려고 합니다.   
복잡하지 않으니 아래의 순서대로만 따라하시면 쉽게 앱의 속성을 추가하실 수 있습니다.

## Cargo.toml 셋팅   
```xml
[package]
name = "winres_sample"
version = "1.0.0"
edition = "2021"
build = "build.rs"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[package.metadata.winres]
FileDescription = "descript"
#FileVersion = "1.0.0.0"
ProductName = "ToolBox"
ProductVersion = "1.0.0.1"
LegalCopyright = "© 2024 Monoslab. All rights reserved."
OriginalFilename = "ToolBox.exe"
CompanyName = "Monoslab"
Comments = "Monoslab ToolBox"
InternalName = "MonoslabToolBox.exe"

[dependencies]
...

[build-dependencies]
winres = "0.1"
```
위와 같이 [build-dependencies]에 winres = "0.1"을 추가하고 [package.metadata.winres] ... 를 추가합니다.
FileVersion은 주석 처리를 하였는데, 이는 [package]의 version이 우선시 되기 때문에 FileVersion에 어떠한 값을 추가하여도 [package]의 version값으로 표시가 되기 때문입니다. 입력 후 빌드를 해 보시고 생성된 실행 파일의 <span style="color:blue">"속성 > 자세히"</span>를 선택하시면 아래와 같이 내용들이 추가 되어 있는 것을 확인 하실 수 있습니다.
![file_info](https://monoslab.github.io/assets/img/posts/rust_file_info.png)  

## 앱 아이콘 설정 및 관리자 권한으로 실행   
앱의 실행 파일 아이콘 및 관리자 권한으로 앱을 실행하도록 설정하기 위해서는 Cargo.toml 파일이 있는 폴더에 build.rs 파일과 assets 폴더 및 toolbox.ico 파일을 생성하고 아래의 예제와 같이 작업을 해줍니다.


```rust
extern crate winres;

const DEF_MANIFEST: &str = r#"
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
<trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
    <security>
        <requestedPrivileges>
            <requestedExecutionLevel level="requireAdministrator" uiAccess="false" />
        </requestedPrivileges>
    </security>
</trustInfo>
</assembly>
"#;

fn main() {
    if cfg!(target_os = "windows") {
        let mut res = winres::WindowsResource::new();
        res.set_icon("assets/toolbox.ico");
        res.set_manifest(DEF_MANIFEST);
        res.compile().unwrap();
    }
}
```   
build.rs를 저장하고 `cargo build --release`를 입력하여 빌드합니다.
정상적으로 빌드가 되었다면 실행파일의 아이콘이 assets 폴더에 추가했던 아이콘으로 변경이 되고 아이콘 우측하단에 방패 모양(관리자 권한 실행 적용)이 생긴 것을 볼 수 있을 것입니다.