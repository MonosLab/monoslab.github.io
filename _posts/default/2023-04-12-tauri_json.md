---
title: "우당탕탕 타우리 #005💬 JSON 파일 가지고 놀기"
sub: post
author: Kwangsoo Seo
date: 2023-04-12 12:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, Tauri, Windows, JSON]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post30.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post30)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# JSON   
JSON은 JavaScript 객체 리터럴, 배열, 스칼라 데이터를 표현하는 텍스트 기반의 파일이며, 프로그램에서 파싱 및 생성하기가 쉽기 때문에 많은 곳에서 사용되고 있습니다.

## JSON 파일 읽기
타우리의 프로젝트 폴더인 src-tauri 아래에 conf 폴더를 하나 생성하고, 그 폴더에 conf.json 파일을 생성하고 아래의 예시처럼 데이터를 입력하고 저장합니다. (폴더 위치는 자신이 선호하는 위치에 하시면 됩니다. 여기서는 설명을 위한 예시로 conf 폴더를 생성한 것 뿐입니다.)
```json
{
	"url": "https://monoslab.github.io/",
	"port": 443,
	"debug": false
}
```
위의 conf.json 파일을 읽어 오기 위해 tauri.conf.json 파일에 리소스를 추가해 줍니다. 배열에 각각의 파일을 입력하여도 되지만 *를 이용하여 conf 폴더내의 모든 파일을 추가하도록 하겠습니다. 그리고 타우리의 모든 API를 사용하도록 허용 해주겠습니다.
```json
{
  "build": { ... },
  "package": { ... },
  "tauri": {
    "allowlist": {
       "all" : true
    },
    "bundle": {
       ...
      "resources": [
        "conf/*"
      ] 
    }
  }
} 
```
다음으로 Cargo.toml 파일을 열어
```
[dependencies]
...
serde_json = "1.0"
```
를 추가해 줍니다.

이제 main.rs의 소스를 열어,
```rust
use std::env;
use std::fs::File;

 fn main() {
  let resource_path = format!("{}\\conf\\conf.json", parent_path());
  let file = File::open(&resource_path).unwrap();
  let conf:serde_json::Value = serde_json::from_reader(file).unwrap();

  println!("URL : {}", conf.get("url").unwrap());
  println!("PORT : {}", conf.get("port").unwrap());
  println!("DEBUG : {}", conf.get("debug").unwrap());
}

fn parent_path() -> String {
  let path = env::current_exe().unwrap();
  format!("{}", path.parent().unwrap().display())
}
```
위와 같이 코딩 해주고 실행시켜 보면 아래와 같은 결과값을 얻으실 수 있습니다.
```
URL : "https://monoslab.github.io/"
PORT : 443
DEBUG : false
```

## JSON 파일 저장하기     
아래의 코드는 읽어 왔던 JSON 파일 변경하여 저장하는 예제입니다.   
```rust
use std::env;
use std::fs;
use std::str::FromStr;
use serde_json::{Number, Value};

fn main() {
  let resource_path = format!("{}\\conf\\conf.json", parent_path());
  let file = fs::File::open(&resource_path).unwrap();
  let mut conf:Value = serde_json::from_reader(file).unwrap();

  // 읽기
  println!("URL : {}", conf.get("url").unwrap());
  println!("PORT : {}", conf.get("port").unwrap());
  println!("DEBUG : {}", conf.get("debug").unwrap());

  // json 데이터 변경 후 쓰기
  conf["url"] = Value::String(String::from("http://monoslab"));
  conf["port"] = Value::Number(Number::from_str("80").unwrap());
  conf["debug"] = Value::Bool(false);
  _ = fs::write(resource_path, serde_json::to_string_pretty(&conf).unwrap());
}

fn parent_path() -> String {
  let path = env::current_exe().unwrap();
  format!("{}", path.parent().unwrap().display())
}
```
serde_json::Value를 이용하여 json의 자료형으로 변환 후 각각의 키에 값을 넣어주고 파일시스템(fs) API를 통해 파일을 저장하면 됩니다. 빌드 후 conf.json 파일을 열어보면 아래와 같이 우리가 원하는 결과 값으로 저장이 되어 있을 것입니다.
```json
{
  "debug": false,
  "port": 80,
  "url": "http://monoslab"
}
```