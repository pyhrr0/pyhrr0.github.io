---
title: "Rust typestate pattern"
date: 2023-02-25T13:33:37Z
---

The type state pattern can be used to prevent consumers of your API from misusing it. 
It takes advantage of Rust's type system, using generics and zero-sized types.

```rust
use std::marker::PhantomData;

struct Landed;
struct Flying;

struct Aeroplane<State = Landed> {
    state: PhantomData<State>,
}

impl Aeroplane<Landed> {
    fn new() -> Self {
        Self {
            state: Default::default(),
        }
    }

    fn take_off(self) -> Aeroplane<Flying> {
        Aeroplane {
            state: PhantomData::<Flying>,
        }
    }
}

impl Aeroplane<Flying> {
    fn land(self) -> Aeroplane<Landed> {
        Aeroplane {
            state: PhantomData::<Landed>,
        }
    }
}

impl<State> Aeroplane<State> {
    fn adjust_flaps(&self) {
        unimplemented!();
    }
}

fn main() {
    let landed_plane = Aeroplane::new();
    landed_plane.adjust_flaps();

    let flying_plane = landed_plane.take_off();
    flying_plane.adjust_flaps();
    flying_plane.land();
}
```
