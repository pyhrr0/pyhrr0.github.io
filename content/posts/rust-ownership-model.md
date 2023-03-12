---
title: "Rust: ownership model"
date: 2023-01-28T13:33:37Z
---

To avoid memory leaks and data race conditions, Rust utilises a unique ownership model,
which governs how memory is managed. It's a game-changer when it comes to safe,
efficient, and easy-to-reason-about code.

Here's how it works:

- Each value in Rust has a variable that's called its owner.
- Only the owner can decide when the value will be dropped (deallocated from memory).
- When a value goes out of scope, it is automatically dropped.

Rust also has a borrow checker, which ensures that:

- While a value is borrowed, the owner can't use it.
- A value can be borrowed immutably or mutably.
- A value can be borrowed only once or multiple times.

These strict rules of ownership and borrowing prevent data races and other undefined
behaviors, making your code more stable and efficient.

```rs
fn main() {
    let mut s = String::from("hello")
    let r1 = &s; // Immutable borrow
    let r2 = &s; // Immutable borrow
    println!("{} and {}", r1, r2);
    // s.push_str(", world"); // This would cause a compile-time error
}
```

The code creates a mutable variable s of type String and assigns the value "hello" to
it. Then it borrows s immutably and assigns it to r1 and r2. The code would not compile
if we try to mutate s while it is borrowed, this is how the borrow checker prevents data
races.
