# lecture-6-notes

## Generics example 1

*Open generics1.rs*

Imagine we are working on many many different type of numbers, and we want to implement a `the_large_one()` function that can return the largest value of two input values.

Without generic, we need to define this function for every single data type:

```rust
fn the_large_one(x: i8, y:i8) {if x > y {x} else {y}};
fn the_large_one(x: i16, y:i16) {if x > y {x} else {y}};
// ...
```

As a smart coder, we cannot let it happen. So, we define a generic type `T`, and tell rust to do the rest for us.

```rust
fn the_large_one<T>(x: T, y: T) -> T {if x > y {x} else {y}};

```

However, rust will complain because not every data type can be compared. 

```
error[E0369]: binary operation `>` cannot be applied to type `T`
 --> src/main.rs:1:44
  |
1 | fn the_large_one<T>(x: T, y: T) -> T {if x > y {x} else {y}}
  |                                          - ^ - T
  |                                          |
  |                                          T
  |
help: consider restricting type parameter `T`
  |
1 | fn the_large_one<T: std::cmp::PartialOrd>(x: T, y: T) -> T {if x > y {x} else {y}}
  |                   ^^^^^^^^^^^^^^^^^^^^^^

For more information about this error, try `rustc --explain E0369`.
```

How to fix this error? We need trait to tell rust what is our generic type `T`!

## Trait example 1

*open trait1.rs*

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

```

## Trait example 2

*open trait2.rs*

Add the following.

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```
