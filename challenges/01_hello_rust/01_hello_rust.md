# Challenge 1: Hello, World (but make it interesting)

## The Theory

Three things to know before you write a line of Rust:

**1. Every Rust program starts at `main`**

```rust
fn main() {
    // your code goes here
}
```

**2. You print with `println!`** (the `!` means it's a macro, not a function — don't worry about that distinction yet)

```rust
println!("Hello, world!");
```

**3. Variables are declared with `let`, and Rust is statically typed**

```rust
let name: &str = "Alice";  // type explicitly declared
let age = 30;              // type inferred by Rust — this is fine too
```

A key quirk: **variables are immutable by default.** If you want to change a variable after declaring it, you need `mut`:

```rust
let x = 5;      // immutable — you can't change x
let mut y = 5;  // mutable — you can change y
```

This will feel odd coming from Python. It's intentional — Rust wants you to be explicit about what changes.

---

## Your Task

Create a new project with:

```bash
cargo new hello_rust
cd hello_rust
```

Then edit `src/main.rs` to write a program that:

1. Stores your name in a variable
2. Stores your age in a variable
3. Stores a *mutable* count variable, starting at `0`
4. Increments that count by `1`
5. Prints a message using all three variables, in this format:

```sh
Hi, I'm Alice. I am 30 years old. This program has run 1 time(s).
```

Run it with `cargo run`.

---

## Acceptance Criteria

- ✅ All three variables are explicitly typed (no type inference — write the types out, it's good practice)
- ✅ The count variable is correctly marked `mut`
- ✅ The output matches the format above
- ✅ It compiles with no warnings
