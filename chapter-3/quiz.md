# Question 1
Which statement best describes what it means if a variable `x` is immutable? 
1. After being defined, x can be changed at most once. 
2. You cannot create a reference to x.  
3. x is stored in the immutable region of memory.
4. x cannot be changed after being asssgined a value.

**Answer: x cannot be changed after being asssgined a value.**


# Question 2
What is the keyword used after `let` to indicate that a variable can be mutated?  
**Answer: `mut`**

# Question 3  
Determine whether the program will pass the compiler. If it passes, write the expected output of the program it were executed:  
```Rust
fn main() {
  let x = 1;
  println!("{x}");
  x += 1;
  println!("{x}");
}
```
**Answer: The program does not compile**

# Question 4
Which of the following statements is correct about the difference between using `let` and `const` to declare a variable?  
1. They are just different syntaxes for declaring variables with the same semantics
2. The compiler will error if a const variable's name is not using UPPER_SNAKE_CASE
3. const can be used in the global scope, and let can only be used in a function
4. A const can only be assigned to a literal, not an expression involving computation 

**Answer: const can be used in the global scope, and let can only be used in a function**

# Question 5 
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.
``` Rust
const TWO: u32 = 1 + 1;
fn main() {
  println!("{TWO}");
}
```
**Answer: DOES compile; Output is 2**

# Question 6
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.

``` Rust
fn main() {
  let mut x: u32 = 1;
  {
    let mut x = x;
    x += 2;
  }
  println!("{x}");
}
```
**Answer: DOES compile; Output is 1**  
Context: The statement x += 2 only affects the shadowed x inside the inner curly braces, not the outer x on line 2.

# Question 7
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.

``` Rust
fn main() {
  let mut x: u32 = 1;
  x = "Hello world";
  println!("{x}");
}
```
**Answer: DOES NOT compile**  
Context: A variable cannot be assigned to a value of a different type than its original type.

# Question 8
The largest number representable by the type i128 is:   
**Answer: 2^127 -1**

# Question 9
If `x : u8 = 0`, what will happen when computing `x-1`  
**Answer: It depends on the compiler mode**  
Context: This expression will panic in debug mode and return 255 in release mode.  

# Question 10
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.
``` Rust
fn main() {
  let x: fsize = 2.0;
  println!("{x}");
}
``` 
**Answer: DOES NOT compile**  
Context: The type fsize does not exist. Floats must be either f32 or f64.  

# Question 11
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed:  
``` Rust
fn main() {
  let message = "The temperature today is:";
  let x = [message, 100];
  println!("{} {}", x[0], x[1]);
}
```

**Answer: This program does NOT compile**  
Context: An array can only contain elements of a single type.

# Question 12
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.  
``` Rust
fn main() {
  let t = ([1; 2], [3; 4]);
  let (a, _) = t;
  println!("{}", a[0] + t.1[0]); 
}
```
**Answer: This program DOES compile. The output will be 4**  
Context: The syntax [x; y] declares an array with y copies of the value x. The syntax (a, _) destructures t and binds a to [1; 2]. The syntax t.1 refers to the second element of t, which is [3; 4].  


# Question 13
The keyword for declaring a new function in Rust is:  
**Answer: fn**  

# Question 14  
Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.  
```Rust

fn f(x) { 
  println!("{x}");
}
fn main() {
  f(0);
}
```  

**Answer: This program does not compile**  
Context: A function must declare the types of its parameters.

# Question 15  
In Rust, a curly-brace block like { /* ... */ } is:  
**Answer: An expression and a syntatic scope**  

# Question 16  
Determine whether the program will pass the compiler. If it passes, write the expected otuput of the program if it were executed.  

```Rust

fn f(x: i32) -> i32 { x + 1 }
fn main() {
  println!("{}", f({let y = 1;
    y + 1
  }));
}
```  
**Answer: It does compile. The output is 3**  

# Question 17  
True/false: executing these two pieces of code results in the same value for `x`.  

Snippet 1:  
```Rust
let x = if cond { 1 } else { 2 };
```

Snippet 2:  
```Rust

let x;
if cond {
  x = 1;
} else {
  x = 2;
}
```   
**Answer: True**  
Context: The first if-expression is a more concise way of representing the behavior of the second if-statement.

Note that Rust does not require x to be initially declared with let mut in the second snippet. This is because Rust can determine that x is only ever assigned once, since only one branch of the if-statement will ever execute.  

# Question 18  

Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.  
```Rust
fn main() {
  let x = 1;
  let y = if x { 0 } else { 1 }; 
  println!("{y}");
}
```  
**Answer: Does not Compile**  
Context: The condition to an if-expression must be a boolean. Rust does not have a concept of "truthy" or "falsy" values.  

# Question 19  
True/false: this code will terminate  
```Rust
fn main() {
    let mut x = 0;
    'a: loop {
        x += 1;
        'b: loop {
            if x > 10 {
                continue 'a;
            } else {
                break 'b;
            }      
        }
        break;       
    }
}
```  
**Answer: Terminates after the first iteration of the loop**

# Question 20  

Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.  

```Rust

fn main() {
    let a = [5; 10];
    let mut sum = 0;
    for x in a {
        sum += x;
    }
    println!("{sum}");
}
```  
**Answer: Compiles + output is 50**