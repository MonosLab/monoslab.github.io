---
title: "[ RUST ] 프로그램 시작시 콘솔창 숨기기"
sub: post
author: Kwangsoo Seo
date: 2024-08-02 08:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, 러스트, 콘솔창, 숨기기, hide, console]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post46.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post46)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# 프로그램 시작시 콘솔창 숨기기   
프로그램 실행시 콘솔창이 보이지 않게 하기 위한 방법에 대해 설명합니다.      
먼저 Cargo.toml의 dependencies에 winapi 크레잇을 추가합니다.   
```toml   
[dependencies]
winapi = {version = "0.3", features = ["wincon", "winuser"]}
```   
다음으로 작업할 것은 콘솔창이 뜰때 핸들을 얻어와서 숨기는 작업을 해주어야 합니다. 아래의 hide_console_window() 함수는 핸들을 얻어와 윈도우를 숨기는 작업에 관한 내용입니다.   
```rust   
fn hide_console_window() {
    use std::ptr;
    use winapi::um::wincon::GetConsoleWindow;
    use winapi::um::winuser::{ShowWindow, SW_HIDE};

    let window = unsafe {GetConsoleWindow()};
    if window != ptr::null_mut() {
        unsafe {
            ShowWindow(window, SW_HIDE);
        }
    }
}
```   
위의 방법은 간단히 한줄로 표현할 수도 있습니다.   
```rust   
fn hide_console_window() {
    unsafe { winapi::um::wincon::FreeConsole() };
}
```   
두 방법 모두 콘솔창이 띄워졌을 때 바로 숨기는 작업을 합니다. 마지막으로 hide_console_window() 함수를 main 함수에서 호출해 주기만 하면 프로그램 시작시 콘솔창을 바로 숨겨주게 됩니다.   
```rust   
fn main() {
	hide_console_window();
	println!("hello, world!!!");
}
```   
<span style="color:blue">단, 콘솔창에서 프로그램을 실행한 경우(즉, 파일탐색기에서 exe 파일을 실행한 경우가 아닌 경우)에는 콘솔의 소유가 실행한 프로그램이 아니기 때문에 콘솔창이 숨겨지지 않습니다.</span> 일반적으로 파일탐색기(또는 단축아이콘)에서 실행하거나, 윈도우 시작시 자동실행 등은 위의 hide_console_window() 함수를 이용하면 콘솔창이 잘 숨겨지는 걸 확인 하실 수 있습니다.   
