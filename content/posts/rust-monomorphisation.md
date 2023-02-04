---
title: "Rust monomorphisation"
date: 2023-02-04T13:33:37Z
---

```rust
use std::ops::Add;

fn add<T: Add<Output = T>>(a: impl Into<T>, b: impl Into<T>) -> T {
    a.into() + b.into()
}

fn main() {
    add::<i64>(42_i64, 42_i32);
    add::<i32>(42_i32, 42_i16);
    add::<i16>(42_i16, 42_i8);
    add::<i8>(42_i8, 42_i8);
    add::<f64>(42, 4.2);
    add::<f64>(4.2, 42);
}
```

```shell
$ objdump -C -t target/debug/my_binary | grep my_binary::add
0000000000009260 l     F .text	00000000000000b0              my_binary::add
0000000000009310 l     F .text	00000000000000c4              my_binary::add
0000000000009020 l     F .text	00000000000000bb              my_binary::add
00000000000093e0 l     F .text	00000000000000cb              my_binary::add
00000000000090e0 l     F .text	00000000000000bb              my_binary::add
00000000000091a0 l     F .text	00000000000000b4              my_binary::add
```
