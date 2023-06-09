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
A `tuple` is a general way way of grouping together a number of values with a variety of types into one `compound` type.

We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same. We’ve added optional type annotations in this example:

``` Rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

The variable `tup` binds the entire tuple because a tuple is considered a single compound element. To get individual types out of a tuple, we can use pattern matching to destructure a tuple value..  

``` Rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

This is called `destructuring` because it's breaking a single tuple into three parts.  

We can also access a single tuple element directly using `.` followed by the index of the value we want to access. 

``` Rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

The tuple without any values has a special name, `unit`. This value and its corresponding type are both written `()` and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value. 


# The Array Type  

Another way to have a collection of multiple values is with an Array. Unlike tuple, every element of an array must have the same type. Arrays in Rust have a fixed length. 

``` Rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

Arrays are useful when you want your data allocated on the stack rather than the heap, or when you want to ensure that you have a fixed number of elements (that do not need to change). 

``` Rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You write an array's type using square brackets with the type of each element, a semicolon, and then the number of elements in the array..

``` Rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number `5` indicates the array contains five elements. 

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets, as shown here:

``` Rust
let a = [3; 5];
```

The array contains 5 elements, all of which are set to the value 3. 
# Accessing Array Elements  
An array is a single chunk of memory of a known, fixed size that can be allocated on the stack. You can access elements of an array using indexing, like this:

``` Rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

# Invalid Array Element Access  
A program such as this

``` Rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!("The value of the element at index {index} is: {element}");
}
```

Will result in a runtime error at the point of using an invalid value in the indexing operation. The program exited with an error message and didn't execute the final `println!` statement.  

When you attempt to access an element using indexing, Rust will check that the index you've specified is less than the array length.  
1. If the index is greater than or equal to the length, Rust will panic.  
2. This check has to happen at runtime, especially in this case, because the compiler can't possible know what a value a user will enter when they run the code later.  

# Functions  
The `fn` keyword allows you to declare new functions.  
1. Rust code uses snake case as the conventional style for functions and variable names.   

``` Rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
``` 

We define a function in Rust by entering `fn` followed by a function name and a set of parentheses. The curly brackets tell the compiler where the function body begins and ends.   

# Parameters  

We can define functions to have parameters, which are special variables that are part of a function's signature.  

``` Rust 
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```  
The declaration of `another_function` has one parameter named `x`.  
1. The type of `x` is specified as `i32`  

In function signatures, you must declare the type of each parameter.  
1. This is a deliberate decision in Rust's design: requiting type annotation in function definitions means the compiler almost never needs you to use them elsewhere in the code to figure out what type you mean.  

``` Rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
``` 

This example creates a function named `print_labeled_measurement` with two parameters. The first parameter is named `value` and is an `i32`. The second is named `unit_label` and is type `char`.  


# Statements and Expressions  
Function bodies aer made up of a series of statements optionally ending in a expression. 
1. So far, the functions we've covered have not included an ending expression, but you have seen an expression as part of a statement.  
2. Because Rust is an expression-based language, this is an important distinction to understand.  
3. Other languages don't have the same distinctions..  

`Statements` are instructions that perform some action and do not return a value.  
`Expressions` evaluate to a resultant value.  

Statement:  
```Rust
fn main() {
    let y = 6;
}
```

Function definitions are also statements. Statements do not return values. Therefore, you can't assign a `let` statement to another variable, as the following code tries to do; you'll get an error:  
```Rust
fn main() {
    let x = (let y = 6);
}
```  

The `let y = 6` statement does not return a value, so there isn't anything for `x` to bind to. 
1. This is different from what happens in other languages, such as C and Ruby, where the assignment returns the value of the assignment.  
2. In those languages, you can write `x = y = 6` and have both `x` and `y` have the value 6; that is not the case in Rust.  

Expressions evaluate to a value and make up most of the rest of the code that you'll write in Rust. 
1. Consider a math operation, such as `5 + 6`, which is an expression that evaluates to the value `11`.  
2. Expressions can be part of statements. 
3. Calling a function is an expression.  
4. Calling a macro is an expression.  
5. A new scope block created with curly brackets is an expression.  

```Rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

