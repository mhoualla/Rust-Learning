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
