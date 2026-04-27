# Challenge 3: The Ownership Puzzle (and the Lifetimes Puzzle)

## The Theory

This is Rust's big idea. No garbage collector, no manual `malloc`/`free` — instead, a set of rules the compiler enforces at compile time.

**The core problem Rust is solving:**
Memory has to be *allocated* when you create data, and *freed* when you're done with it. Every other language either makes you do this manually (C/C++, prone to bugs) or runs a garbage collector to clean up (Python, Java — safe but adds overhead). Rust does neither. It uses **ownership rules** instead.

**The three rules:**

1. Every value has exactly one owner
2. When the owner goes out of scope, the value is dropped (memory freed)
3. There can only ever be one owner at a time

---

### 1. Move semantics — when ownership transfers

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is MOVED into s2 — s1 no longer exists
println!("{}", s1);  // ❌ compiler error: s1 was moved
```

### 2. Borrowing — lending without transferring

```rust
let s1 = String::from("hello");
let s2 = &s1;  // s2 borrows s1 — s1 still owns the data
println!("{} {}", s1, s2);  // ✅ both work
```

Think of it like a library book. You (the owner) can lend it to a friend (`&s1`), but you still own it. You can't give it away twice though.

### 3. Mutable borrowing

```rust
let mut s = String::from("hello");
let s2 = &mut s;  // mutable borrow
s2.push_str(", world");
```

The key rule: **you can have many immutable borrows, OR exactly one mutable borrow — never both at once.** This is how Rust prevents data races at compile time.

---

## Your Task

Write a program that demonstrates you understand ownership and borrowing. It must do all of the following:

1. Create a `String` called `original`
2. Create a function `print_length` that takes an **immutable borrow** of a `String` and prints its length — `original` must still be usable after calling it
3. Create a function `make_loud` that takes a **mutable borrow** of a `String` and converts it to uppercase in place (hint: `s.make_ascii_uppercase()`)
4. After calling both functions, print `original` to prove it still lives

---

## Acceptance Criteria

- ✅ `print_length` takes `&String`, not `String` (no ownership transfer)
- ✅ `make_loud` takes `&mut String`
- ✅ `original` is still usable and printed at the end — proving the functions only borrowed it
- ✅ Compiles with no warnings

---

## Bonus Challenge: The Lifetimes Puzzle

### Additional Concepts

Returning a reference from a function raises a question for the compiler: *how long does the thing you're pointing at live?*

Lifetime annotations make relationships explicit:

```rust
fn longer<'a>(s1: &'a str, s2: &'a str) -> &'a str {
```

Read `'a` as: *"some lifetime I'm calling 'a"*. This says: *"given two string references that both live at least as long as `'a`, I'll return a reference that also lives at least as long as `'a`."*

### Your Bonus Task

Write a program with three functions:

1. `longest<'a>(s1: &'a str, s2: &'a str) -> &'a str`  
   Takes two string slices, returns the longer one (if equal length, return either).  
   No cloning - return a direct reference to one of the inputs.
2. `first_word(s: &str) -> &str`  
   Takes a string slice, returns a slice containing only the first word (everything before the first space).  
   If there's no space, return the whole string.

   Hint: `s.find(' ')` returns an `Option<usize>` - we'll cover `Option` properly in Challenge 6, but for now you can use it like:

   ```rust
   match s.find(' ') {
       Some(i) => &s[..i],
       None => s,
   }
   ```

3. `main`
   - Create two `String` variables
   - Call `longest` and print the result
   - Call `first_word` on a sentence and print the result
   - After both calls, prove the original `String` values are still alive by printing them

### Bonus Acceptance Criteria

- ✅ `longest` uses an explicit lifetime annotation and returns a reference to one of its inputs - no cloning
- ✅ `first_word` returns a `&str` slice of its input - no cloning
- ✅ Both original `String` values are printed after the function calls
- ✅ Compiles with no warnings
