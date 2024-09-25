---
title: "[ RUST ] 스마트 포인터 (smart pointer)"
sub: post
author: Kwangsoo Seo
date: 2024-08-09 08:00:00 +0900
categories: [프로그래밍, RUST]
tags: [RUST, smart, point, 스마트, 포인터]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post47.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post47)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---

# 스마트 포인터   
<span style="color:blue">스마트 포인터</span>는 메모리의 주소 값을 가지는 변수, 즉 포인터에 추가적으로 데이터의 소유와 메타 데이터의 기능도 가지고 있는 데이터 구조입니다.   
<span style="color:blue">스마트 포인터</span>는 구조체(Struct) 타입으로 정의 하며, 포인터가 가르키는 값을 참조하기 위한 <span style="color:red">Deref</span>와 참조한 값의 소유권을 잃을 때 호출될 <span style="color:red">Drop</span> trait을 구현하여야 합니다.    

러스트의 대표적인 스마트 포인터로는 Box, Rc, Arc, Cell 등이 있으며, 아래와 같은 특징을 가집니다.   

|스마트 포인터|소유권|스레드|   
| :---: | :---: | :---: |   
|Box\<T\>|단일 소유권|싱글 스레드|   
|Rc\<T\>|다중 소유권|싱글 스레드|   
|Arc\<T\>|다중 소유권|멀티 스레드|   
|Cell\<T\>|단일 소유권|싱글 스레드|   

## Box\<T\>   
가장 직관적인 스마트 포인터이며, Box는 데이터를 스택이 아니라 힙에 저장할 수 있도록 해줍니다. 스택에 남는 것은 힙 데이터를 가리키는 포인터입니다.   
컴파일시에 크기를 특정할 수 없는 데이터 타입을 가지고 있고, 데이터의 소유권을 이동시킬때 데이터의 복사가 일어나는 것을 원치 않을 경우, 그리고 스택에 큰 데이터를 저장할 때 스택 오버플로우가 예상되는 경우 사용하면 됩니다.   
\<사용 예시\>   
```rust  
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```

## Rc\<T\>   
러스트에서는 대부분의 소유자가 명확합니다. 그러나 하나의 제너릭 데이터 T에 대해 여러 소유자가 필요한 경우도 있습니다. 이럴 경우 소유권을 공유할 수 있는 Rc(Reference counted) 스마트 포인터를 사용하면 됩니다.
\<사용 예시\>   
```rust  
use std::rc::Rc;

fn main() {
    let a = Rc::new(5);
    let b = Rc::clone(&a);
    println!("a: {}, b: {}", a, b);
}
```

## Arc\<T\>   
Arc(Atomically reference counted)는 힙에 할당된 제너릭 데이터 T의 값을 다중 스레드 상황에서 안전하게 공유할 수 있도록 하는 스마트 포인터입니다. Arc는 참조 연산시 원자적 연산을 하게 되는데, 이는 메모리 접근 비용 보다 더 많은 비용을 소모하게 됩니다. 따라서 스레드 상황이 아니라면 Rc를 사용하는 것이 좋습니다.   
> 원자적 연산이란 처리 중간에 다른 연산을 허용하지 않는 연산입니다. 다중 스레드 처리시 "이 만큼 했다"라는 모호함이 없고, 오직 "끝났다"와 "시작전이다"만 존재하는 연산입니다.   

\<사용 예시\>   
```rust  
use std::sync::Arc;
use std::thread;

fn main() {
    let data = Arc::new(vec![1, 2, 3]);
    let dc1 = data.clone();
    let dc2 = data.clone();

    let handle1 = thread::spawn(move || {
        println!("data clone #1: {:?}", dc1);
    });

    let handle2 = thread::spawn(move || {
        println!("data clone #2: {:?}", dc2);
    });

    handle1.join().unwrap();
    handle2.join().unwrap();
}
```

## Cell\<T\> 과 RefCell\<T\>   
Cell, RefCell은 Box와 마찬가지로 단일 소유권을 가지는 스마트 포인터입니다. 단지 차이점은 Box는 제너릭 데이터 T를 가변적 변수로 사용할 수 없는데, 반해 Cell은 불변 변수를 가변 변수처럼 사용이 가능합니다.   
Cell은 어떤 데이터에 대한 불변 참조자가 있더라도 데이터를 변경할 수 있도록 내부 가변성(interior mutability) 디자인 패턴을 제공합니다. 그리고 RefCell의 경우는 데이터 변경이 아닌 참조를 이용하여 데이터를 변경하려고 하면 RefCell을 사용해야 합니다.   
\<사용 예시\>   
```rust     
use std::cell::Cell;

fn main() {
    let c = Cell::new(1);
    println!("set #1 : {}", c.get());
    c.set(2);
    println!("set #2 : {}", c.get());
}
```   
\<사용 예시\>   
```rust     
use std::cell::RefCell;

fn main() {
    let rc = RefCell::new(1);
    println!("borrow #1 : {}", rc.borrow());
    *rc.borrow_mut() += 2;
    println!("borrow #2 : {}", rc.borrow());
}
```   

---

# 나만의 스마트 포인터   
마지막으로 Box와 유사한 스마트 포인터를 구현하는 방법을 알아 보겠습니다.   
```rust   
struct BoxEx<T>(T);

impl<T> BoxEx<T> {
  fn new(x: T) -> BoxEx<T> {
    BoxEx(x)
  }
}
```
Box\<T\>와 같이 제너릭 데이터 T를 사용하도록 하였고, new를 이용하여 힙에 데이터를 할당하도록 하였습니다.   
다음으로 앞에서 잠깐 언급했던 Deref와 Drop을 아래와 같이 구현해 줍니다.
```rust   
use std::ops::Deref; 

impl<T> Deref for BoxEx<T> {
  type t = T;
  
  fn deref(&self) -> &Self::t {
    &self.0
  }
}
```


```rust   
fn main() {
  let a = 5;
  let b = BoxEx::new(a);
  
  println!("BoxEx value : {}", b);
}
```
