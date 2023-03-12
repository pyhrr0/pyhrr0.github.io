---
title: "Rust: polymorphism"
date: 2023-02-03T13:33:37Z
---

Using Enums
-----------

```rust
enum Animal {
    Cow,
    Pig,
    Rabbit,
}

impl Animal {
    fn make_noise(&self) {
        let noise = match self {
            Animal::Cow => "Moo!",
            Animal::Pig => "Oink!",
            _ => "Default noise!",
        };

        println!("{noise}")
    }
}

fn main() {
    let (cow, pig, rabbit) = (Animal::Cow, Animal::Pig, Animal::Rabbit);

    cow.make_noise();
    pig.make_noise();
    rabbit.make_noise();
}
```

Using traits
------------

```rust
trait Animal {
    fn make_noise(&self) {
        println!("Default noise!")
    }
}

struct Cow;
impl Animal for Cow {
    fn make_noise(&self) {
        println!("Moo!")
    }
}

struct Pig;
impl Animal for Pig {
    fn make_noise(&self) {
        println!("Oink!")
    }
}

struct Rabbit;
impl Animal for Rabbit {}

fn static_dispatch(animal: &impl Animal) {
    animal.make_noise();
}

fn dynamic_dispatch(animal: &dyn Animal) {
    animal.make_noise()
}

fn main() {
    let (cow, pig, rabbit) = (Cow, Pig, Rabbit);

    static_dispatch(&cow);
    dynamic_dispatch(&cow);

    static_dispatch(&pig);
    dynamic_dispatch(&pig);

    static_dispatch(&rabbit);
    dynamic_dispatch(&rabbit);
}
```
