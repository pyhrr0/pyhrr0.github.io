---
title: "Rust: generic (smart) pointers using GATs."
date: 2023-03-10T13:33:37Z
---

Generic associated types (GATs) allow you to define type, lifetime or const generics on
associated types. An example use case can be found below:

```rust
use std::ops::Deref;
use std::rc::Rc;
use std::sync::{Arc, Mutex};

trait SmartPointer {
    type Pointer<Target>: Deref<Target = Target> + Clone;

    fn new<T>(value: T) -> Self::Pointer<T>;
}

struct NonAtomic;

impl SmartPointer for NonAtomic {
    type Pointer<Target> = Rc<Target>;

    fn new<T>(value: T) -> Rc<T> {
        Rc::new(value)
    }
}

struct Atomic;

impl SmartPointer for Atomic {
    type Pointer<Target> = Arc<Target>;

    fn new<T>(value: T) -> Arc<T> {
        Arc::new(value)
    }
}

#[derive(Debug)]
struct Foo<P: SmartPointer> {
    value: P::Pointer<Mutex<u32>>,
}

impl<P: SmartPointer> Clone for Foo<P> {
    fn clone(&self) -> Self {
        Self {
            value: self.value.clone(),
        }
    }
}

impl<P: SmartPointer> Foo<P> {
    fn new(value: u32) -> Self {
        Self {
            value: P::new(Mutex::new(value)),
        }
    }
}

fn main() {
    let rc = Foo::<NonAtomic>::new(0);
    *rc.value.lock().unwrap() = 42;

    let arc = Foo::<Atomic>::new(0);
    *arc.value.lock().unwrap() = 42;

    println!("Foo<Atomic>: {:?}", *rc.value.lock().unwrap());
    println!("Foo<NonAtomic>: {:?}", *arc.value.lock().unwrap());
}
```
