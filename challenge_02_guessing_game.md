# Challenge 2: The Guessing Game

## The Theory

### 1. Functions

```rust
fn add(x: i32, y: i32) -> i32 {
    x + y  // no semicolon = this is the return value
}
```

Note: the last expression in a function is automatically returned if you omit the semicolon. You can also use `return x + y;` explicitly, but idiomatic Rust prefers the implicit style.

### 2. Control flow

```rust
if x > 10 {
    println!("big");
} else if x > 5 {
    println!("medium");
} else {
    println!("small");
}
```

### 3. Loops

Rust has three loop types. You'll use `loop` today:

```rust
loop {
    // runs forever until you explicitly break
    break;
}
```

### 4. Reading user input

This is a little ceremonious in Rust — bear with it:

```rust
use std::io;

let mut input = String::new();
io::stdin().read_line(&mut input).expect("Failed to read");
let input: u32 = input.trim().parse().expect("Please enter a number");
```

Two things to notice:

- `read_line` takes `&mut input` — it *borrows* your string to write into it. We'll dig into what that means in Challenge 3.
- `.expect("...")` is basic error handling — if something goes wrong, the program crashes with that message. We'll do this properly in Challenge 6.

### 5. Random numbers

Rust's standard library doesn't include random numbers — you need an external *crate* (Rust's word for library). Add this to your `Cargo.toml` under `[dependencies]`:

```toml
rand = "0.8"
```

Then use it like:

```rust
use rand::Rng;

let secret: u32 = rand::thread_rng().gen_range(1..=100);
```

---

## Your Task

Build a number guessing game that:

1. Generates a **random secret number** between 1 and 100
2. Loops, asking the user to guess until they get it right
3. After each wrong guess, prints either `"Too high!"` or `"Too low!"`
4. When correct, prints `"Correct! You got it in X guess(es)."` where X is the number of attempts
5. The guess-checking logic must live in a **separate function** with this exact signature:

```rust
fn check_guess(guess: u32, secret: u32) -> &'static str {
```

Don't worry about `'static` for now — it just means the string lives for the whole program.

---

## Acceptance Criteria

- ✅ Random number is generated fresh each run
- ✅ Correct `"Too high!"` / `"Too low!"` feedback
- ✅ Guess counter works correctly
- ✅ `check_guess` function exists with the signature above
- ✅ Program exits naturally after a correct guess
