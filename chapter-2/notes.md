# Setting Up a New Project
To set up a new project, use Cargo:  
*cargo new guessing_game*
1. The first command, *cargo new* takes the name of the project (*guessing game*), as the first argument.  
# Processing a Guess  
The first part of the guessing game program will ask for user input, process that input, and check that the input is in the expected form.  

To start, we'll allow the player to input a guess: 

```rust
    use std::io;  
    fn main() {  
    println!("Guess the number!");  

    println!("Please input your guess.");  

    let mut guess = String::new();  

    io::stdin()  
        .read_line(&mut guess)  
        .expect("Failed to read line");  

    println!("You guessed: {guess}");  
}  
```

# use std::io;
1. To obtain user input and then print the result as output, we need to bring the *io* input/output library into scope. The *io* library comes from the standard library, known as *std*  
2. By default, Rust has a set of items defined in the standard library that it brings into the scope of every program. This set is called the *prelude*, and you can see everything in it here: https://doc.rust-lang.org/std/prelude/index.html  
3. If a type you want to use isn't in the prelude, you have to bring that type into scope explicitly with a *use* statement.  
4. Using the *std::io* library provides you with a number of useful features, including the ability to accept user input.  

# fn main() {
1. The *fn* syntax declares a new function.  
2. The parentheses, (), indicate there are no parameters.  
3. And the curly bracket, starts the body of the function.  

# println!
*println!* is a macro that prints a string to the screen.

# Storing Values with Variables  
Next, we'll create a *variable* to store the user input.
1. We use the *let* statement to create the variable. 
2. Another example would be, *let apples = 5*, which creates a new variable named *apples* and binds it to the value 5.  
3. In Rust, variables are immutable by default, meaning once we give the variable a value, the value won't change.  
4. To make a variable mutable, we add *mut* before the variable name.  

  ```rust
    let apples = 5; // immutable  
    let mut bananas = 5; // mutable
  ```

On the right of the equal sign is that value that *guess* is bound to, which is the result of calling *String::new*:  
1. *String::new* is a function that returns a new instance of a *String*  
2. The *::* syntax in the *::new* line indicates that *new* is an associated function of the *String* type.  
3. An associated function is a function that's implement on a type, in this case *String*  
4. The *new* function creates a new, empty string. 

In full, the **let mut guess = String::new();** line has created a mutable variable that is currently bound to a new, empty instance of a *String*.  

# Receiving User Input  
On the first line of the program, we included the input/output functionality from the standard library. Now we'll call the *stdin* function from the *io* module, which will allow us to handle user input:  
``` rust
io:stdin()  
    .read_line(&mut guess)
```

If we hadn't imported the *io* library with *use std::io;* at the beginning of the program, we could still use the function by writing this fnction call as *std::io::stdin*. The *stdin* function returns an instance of *std::io::Stdin*, which is a type that represents a handle to the standard input for your terminal.  

Next, the line *.read_line(&mut guess)* calls the *read_line* method on the standard input handle to get input from the user.  
1. We are also passing *&mut guess* as the argument to *read_line* to tell it what string to store the user input in.  
2. The full job of *read_line* is to take whatever the user types into standard input and append that into a string (without overwriting its contents), so we therefore pass that string as an argument.  
3. The string argument needs to be mutable so the method can change the sting's content.  
4. The *&* indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times.  
5. References are a complex feature, and one of Rust's major advantages is how safe and easy it is to use references.  

In all, like variables, references are immutable by default. Hence, you need to write *&mut guess* rather than *&guess* to make it mutable.  

# Handling Potential Failure with Result  

``` Rust
.expect("Failed to read line");  
```

As mentioned earlier, *read_line* puts whatever the user enters into the string we pass to it, but it also returns a *Result* value.  
1. *Result* is an enumeration, often called an enum, which is a type that can be in one of multiple possible states.  
2. We call each possible state a variant.  
3. The purpose of these Result types is to encode error-handling information.  
4. Result's variants are *Ok* and *Err*.  
5. The Ok variant indicates the opreation was successful, and inside Ok is the successfully generated value.  
6. The Err variant means the operation failed, and Err contains information about how or why the operation failed.  

Values of the Result type, like values of any type, have methods defined on them. An instance of Result has an expect method that you can call.  
1. If this instance of Result is an Err value, expect will cause the program to crash and display the message that you passed as an argument to expect.  
2. If this instance of Result is an Ok value, expect will take the return value that Ok is holding and return just that value to you so you can use it.  
3. In this case, that value is the number of bytes in the user's input.  

If you don't call expect, the program will compile, but you'll get a warning. Rust warns that you haven't used the Result value returned from read_line, indicating that the program hasn't handled a possible error.  

# Printing Values with println! Placeholds  

``` rust
println!("You guessed: {guess}");  
```

This line prints the string that now contains the user's input. The {} set of curly brackets is a placeholder.  
1. Think of {} as little crab pincers that hold a value in place.  
2. When printing the value of a variable, the variable name can go inside the curly brackets.  


When printing the result of evaluating an expression, place empty curly brackets in the format stirng, then follow the format string with a commoa-separate list of expressions to print in each empty curly bracket placeholder in the same order. Printing a variable and the result of an expression would look like this:  

``` rust
let x = 5;  
let y = 10;  

println!("x = {x} and y + 2 = {}", y + 2);  
```

The code would print x = 5 and y + 2 = 12.  

# Generating a Secret Number  




