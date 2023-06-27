# Setting Up a New Project
To set up a new project, use Cargo:  
```cargo new guessing_game```
1. The first command, ```cargo new``` takes the name of the project (*guessing game*), as the first argument.  
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
1. To obtain user input and then print the result as output, we need to bring the ```io``` input/output library into scope. The ```io``` library comes from the standard library, known as *std*  
2. By default, Rust has a set of items defined in the standard library that it brings into the scope of every program. This set is called the ```prelude```, and you can see everything in it here: https://doc.rust-lang.org/std/prelude/index.html  
3. If a type you want to use isn't in the prelude, you have to bring that type into scope explicitly with a *use* statement.  
4. Using the ```std::io``` library provides you with a number of useful features, including the ability to accept user input.  

# fn main() 
1. The ```fn`` syntax declares a new function.  
2. The parentheses, ```()```, indicate there are no parameters.  
3. And the curly bracket, starts the body of the function.  

# println!
```println!``` is a macro that prints a string to the screen.

# Storing Values with Variables  
Next, we'll create a ```variable``` to store the user input.
1. We use the ```let``` statement to create the variable. 
2. Another example would be, ```let apples = 5```, which creates a new variable named ```apples`` and binds it to the value 5.  
3. In Rust, variables are immutable by default, meaning once we give the variable a value, the value won't change.  
4. To make a variable mutable, we add ```mut``` before the variable name.  

  ```rust
    let apples = 5; // immutable  
    let mut bananas = 5; // mutable
  ```

On the right of the equal sign is that value that ```guess``` is bound to, which is the result of calling ```String::new```:  
1. ```String::new``` is a function that returns a new instance of a ```String```  
2. The ```::``` syntax in the ```::new``` line indicates that ```new``` is an associated function of the ```String``` type.  
3. An associated function is a function that's implement on a type, in this case ```String```  
4. The ```new``` function creates a new, empty string. 

In full, the ```let mut guess = String::new();``` line has created a mutable variable that is currently bound to a new, empty instance of a ```String```.  

# Receiving User Input  
On the first line of the program, we included the input/output functionality from the standard library. Now we'll call the *stdin* function from the *io* module, which will allow us to handle user input:  
``` rust
io:stdin()  
    .read_line(&mut guess)
```

If we hadn't imported the ```io``` library with ```use std::io;``` at the beginning of the program, we could still use the function by writing this fnction call as ```std::io::stdin```. The ```stdin``` function returns an instance of ```std::io::Stdin```, which is a type that represents a handle to the standard input for your terminal.  

Next, the line ```.read_line(&mut guess)``` calls the ```read_line``` method on the standard input handle to get input from the user.  
1. We are also passing ```&mut guess``` as the argument to ```read_line``` to tell it what string to store the user input in.  
2. The full job of ```read_line``` is to take whatever the user types into standard input and append that into a string (without overwriting its contents), so we therefore pass that string as an argument.  
3. The string argument needs to be mutable so the method can change the sting's content.  
4. The ```&``` indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times.  
5. References are a complex feature, and one of Rust's major advantages is how safe and easy it is to use references.  

In all, like variables, references are immutable by default. Hence, you need to write ```&mut guess``` rather than ```&guess``` to make it mutable.  

# Handling Potential Failure with Result  

``` Rust
.expect("Failed to read line");  
```

As mentioned earlier, ```read_line``` puts whatever the user enters into the string we pass to it, but it also returns a ```Result``` value.  
1. ```Result ```is an enumeration, often called an enum, which is a type that can be in one of multiple possible states.  
2. We call each possible state a variant.  
3. The purpose of these ```Result``` types is to encode error-handling information.  
4. Result's variants are ```Ok``` and ```Err```.  
5. The Ok variant indicates the opreation was successful, and inside Ok is the successfully generated value.  
6. The ```Err``` variant means the operation failed, and ```Err``` contains information about how or why the operation failed.  

