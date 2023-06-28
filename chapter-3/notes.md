# Common Programming Concepts 
# Variables and Mutability  
By default, variables are immutable  
1. You still have the option to make your variables mutable
2. When a variable is immutable, once a value is bound to a name, you cannot change that value.  

``` Rust
fn main() {
    let x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

The following code produces an error regarding immutability. 
1. It's important that we get compile-time errors when we attempt to change a value that's designated as immutable because this very situation can lead to bugs.  
2. if one part of our code operates on the assumption that a value will never change and another part of our code changes that value, it's possible that the first part of the code won't do what it was designed to do.  
3. The cause of this kind of bug can be dfficult to track down after the fact, especially when the second piece of code changes the value sometimes.  
4. The rust compiler guarantees that when you state a value won't change, it really won't change. 

You can make variables mutable by adding the `mut` keyword in front of the variable name.  

``` Rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

We're allowed to change the value bound to `x` from 5 to 6 when `mut` is used. 

# Constants  
Like immutable variables, `constants` are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.  
1. First, you're not allowed to use `mut` with constants. Constants aren't just immutable by default - they are always immutable. You declare constants using the `const` keyword instead of the `let` keyword, and the type of the value must be annotated.  
2. Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.  
3. The last difference is that constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.  

``` Rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Rust's naming convention for constants is to use all uppercase with underscores betweeen words. 
1. Constants are valid for the entire time a program runs, within the scope in which they were declared. 

# Shadowing
You can declare a new variable with the same name as a previous variable.  
1. The first variable is shadowed by the second.
2. This means that the second variable is what the compiler will see when you use the name of the variable.  
3. In effect, the second variable overshadows the first, taking any uses of the variable name to itslf until either it itself is shadowed or the scope ends.  
4. we can shadow a variable by using the same variable's name and repeating the use of the `let` keyword as follows:  

``` Rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
1. The program first binds x to a value of 5.  
2. then it creates a new variable x by repeating `let x = `, so the value of x is then 6.  
3. Then, within an inner scope created with the curly brackets, the third `let` statement also shadows `x` and creates a new variable, to give x a value of 12.  
4. When that scope is over, the inner shadowing ends and x returns to being 6.

Output:
``` Rust
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/variables`
The value of x in the inner scope is: 12
The value of x is: 6
```
Shadowing is different from marking a variable as `mut` because we'll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword.  
1.  By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed.
2. The other difference between mut and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name.

# Data Types  
Every value in Rust is of a certain `data type` , which tells rust what kind of data is being specified so it knows how to work with that data. We'll take a look at two data types: `scalar` and `compound`. Keep in mind that Rust is a statically typed language, which means that it must know the types of all variables at compile time.  

The compiler can usually infer what type we want to use based on the value and how we use it. in cases when many types are possible, such as when we converted a `String` to a numeric type using `parse`, we must add an annotation like this:  

``` Rust
let guess: u32 = "42".parse().expect("Not a number!");
```

If we don't add the `: u 32` type annotation shown in the preceding code, Rust will display a compile error, which means the compiler needs more information form us to know which type we want to use.  

# Scalar Types  
A `scalar` type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters.  

# Integer Types  
An integer is a number without a fractional component.  

| Length | Signed | Unsigned |
| ------ | -------| --------|
| 8-bit  | i8 | u8
| 16-bit | i16 | u16
| 32-bit | i32| u32
| 64-bit | i64 | u64
| 128-bit| i128 | u128
| arch | isize| usize

Each variant can be signed or unsigned and has an explicit size.   
1. Signed and unsigned refer to whether it's possible for the number to be negative
2. Whether the number needs to have a sign with it or whether it will only ever be positive and can therefore be represented without a sign.  
3. Each signed variant can store numbers from -(2^n-1) to 2^n-1 -1 inclusive, where n is the number of bits that variant uses.  
4. So an i8 can store numbers from -(27) to 27 - 1, which equals -128 to 127.  
5. Unsigned variants can store numbers from 0 to 2n - 1, so a u8 can store numbers from 0 to 28 - 1, which equals 0 to 255.  
6. Additionally, the isize and usize types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.  


You can write integer literals in any of the forms shown below. 
| Number literals | Example |
| ----------------|--------|
| Decimal| 98_222|
|Hex|0xff|
|Octal|0o77|
|Binary|0b1111_0000|
|Byte (u8 only)| b'A'

To explicitly handle the possibility of overflow, you can use these families of methods provided by the standard library for primitive numeric types:

1. Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`.
2. Return the None value if there is overflow with the `checked_*` methods.
3. Return the value and a boolean indicating whether there was overflow with the `overflowing_*` methods.
4. Saturate at the value’s minimum or maximum values with the `saturating_*` methods.  

# Floating-Point Types  
Rust also has two primitive types for floating-point numbers, which are numbers with decimal points.  
1. Rust's floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respecitvely.  
2. The default type is `f64` because on modern CPUs, it's roughly the same speed as `f32` but is capable of more precision.  
3. All floating-point types are signed.

``` Rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

# Numeric Operations  
Rust supports the basic mathematical operations you'd expect for all the number types: addition, subtraction, multiplication, division, and remainder. Integer division truncates toward zero to the nearest integer:

``` Rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Results in -1

    // remainder
    let remainder = 43 % 5;
}
```

# The Boolean Type
``` Rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Results in -1

    // remainder
    let remainder = 43 % 5;
}
```

# The Character Type
Rust's `char` type
``` Rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```
We specify `char` literals with single quotes, as opposed to string literals, which use double quotes.  Rust's `char` type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. 
1. Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid `char` values in Rust.  

# Compound Types  
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.  

# The Tuple Type 
