## Challenge 5: Categorise a Thing

### The Theory

**Enums** let you define a type that can be one of several distinct *variants*. You've already felt the need for them — in Challenge 2, you used strings like `"Too high!"` as stand-ins for states. Enums are the right tool for that job.

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let d = Direction::North;
```

**Enums can carry data**

This is where Rust enums go beyond what most languages offer. Each variant can hold different data:

```rust
enum Shape {
    Circle(f64),           // one value: radius
    Rectangle(f64, f64),   // two values: width, height
    Triangle(f64, f64, f64), // three values: sides
}
```

**Pattern matching with `match`**

`match` is how you work with enums. It's like a `switch` statement, but exhaustive — the compiler forces you to handle every variant:

```rust
let shape = Shape::Circle(5.0);

match shape {
    Shape::Circle(r)         => println!("Circle with radius {r}"),
    Shape::Rectangle(w, h)   => println!("Rectangle {w}x{h}"),
    Shape::Triangle(a, b, c) => println!("Triangle with sides {a}, {b}, {c}"),
}
```

If you forget a variant, the compiler refuses to compile. No silent bugs.

**`impl` on enums**

Just like structs, enums can have methods:

```rust
impl Shape {
    fn area(&self) -> f64 {
        match self {
            Shape::Circle(r)         => std::f64::consts::PI * r * r,
            Shape::Rectangle(w, h)   => w * h,
            Shape::Triangle(a, b, c) => { ... }
        }
    }
}
```

---

### Your Task

Define an enum `Shape` with three variants: `Circle(f64)`, `Rectangle(f64, f64)`, and `Triangle(f64, f64, f64)` — where the values represent radius, width/height, and the three sides respectively.

Implement the following methods on `Shape`:

1. `area(&self) -> f64`
   - Circle: `π * r²`
   - Rectangle: `w * h`
   - Triangle: use Heron's formula — given sides `a`, `b`, `c`:

    ```rust
    s = (a + b + c) / 2
    area = sqrt(s * (s-a) * (s-b) * (s-c))
    ```

2. `perimeter(&self) -> f64`
- Circle: `2 * π * r`
- Rectangle: `2 * (w + h)`
- Triangle: `a + b + c`
 
 3. `describe(&self) -> String`
    Returns a human-readable string like `"Circle with radius 5"` or `"Rectangle 4x3"` or `"Triangle with sides 3, 4, 5"`
 
 In `main`:
 - Create one of each variant
 - For each shape, print its description, area, and perimeter
 
 **Bonus:** Go back and rewrite `check_guess` from Challenge 2 to return a `GuessResult` enum instead of `&'static str`. You don't need to paste the whole program — just the enum definition and the updated function signature.
 
 ---
 
 ### Acceptance Criteria
 - ✅ `Shape` enum has exactly three variants, each carrying data
 - ✅ All three methods implemented using `match`
 - ✅ Every `match` is exhaustive — no wildcard `_` catch-alls used as a shortcut
 - ✅ `describe` returns a `String`, not a `&str`
 - ✅ Compiles with no warnings