Values of the Result type, like values of any type, have methods defined on them. An instance of Result has an expect method that you can call.  
1. If this instance of Result is an Err value, expect will cause the program to crash and display the message that you passed as an argument to expect.  
2. If this instance of Result is an Ok value, expect will take the return value that Ok is holding and return just that value to you so you can use it.  
3. In this case, that value is the number of bytes in the user's input.  

If you don't call expect, the program will compile, but you'll get a warning. Rust warns that you haven't used the Result value returned from read_line, indicating that the program hasn't handled a possible error.  

# Printing Values with println! Placeholds  

``` rust
println!("You guessed: {guess}");  
```

This line prints the string that now contains the user's input. The ```{}``` set of curly brackets is a placeholder.  
1. Think of ```{}``` as little crab pincers that hold a value in place.  
2. When printing the value of a variable, the variable name can go inside the curly brackets.  


When printing the result of evaluating an expression, place empty curly brackets in the format stirng, then follow the format string with a commoa-separate list of expressions to print in each empty curly bracket placeholder in the same order. Printing a variable and the result of an expression would look like this:  

``` rust
let x = 5;  
let y = 10;  

println!("x = {x} and y + 2 = {}", y + 2);  
```

The code would print x = 5 and y + 2 = 12.  

# Generating a Secret Number  
Rust doesn't yet include random number functionality in its standard library. However, Rust does provide a rand crate with said functionality  

# Use a Crate to Get More Functionality  
Remember that a crate is a collection of Rust source code files.  
1. The project we've been building is a binary crate, which is an executable.  
2. The rand crate is a library crate, which contains code that is intended to be used in other programs and can't be executed on its own.  

We need to modify the Cargo.toml file to include the rand crate as a dependency. In [dependencie] you tell Cargo which external crates your project depends on and which versions of those crates you require. Cargo will also grab other crates that rand depends on to work, after building.  

# Ensuring Reproducible Builds with the Cargo.lock File  
Cargo has a mechanism that ensures you can rebuild the same artifact every time you or anyone else builds your code.  
1. Cargo will use only the versions of the dependencies you specified until you indicate otherwise.  
2. For example, say the next week version of the rand crate comes out, and that version contains an important bug fix, but it also contains a regression that will break your code.  
3. To handle this, Rust creates the Cargo.lock file the first time you run cargo build. When you build your project in the future, Cargo will see that the Cargo.lock file exists and will use the versions specified there rather than doing all the work of figuring out versions again.  
4. This lets you have a reproducible build automatically. Your project will remain at a version until you explicitly upgradte.  

# Updating a Crate to Get a New Version  
When you do want to update a crate, Cargo provides the command update, which will ignore the Cargo.lock file and figure out all the latest versions that fit your specifications in Cargo.toml.  

Cargo will then write those versions to the Cargo.lock file.  


# Generating a Random Number  

``` Rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```  

First we add the line ```use rand::Rng;```  
1. The Rng trait defines methods that random number generators implement, and this trait must be in scope for us to use those methods.  


Next, we're adding two lines in the middle. In the first line, we call the ```rand::thread_rng```
1. function that gives us the particular random number generator we're going to use: one that is local to the current thread of execution and is seeded by the OS.  
2. Then we call the ```gen_range``` method on the random number generator. This method is defined by the Rng trait that we brought into scope with the ```use rand::Rng;``` statement.  
3. The gen_range method takes a range expression as an argument and generates a random number in the range.  
4. The kind of range expression we're using here takes the form ````start..=end``` and is inclusive on the lower and upper bounds, so we need to specifu ```1..=100``` to request a number between 1 and 100.  

# Comparing the Guess to the Secret Number  

``` Rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

1. First we add another use statement, bringing a type called std::cmp::Ordering into scope from the standard library.  
2. The Ordering type is another enum and has the variants Less, Greater, and Equal. These are the three outcomes that are possible when you compare two values.  
3. Then we add five new lines at the bottom that use the Ordering type. The cmp method compares two values and can be called on anything that can be compared. It takes a reference to whatever you want to compare with: here it's comparing guess to secret_number.  
4. Then it returns a variant of the Ordering enum we brought into scope using the use statement. We use a match expression to decide what to do next based on which variant of Ordering was returned from the call to cmp with the values in guess and secret_number.  

A match expression is made up of arms.  
1. An arm consists of a pattern to match against, and the code that should be run if the value given to match fits that arm's pattern.  
2. Rust takes the value given to match and looks through each arm's pattern in turn.  

However, compiling this will generated an error that states there are mismatched types.  
1. Rust has a strong static type system. However, it also has type inference.  
2. When we wrote ``` let mut guess = String::new() ```, Rust was able to infer that guess should be a String, and didn't make us write the type.  
3. The ```secret_number``` on the other hand  is a number type. Rust cannot compare a string and a number type.  
4. Ultimately, we want to convert the String the program reads as input into a real number type so we can compare it numerically to the secret number.

``` rust
    // --snip--

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse().expect("Please type a number!");

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
  ```
1.  We create a variable named ```guess```. But wait, doesn't the program already have a variable named guess?  
2. It does, but helpfully Rust allows us to shadow the previous value of ```guess``` with a new one. Shadowing lets us reuse the ```guess``` variable name rather than forcing us to create two unique variables, such as ```guess_str``` and ```guess```.  
3. For example, this feature is often used when you want to convert a value from one type to another type.  

We bind this new variable to the expression ```guess.trim().parse```.  
1. The ```guess``` in the expression refers to the original guess variable that contained the input as a string.  
2. The ```trim``` method on a String instance will eliminate any whitespace at the beginning and end, which we must do to be able to compare the string to the ```u32```, which can only contain numerical data.  
3. The user must press enter to satisfy ```read_line``` and input their guess, which adds a newline character to the string. The trim method eliminates the newline  


The parse method on strings converts a string to another type. Here we use it to convert from a string to a number.  
1. We need to tell Rust the exact number type we want by using ```let guess : u32 ```. The colon after guess tells Rust we'll annotate the variable's type.   
2. Additionally, the ```u32``` annotation in this example program and the comparison with `secret_number` means Rust will infer that `secret_number` should be a `u32` as well.  
3. The `parse` method wil only work on characters that can logically be converted into numbers and so can easily cause errors.  
4. Because it might fail, the parse method returns a `Result` type, much as the `read_line` method does. 
5. We'll treat this `Result` the same way by using the `expect` method again if `parse` returns an `Err Result` variant.  

# Allowing Multiple Guesses with Looping  

The loop keyword creates an infinite loop. 
``` Rust  
    // --snip--

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
```  

The program now asks for another guess forever. We can have the player type 'quit` to leave the game.  

# Quitting After a Correct Guess  
We can have the program quit when the user wins by adding a break statement
``` Rust
  match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
  }
```
Adding the `break` line after `You win!` makes the program exit the loop when the user guesses the secret number correctly.  


# Handling Invalid Input  
Let us make the game ignore a non-number so the user can continue guessing. We can do that by altering the line where `guess` is converted from a `String` to a `u32`  
``` Rust
        // --snip--

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        // --snip--
```
We switch from an `expect` call to a `match` expression to move from crashing on an error to handling the error.  
1. Remember that `parse` returns a `Result` type and `Result` is an enum that has the variants `Ok` and `Err`  
2. We're using a `match` expression hhere, as we did with the `Ordering` result of the `cmp` method.  
3. If `parse` is able to sucessfully turn the string into a number, it will return an `Ok` value that contains the resultant number. That `Ok` value will match the first arm's pattern, and the `match` expression will just return the `num` that `parse` produced and put it inside the `Ok` value. That number will end up right where we want it in the new guess variable that we're creating.  
4. If `parse` is not able to turn the string into a number, it will return an `Err` value that does match the `Err(_)` pattern in the second arm. The underscore, `_`, is a catchall value; in this example, we are saying that we want to match all `Err` values. 
5. so the program will execute the second arm's code,  `continue`, which tells the program to go to the next iteration of the `loop` and ask for another guess. The program effectively ignores all errors that parse might encounter. 