This expression:
```Rust
{
    let x = 3;
    x + 1
}
```  
Is a block that, in this case, evaluates to `4`. That value gets bound to `y` as part of the `let` statement.  

Note that the `x+1` line doesn't have a semicolon at the end.  
1. Expressions do not including ending semicolons.  
2. if you add a semicolon to the end of an expression, you turn it into a statement.  
3. it will then not return a value.  


# Functions with Return Values  
Functions can return values to the code that calls them. We don't name return values, but we must declare their type after an arrow `->`. In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function. You can return early from a function by using a `return` keyword and specifying a value, but most functions return the last expression implicitly. Here's an example of a function that returns a value:  

```Rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}

```  

There are no function calls, macros, or even `let` statements in the `five` function --- just the number `5` itself.  
1. That's perfectly valid in Rust  
2. Note that the function's return type is specified too, as `-> i32`  
3. The line `let x = five();` shows that we're using the return value of a function to initialize a variable.  
4. Because the function five returns a 5, that line is the same as the following:
```Rust
let x = 5
```  
5. Second, the five function has no parmaeters and defines the type of the return value, but the body of the function is a lonely 5 with no semicolon because it's an expression whose value we want to return.  

Let's look at another example:  
```Rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```  
Running this code will print `The value of x is: 6`. But if we place a semicolon at the end of the line containing `x + 1`, changing it from an expression to a statement, we'll get an error.  
1. The main error message, `mismatched types`, reveals the core issue with this code.  
2. The definition of the function `plus_one` says that it will return an `i32`, but statements don't evaluate to a value, which is expressed by `()`, the unit type. Therefore, nothing is returned, which contradicts the function definition and results in an error.  

# Comments  
`///`  
Nice.  

# Control Flow  
The ability to run some code depending on whether a condition is `true` and to run some code repeatedly while a condition is `true` are basic building blocks in most programming languages.  

# If Statements  
An if expression allows you to branch your code depending on conditions. You provide a condition and then state, "If this code is met, run this block of code. If the condition is not met, do not run this block of code"  

```Rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```  

This is intuitive. Notice the lack of parentheses. 

# Using if in a let statement  
Because `if` is an expression, we can use it on the right side of a `let` statement to assign the outcome to a variable.  

```Rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```  

Reminder that Rust `needs` to know at compile time what type the `number` variable is, definitively. Rust would not be able to do that if the type of number was only determined at runtime; the compiler would be more complex and would make fewer guarantees about the code if it had to keep track of multiple hypothetical types for any variable.  

# Repetition with Loops  
Rust has three kinds of loops: loop, while, and for.  

# Repeating Code with loop  
The `loop` keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.  

```Rust
fn main() {
    loop {
        println!("again!");
    }
}
```  
You can use the `break` keyword within the loop to tell the program when to stop executing the loop. In a different vein, `continue` will skip over any remaining code in this iteration of the loop and go to the next iteration.  

# Returning Values from Loops  
One of the uses of a loop is to retry an operation you know might fail, such as checking whether a thread has completing its job.  
1. You might also need to pass the result of that operation out of the loop to the rest of your code. 
2. To do this, you can add the value you want returned after the break expression you use to stop the loop; that value will be returned out of the loop so you can use it:  

```Rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```  

# Loop Labels to Disambiguate Between Multiple Loops  
If you have loops within loops, break and continue apply to the innermost loop at that point.  
1. You can optionally specify a loop label on a loop that you can then use with break or continue to specify that those keywords apply to the labeled loop instead of the innermost loop. 
2. Loop labels must begin with a single quote.  

```Rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```  

# Conditional Loops with while  
```Rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```  

# Looping Through a Collection with for  
```Rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}

```



