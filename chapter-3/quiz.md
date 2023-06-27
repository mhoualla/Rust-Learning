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