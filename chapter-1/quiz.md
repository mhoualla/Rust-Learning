# Question 1
What is the name of the command-line tool for managing the version of Rust on your machine?  
**Answer:**  rustup  
**Context:** For example, you can write *rustup update* to get the latest version of Rust.  

# Question 2
Every executable Rust program must contain a function with the name:  
**Answer:** main  
**Context:** In your program, you add a main function like this:  

fn main() {  
  // your code here  
}  

# Question 3
Let's say you have the following program in a file hello.rs:  

fn main() {  
  println!("Hello world!");  
}  

Say you then run the command *rustc* hello.rs from the command-line. Which statement best describes what happens next?  
**Answer:** rustc generates a binary executable named hello  
**Context:** Running rustc checks and compiles your program, but does not execute it.  

# Question 4
Say you just downloaded a Cargo project, and then you run *cargo run* at the command-line. Which statement is NOT true about what happens next?  
- "Cargo downloads and builds any dependencies of the project"  
- "Cargo builds the project into a binary in the `target/debug` directory"  
- "Cargo executes the project's binary"
- "Cargo watches for file changes and re-executes the binary on a change

**Answer:** Cargo watches for file changes and re-executes the binary on a change.  
**Context:** Cargo does not watch your files by default. But you can use plugins like [cargo-watch](https://crates.io/crates/cargo-watch) for this
purpose.