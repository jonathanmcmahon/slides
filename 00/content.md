# CIS 198: Intro to COBOL #

![]({{ site.baseurl }}/img/cobol.png)

---
# CIS 198: Rust Programming #

![]({{ site.baseurl }}/img/rust.png)

---
# Lecture 00: Hello, Rust! #

![]({{ site.baseurl }}/img/ferris.png)

---
## Overview ##

"Rust is a systems programming language that runs blazingly fast, prevents
nearly all segfaults, and guarantees thread safety." -- [rust-lang.org](https://www.rust-lang.org/)

---
### What _is_ Rust? ###

Rust is:
- Fast
- Safe
- Functional
- Zero-cost

---
### Fast ###

- Rust compiles to native code
- Rust has no garbage collector
- Most abstractions have zero cost
- Fine-grained control over lots of things
- Pay for exactly what you need...
- ...and pay for most of it at compile time

---
### Safe ###

- No null
- No uninitialized memory
- No dangling pointers
- No double free errors
- No manual memory management!

---
### Functional ###

- First-class functions
- Trait-based generics (Java interfaces; composable like Haskell typeclasses)
- Algebraic datatypes
- Pattern matching

---
### Zero-Cost 100% Safe Abstractions ###

- Rust's defining feature
- Strict compile-time checks remove need for runtime
- Big concept: Ownership

---
### Release Model

- Rust has a new stable release every six weeks
- Nightly builds are available, well, nightly
- Current stable: Rust 1.5
- Rust 1.6 will be out tomorrow (1/21)!
    - This is the version we'll be using in this class

Date | Stable | Beta | Nightly
--- | --- | --- | ---
2015-12-10 | 🚂 1.5 | 🚆 1.6 | 🚝 1.7
2016-01-21 | 🚆 1.6 | 🚝 1.7 | 🚈 1.8
2016-03-03 | 🚝 1.7 | 🚈 1.8 | 🚅 1.9

---
### Development

