---
title: "Rust: builder pattern"
date: 2023-02-07T13:33:37Z
---
The Builder pattern is a creational design pattern that allows for the creation of
complex objects step by step, without exposing the internal representation of the object
being built. The pattern is especially useful in situations where the object being
created has multiple optional parameters, as it allows for easy creation of objects with
different combinations of options.

```rust
#[derive(Debug)]
struct Pizza {
    dough: String,
    sauce: String,
    toppings: Vec<String>,
}

#[derive(Default)]
struct PizzaBuilder {
    dough: String,
    sauce: String,
    toppings: Vec<String>,
}

impl PizzaBuilder {
    fn new() -> Self {
        Self::default()
    }

    fn with_dough(mut self, dough: &str) -> Self {
        self.dough = dough.to_owned();
        self
    }

    fn with_sauce(mut self, sauce: &str) -> Self {
        self.sauce = sauce.to_owned();
        self
    }

    fn with_topping(mut self, topping: &str) -> Self {
        self.toppings.push(topping.to_owned());
        self
    }

    fn build(self) -> Pizza {
        Pizza {
            dough: self.dough,
            sauce: self.sauce,
            toppings: self.toppings,
        }
    }
}

fn main() {
    let pizza = PizzaBuilder::new()
        .with_dough("thin crust")
        .with_sauce("tomato")
        .with_topping("mozzarela cheese")
        .with_topping("olives")
        .build();

    println!("{pizza:#?}")
}
```
