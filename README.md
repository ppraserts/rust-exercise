# Rust exercise
Rust exercise is learning chapter for me about "Rust Programming" step by step.

<br/>

## Prerequisite
- Install Rust in your machine and depend on your os by bellow link:
```
https://www.rust-lang.org/learn/get-started
```
- Install Visual Studio Code bellow link:
```
https://code.visualstudio.com/download
```
- Recommended extensions:

Rust Lang
```
https://marketplace.visualstudio.com/items?itemName=rust-lang.rust
```

Rust Analyzer
```
https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
```

Rust Syntax
```
https://marketplace.visualstudio.com/items?itemName=dustypomerleau.rust-syntax
```

Error Lens
```
https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens
```
<br/>

## General Command
-  Compile rust to executeable binary file:
```sh
rustc main.rs
```

- Create new Rust project:
```sh
cargo new <your_project_name>
```

- Compile with binary file:
```sh
cargo build
```

- Compile without binary file:
```
cargo check
```

- Compile with binary file and run:
```sh
cargo run
```

- Auto format Rust file
```sh
rustfmt <your_rust_file>
```

<br/>

## Common of Rust
```rust
fn main() {
    // Single-line code comments
    /*
       Multiple-line code comments
    */

    // Print value
    println!("Hello Rust!!!");

    // Print variable value
    let year = 2022;
    println!("Hello Rust {} !!!", year); // Output: Hello Rust 2022 !!!

    // Print memory address in Ram
    println!("'year' memory address is '{:p}'", &year); // Output: 'year' memory address is '0x6888cff79c'
```

<br/>

## Variables, Constants and Shadowing
- Variables
```rust
    // Naming convention:
    // 'Structs' get camel case
    // 'Variables' get snake case
    // 'Constants' get all upper case

    // Variables
    let n1: u32 = 5; 
    let n2 = 10; 
    let n3 = 15;
    n3 = "test" // ^^ expected integer, found `&str` : Because 'n3' Statically typed check on compile time and Statically typed because type not dynamic
```

- Mutable vs imutable
```rust
    let n4 = 20;
    n4 = 20; // ^^ cannot assign twice to immutable variable

    let mut n5 = 20;
    n5 = 20;
```

- Constants
```rust
    const SECONS_IN_MINUTE: i32 = 60;
    const SECONS_IN_MINUTE = 60; // ^^ provide a type for the constant: `SECONS_IN_MINUTE: i32`
    SECONS_IN_MINUTE = 59; // ^^ cannot assign to this expression
    const SECONS_IN_MINUTE: i32 = 60; // ^^ `SECONS_IN_MINUTE` redefined here
```

- Shadowing
```rust
    let n6 = 5; // value n6 is 5
    {
        let n6 = 10; // value n6 is= 10
    }
    let n6 = n6 + 5;// value n6 is 5 + 5 = 10
```

<br/>

## Memory Mangement
### Stack
- Order
- Fixed-Size
- LIFO
```rust
    n1 -> n2 -> n3  vvv            ^^^ n1 -> n2 -> n3
                    number3 0x1..8 15
                    number2 0x1..0 10
                    number1 0x1..c 5
```
- Fast

### Heap
- Unorder
```rust
    let num1 = Box::new(1);
    let num2 = Box::new(2);
```
in memory
```rust
                      Heap

        0x1ee42ff648 | 1
                    0x1ee42ff640 | 2
    -----------------------------------
    num2: 0x1ee42ff650  |  0x1ee42ff640
    num1: 0x1ee42ff63c  |  0x1ee42ff648
                      
                      Stack
```
- Variable-Size
- Slow

### Ownership, reference and borrowing
```rust
    let num1 = 1;
    let num2 = &num1;
    let num3 = num2;
    let num4 = &num2;

    println!("num1: {:p}  {}  -", &num1, num1);
    println!("num2: {:p}  {}  {:p}", &num2, num2, num2);
    println!("num3: {:p}  {}  {:p}", &num3, num3, num3);
    println!("num4: {:p}  {}  {:p}", &num4, num4, num4);
    /*
        num1: 0x1ee42ff63c    1  -
        num2: *0x1ee42ff640*  1  0x1ee42ff63c
        num3: 0x1ee42ff648    1  0x1ee42ff63c
        num4: 0x1ee42ff650    1  *0x1ee42ff640*
    */
```

### Mutable
```rust
    let mut num1 = 1;
    println!("num1: {:p}  {}  -", &num1, num1);

    let num2 = &mut num1;
    *num2 = 20;
    println!("num2: {:p}  {:p}  {}", &num2, num2, num2);
    println!("num1: {:p}  {}  -", &num1, num1);
    /*
        num1: 0xb9942ff5a4  1  -
        num2: 0xb9942ff600  0xb9942ff5a4  20
        num1: 0xb9942ff5a4  20  -
    */
```

### Safe Issue
- Dangling Pointer
```rust
        Stack           |           Heap
        --------------------------------
        0xb9942ff5a4    |
        n1[ptr]--------------------->
                        |
```
- Memory Leak
```rust
        Stack           |           Heap
        --------------------------------
                        |      0xb9942ff5a4
                        |           [5]
```