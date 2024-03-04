---
title: "[ RUST ] 포맷터(Formatter)와 린터(Linter)"
sub: post
author: Kwangsoo Seo
date: 2024-03-04 08:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, 포맷터, 린터, formatter, linter]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post41.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post41)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# Rust 표준 포맷터(Formatter)와 린터(Linter)   
2019년 6월 Tidelift와 The New Stack에서 소프트웨어 개발자를 대상으로 설문조사를 한 내용으로 보면, 새 코드를 작성하거나 기존 코드를 개선하는데 32%의 시간을 사용하고 코드 유지 관리를 위해 19%, 그 외 테스트나 보안 문제 대응에 16%를 사용한다고 합니다.   
![formatter&linter](https://monoslab.github.io/assets/img/posts/developer_spend_time.png)   
(* 참조 사이트 : https://thenewstack.io/how-much-time-do-developers-spend-actually-writing-code/ )   

이 통계를 보면 소프트웨어 개발자들은 평균 51% 정도의 시간에 코드를 개발/개선 또는 유지 관리를 위해 할애하고 있습니다. 이처럼 짧은 시간에 효율적인 코딩을 하기 위해서는 일관된 코딩 스타일로 작업하는 것이 필수적입니다. 협업을 하는 경우에는 더더욱 그러하구요. 일관된 코드는 가독성을 높여 적은 노력과 적은 시간으로 원하는 결과를 얻어 낼 수가 있기 때문입니다. 일관된 코드를 작성하기 위해서는 어떠한 규칙을 정하고 정한 규칙에 대해 올바르게 따르고 있는지 확인해 줄 도구가 필요한데, 이 도구가 포맷터와 린터입니다. 포매터와 린터에 규칙을 잘 세워두면 일관된 코드를 만드는데 도움이 됩니다.   
* 포매터(Formatter) : 작성한 코드를 정해진 규칙(들여쓰기 등)에 따라 코드 스타일을 교정하기 위한 도구.   
* 린터(Linter) : 코드의 구조를 검사해서 버그가 날 수 있을 만한 코드, 스타일 오류, 의심스러운 구조 등을 찾아주는 도구.   
> **<span style="color:blue">일관된 코드 스타일을 유지하려면 포맷터를 ..., 코드의 구조와 잠재적 문제를 해결하려면 린터를 사용하라!</span>**

## Rust 표준 포맷터(Formatter) : rustfmt   
개개인의 다양한 코딩 스타일로 코딩된 코드를 Rust 팀에서 개발한 rustfmt을 이용하여 정형화된 코드 스타일을 유지할 수 있게 해줍니다. 아래와 같은 방법으로 설치를 합니다.
```
\> rustup component add rustfmt
```   
사용은 아래와 같이 명령을 실행해 줍니다.
```
\> cargo fmt
```   
위의 명령이 잘 수행되었다면, 소스 코드를 열어 아름답게 정리(?)된 코드를 확인 하실 수 있을 것입니다.
이외에도 Cargo.toml 파일에 'rustfmt-nightly = "1.4.21"'를 추가하고 코드 내에서 정리가 필요하지 않은 부분들은 설정을 통해 skip등의 작업을 수행할 수도 있습니다. (자세한 내용은 https://crates.io/crates/rustfmt-nightly 를 참조 하세요.)

## Rust 표준 린터(Linter) : clippy   
프로그래밍 언어에는 잠재적 문제를 유발할 수 있는 코드나 추천 하지 않는 방법을 사용하는 경우를 체크해서 경고하여 주는 린트(lint)라는 프로그램이 있습니다. Python의 pylint나 ECMAScript의 eslint 처럼 Rust에서도 Clippy라는 린터가 존재합니다. Rust의 1.78.0 버전 미만에서는 아래와 같은 방법으로 설치를 합니다. (1.78.0 버전 이상부터는 포함되어 배포되기 때문에 별도로 설치하지 않습니다.)
```
\> rustup component add clippy
```   
아래의 명령을 실행해 줍니다.
```
\> cargo clippy
```   
이외에도 Cargo.toml 파일에 'clippy = "0.0.302"' (0.0.302는 사용할 버전)를 추가해 주고 코드 내에서 clippy와 관련하여 경고나 특정 코드를 허용할 수도 있습니다. (자세한 내용은 https://github.com/rust-lang/rust-clippy#clippy 를 참조 하세요.)   
clippy를 사용하면, 추천하지 않는 방법으로 코딩한 경우에는 아래와 같은 경고와 해결 방안에 대해서 친절히 안내 받을 수 있습니다.

```rust
warning: you seem to be trying to use `match` for destructuring a single pattern. Consider using `if let`
  --> src\main.rs:53:44
   |
53 |           .on_system_tray_event(|app, event| match event {
   |  ____________________________________________^
54 | |             SystemTrayEvent::MenuItemClick { id, .. } => match id.as_str() {
55 | |                 "quit" => {
56 | |                     std::process::exit(0);
...  |
68 | |             _ => {}
69 | |         })
   | |_________^
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#single_match
   = note: `#[warn(clippy::single_match)]` on by default
help: try this
   |
53 ~         .on_system_tray_event(|app, event| if let SystemTrayEvent::MenuItemClick { id, .. } = event { match id.as_str() {
54 +             "quit" => {
55 +                 std::process::exit(0);
56 +             }
57 +             "settings" => {
58 +                 let window = app.get_window("main").unwrap();
59 +                 window.emit("trayEvent", "settings").unwrap();
60 +             }
61 +             "refresh" => {
62 +                 let window = app.get_window("main").unwrap();
63 +                 window.emit("trayEvent", "refresh").unwrap();
64 +             }
65 +             _ => {}
66 ~         } })
   |
```

> 린터(Linter)는 코드의 품질을 향상 시켜주어 현장에서 많이 사용되고 있는 도구이지만 실제로는 너무 엄격한 잣대로 인해 오히려 많은 개발 시간을 투자하게 만들지도 모릅니다. 하지만 그 보다 더 큰 잇점인 코드의 품질 향상이나 일관성, 가독성 등을 높여주기 때문에 사용하는 것을 권장합니다.