- Rust is developed by Mozilla Research.
- With very active community involvement -- on GitHub, Reddit, irc.
    - [Rust Source](https://github.com/rust-lang/rust/)
    - [Rust Internals Forum](https://internals.rust-lang.org/)
    - [/r/rust](http://www.reddit.com/r/rust)

---
## Administrivia (TODO)

- X homeworks (xx%), final project (xx%)
- Weekly Rust lecture: Wed. 4:30-6:00pm, Towne 321
- Mini-Course lecture: Tue. 6:00-7:30pm, Berger Auditorium (SKIR AUD)
- [Piazza](https://piazza.com/class/iiksjduyiy773s)

Consult [the website](http://cis198-2016s.github.io/) for homework assignments and further details.

---
## Helpful Links ##

- [Official Rust Docs](https://doc.rust-lang.org/stable/std/)
- [The Rust Book (our course textbook)](https://doc.rust-lang.org/stable/book/)
- [Rust By Example](http://rustbyexample.com/)
- [Rust Playpen](https://play.rust-lang.org/)

---
## Let's Dive In! ##

Hello, Rust!
```rust
fn main() {
    println!("Hello, CIS 198!");
}
```

---
# Basic Rust Syntax #

---
### Variable Bindings ###
- Variables are bound with `let`:
```rust
let x = 17;
```

- Bindings are implicitly-typed: the compiler infers based on context.
- The compiler can't always determine the type of a variable, so sometimes you
  have to add type annotations.
```rust
let x: i16 = 17;
```

- Variables are inherently immutable:

    ```rust
    let x = 5;
    x += 1; // error: re-assignment of immutable variable x

    let mut y = 5;
    y += 1; // OK!
    ```

---
### Variable Bindings ###
- Bindings may be shadowed:
```rust
let x = 17;
let y = 53;
let x = "Shadowed!";
// x is not mutable, but we're able to re-bind it
```

- The shadowed binding for `x` above lasts until it goes out of scope.
- Above, we've effectively lost the first binding, since both `x`s are in the same scope.

- Patterns may also be used to declare variables:
```rust
let (a, b) = ("foo", 12);
```
(more on this later)

---
### Expressions ###

- (Almost!) everything is an expression: something which returns a value.
   - Exception: variable bindings are not expressions.
- Turn an expression into a statement by adding a semicolon (discarding its value).

- The "nothing" value is called "unit", which is written `()`. Statements return `()`.
- If a function has no explicit return value, it returns `()`.

    ```rust
    fn foo() {
    }

    fn foo() -> () {
    }
    ```

---
### Expressions ###
- Because everything is an expression, we can bind many things to variable names:
```rust
let x = -5;
let y = if x > 0 { "greater" } else { "less" };
println!("{}", y); // "less"
```

- Aside: `"{}"` is Rust's string interpolation operator
    - Similar to Python, Ruby, C#, and others; like `printf`'s `"%s"` in C/C++.

---
### Primitives ###

- `bool`: spelled `true` and `false`.
- `char`: spelled like `'c'` or `'😺'` (`chars` are Unicode!).

- Numerics: specify the signedness and size.
    - `i8`, `i16`, `132`, `i64`, `isize`
    - `u8`, `u16`, `132`, `u64`, `usize`
    - `f32`, `f64`
    - Type inference for numeric literals will default to `i32` or `f64`.

---
### References ###

- References are written with an `&`, much like in C++
- References can be _dereferenced_ with `*` like in C/C++
- References are guaranteed to always be valid
    - Validity is enforced through compile-time checks!
- These are *not* the same as pointers!
- Reference lifetimes are pretty complex, as we'll explore later on in the course.

```rust
let x = 12;
let ref_x = &x;
println!("{}", *x); // 12
```

---
### Arrays ###
- Arrays are generically of type `[T; N]`
    - N is a compile-time _constant_. Arrays cannot be resized.
    - Array access is bounds-checked at _compile time_.
- Arrays are indexed just like in C/C++/Java/etc: `arr[3]` gives you the 4th element of `arr`

    ```rust
    let arr1 = `[1, 2, 3]`; // (array of 3 elements)
    let arr2 = `[2; 32]`;   // (array of 32 `2`s)
    ```

---
### Slices ###
- A "view" into an array by reference
- Generically of type `&[T]`
- Not created directly, but are borrowed from other variables
- Mutable or immutable
- How do you know when a slice is still valid? Coming soon...

    ```rust
    let arr = [0, 1, 2, 3, 4, 5];
    let total_slice = &arr[..];     // Slice all of `arr`
    let partial_slice = &arr[2..5]; // [2, 3, 4]
    ```
---
### Strings ###
- Two types of Rust strings: `String` and `str`
- `String` is a heap-allocated, growable vector of characters
- `str` is an unsized type that's used to slice `String`s as an `&str`
- String literals like `"foo"` are of type `&str`
    - To make a `String` from an `&str`, use `"foo".to_string()` or `String::from("foo")`

    ```rust
    let s: &str = "foo";
    let s1: String = "foo".to_string();
    let s2: String = String::from("foo");
    ```

---
### Tuples ###
- Fixed-size, ordered, heterogeneous lists
- Index into tuples with `foo.0`, `foo.1`, etc.
- Can be destructured in `let` bindings
    ```rust
    let foo: (i32, char, f64) = (72, 'H', 5.);
    let (x, y, z) = (72, 'H', 5.);
    let (a, b, c) = foo;
    ```

---
### Casting ###

- Cast between types with `as`:

```rust
let x: i32 = 100;
let y: u32 = x as u32;
```

- Naturally, you can only cast between types that are safe to cast between.
    - No casting `[i16; 4]` to `char`!
    - There are unsafe mechanisms to overcome this, if you know what you're doing.

---
## Control Flow ##

---
### If Statements ###

```rust
if x > 0 {
    10
} else if x == 0 {
    0
} else {
    println!("Not greater than zero!");
    -10
}
```
---
### Loops ###
- Loops come in three flavors: `while`, `loop`, and `for`
- `while` works just like you'd expect:

    ```rust
    let mut x = 0;
    while x < 100 {
        x += 1;
        println!("x: {}", x);
    }
    ```

---
### Loops ###
- `loop` is equivalent to `while true`, but the compiler can take advantage of
  knowing that it's infinite

    ```rust
    let mut x = 0;
    loop {
        x += 1;
        println!("x: {}", x);
    }
    ```

---
### Loops ###
- `for` is the most different from most C-like languages
     - `for` loops use an _iterator expression_:

    ```rust
    // 0..10 is an iterator from 0 to 10 (exclusive)
    for x in 0..10 {
        println!("{}", x);
    }

    let xs = [0, 1, 2, 3, 4];
    // Arrays can be used as iterators.
    for x in xs {
        println!("{}", x);
    }
    ```

- `break` and `continue` exist just like in most languages

---
### Functions ###

```rust
fn foo(x: T, y: U, z: V) -> T {
    // ...
}
```

- `foo` is a function that takes three parameters:
    - `x` which is of type `T`
    - `y` which is of type `U`
    - `z` which is of type `V` 
    - It then returns a `T`.

- Must explicitly define the types of function arguments and return value.
    - The compiler is actually smart enough to figure this out for you, but Rust's designers decided it was
      better practice to force explicit function typing.
- The final expression in a function is its return value.
    - Use `return` for _early_ returns from a function.

---
### Functions ###
```rust
fn square(n: i32) -> i32 {
    n * n
}

fn square(n: i32) -> i32 {
    if n < 5 {
        return n;
    }
    n * n
}

fn square(n: i32) -> i32 {
    n * n;
}
```

- The last one won't even compile!
- Why? Its last statement ends in a semicolon, so it evaluates to `()`.

---
### Bonus: Function Pointers ###
- Much simpler than in C:

    ```rust
    let x: fn(i32) -> i32 = square;
    ```

---
### `Vec<T>` ###

- A `Vec` (pronounced "vector") is a growable array allocated on the heap
    (cf. Java ArrayList, C++ std::vector, etc.)
- `<T>` denotes a generic type.
    - The type of a `Vec` of `i32`s is `Vec<i32>`.
- Vectors can be created with `Vec::new()` or with the `vec!` macro:
    - `Vec::new()` is an example of namespacing. `new` is a function defined in
      the `Vec` struct.

---
### `Vec<T>` ###
```rust
// v1 and v2 are equal
let mut v1 = Vec::new();
v1.push(1);
v1.push(2);
v1.push(3);

let v2 = vec![1, 2, 3];
```

```rust
// v3 and v3 are equal
let v3 = vec![0; 4];
let v4 = vec![0, 0, 0, 0];

let i = v2[2]; // 3
```

---
### `Vec<T>` ###
- Vectors can be indexed with `v2[3]`
    - You can't index a vector with an i32/i64/etc. You must use a `usize`
      because `usize` is guaranteed to be the same size as a pointer

- Vectors have other methods on them which you may find useful. They can be
    found in the offical Rust documentation [here](https://doc.rust-lang.org/stable/std/vec/)

---
### Macros!

- Macros are like functions, but they're named with an `!` at the end.
- Can do generally very powerful stuff.
- Call and use them like functions.
    - They actually generate code at compile time!
- You can define your own with `macro_rules! macro_name` blocks.
    - These are very complicated.

---
### `print!` & `println!` ###
- Print stuff out
- Use `{}` for string interpolation, and `{:?}` for debug printing

    ```rust
    print!("{}, {}, {}", "foo", "3", true);
    // foo, 3, true
    println!("{:?}, {:?}", "foo", [1, 2, 3]);
    // "foo", [1, 2, 3]
    ```

---
### `format!` ###
- Uses `println!`-style string interpolation to create formatted `String`s

    ```rust
    let formatted = format!("{}, {:x}, {:?}", 12, 15, Some("Hello"));
    // formatted == "12, f, Some("Hello")"
    ```

---
### `panic!(msg)` 
- Exits current task with given message. Similar to segfaulting.
- Don't do this lightly! It is better to handle and report errors explicitly.

---
#### `assert!(bool)` & `assert_eq!(expected, actual)`
- `panic!`s if `bool` is false or `expected != actual`
- Useful for testing

---
### Match statements ###

```rust
let x = 3;

match x {
    1 => println!("one fish"),
    2 => {
        println!("two fish")
    },
    _ => println!("no fish"),
}
```
*Notes:*
- `match` takes an expression (`x`) and then branches on a list of `value => expression` statements.
- Anything that evaluates to an expression is allowed on the right.
    - A single expression should end with a comma
    - Multiple lines need braces (comma optional)
- `_` represents the wildcard pattern (similar to Haskell, OCaml).

---
### Match statements ###
```rust
let x = 3;
let y = -3;

match (x, y) {
    (1, 1) => println!("one"),
    (2, _) => println!("two"),
    (_, 3) => println!("three"),
    (i, j) if i > 5 && j < 0 => println!("Guard condition"),
    (_, _) => println!("something else"),
}
```

- The expression can be any expression, including tuples and function calls.
- You _must_ have an exhaustive match: the compiler will complain if you don't.
- use `if`-guards to match on certain conditions instead of specific values.
- Patterns are very complex, as we'll see later.

---
## Rustc ##

- Rust's compiler is named `rustc`
- Simply run `rustc your_program.rs` to compile-- things like warnings are
    enabled by default
- Typically, you'll use `cargo`, Rust's package manager, to build instead

---
## Cargo ##

- Rust's package manager & build tool
- Create a new project:
    - `cargo new project_name` (library)
    - `cargo new project_name --bin` (executable)
- Build your project: `cargo build`
- Run your tests: `cargo test`
    - These get tedious to type, shell alias to your heart's content: `cargob`/`cb` and `cargot`/`ct`
- Magic, right? How does this work?

---
### Cargo.toml ###

- Cargo uses a TOML file named Cargo.toml to figure out dependencies
- Build targets are determined by module declarations in `main.rs`/`lib.rs`
    - More on this later

```toml
[package]
name = "french_press"
version = "0.1.0"
authors = ["David Mally <djmally@gmail.com>"]

[dependencies]
uuid = "0.1"

[profile.release]
opt-level = 3
debug = false
rpath = false
lto = false
debug-assertions = false
codegen-units = 1
```

---
## `cargo test`

- A test is any function which has been annotated with `#[test]`.
- `cargo test` will run all annotated functions in your project.
- Any function which executes without crashing (`panic!`ing) succeeds.
- Use `assert!` to cause a panic in your code.

- You will learn more about this in your first homework.

---
## Installation

- A few choices...
    - `rustup.sh`: a provided installation script which you can `curl | sh`
    - Official stable binaries
    - `multirust`: manages installations of multiple versions of Rust
        - similar to `rvm`
- Choose the method you are most comfortable with.

TODO: I'm not sure which are the most common/recommended installation options?

---
## Homework

TODO: brief note on homework

---
## Next Time ##
![Next time, baby]({{ site.baseurl }}/img/next_time.jpg)

- Ownership & Lifetimes
- Structs
- Enums
- Methods

Many code examples taken from [_The Rust Programming Language_](https://doc.rust-lang.org/stable/book/)