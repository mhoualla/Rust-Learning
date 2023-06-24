# Hello, World!
- Rust files always end with the .rs extension
# Anatomy of Rust Program
- Main function is always the first code that runs in every executable Rust program
- Rust requires curly brackets around all function bodies
- Formatter tool: *rustfmt*
- *println!* calls a Rust macro. If it had called a function instead, it would be entered without the exclamation mark (i.e., *println*)
- Macros don't always follow the same rules as functions
- Strings inside quotation marks... code ends with a semicolon


# Good Practices
- It's good style to place the opening curly bracket on the same line as the function declaration, adding one space in between
- Rust style is to indent with four spaces, not a tab


# Compiling and Running Are Separate Steps
- Before running a Rust program, you must compile it using the Rust compiler by entering the rustc command and passing it the name of your source file
- For example, *rustc main.rs*
- This is very similar to gcc or clang - after compiling successfully, Rust outputs a binary executable.
- Rust is an ahead-of-time compiled language, meaning you can compile a program and give the executable to someone else, and they can run it even without having Rust installed.
- If you give someone a .rb, .py, or .js file, they need to have a Ruby, Python, or JS implementation installed. But in those languages, you only need one command to compile and run your program.
- Just compiling with *rustc* is fine for simple programs, but as your project grows, you'll want to manage all the options..

# Hello, Cargo!
Cargo is Rust's build system and package manager 
- Tool for managing Rust projects ... building code, downloading libraries your code depends on, building those libraries (dependencies)
- If we wrote our first program using Cargo, it would only use the part of Cargo that handles building your code. As we write more complex Rust programs, we introduce more dependencies -- It's easier to manage this using Cargo. 

# Creating a Project with Cargo
- Running cargo new hello_cargo will create a new directory and proejct called *hello_cargo*
- Cargo creates its files in a directory of the same name.
- Cargo has generated two files and one directory for us: a *Cargo.toml* file and a *src* directory with a *main.rs* file inside. 
- Also initialized a new Git repository along with a *.gitignore* file. Git files won't be generated if you run *cargo new* within an existing Git repo; you can override this behavior by using *carg new --vcs=git*

# Cargo.toml
- This file is in the TOML (Tom's Obvious, Minimal Language) format, which is Cargo's configuration format
- The first line, *[package]*, is a section heading that indicates that the following statements are configuring a package. 
- The next three lines set the configuration information Cargo needs to compile your program: the name, the version, and the edition of Rust to use.
- The last line, *[dependencies]* is the start of a section for you to list any of your project's dependencies.
- In Rust, packages of code are referred to as *crates*

# src/main.rs
- Hello world program
- Only difference is that the project Cargo generated placed the code in the *src* directory and we have a *Cargo.toml* configuration in the top directory. 
- Cargo expects your source files to live inside the *src* directory. Top-level is just for README, license, config files, etc.

# Building and Running a Cargo Project
- *cargo build*
- *./target/debug/hello_cargo* .. Or, alternatively: *cargo run*
- Notice that this time we didn't see output indicating that Cargo was compiling *hello_cargo*
- Cargo figured out that the files hadn't changed, so it didn't rebuild but just ran the binary.
- Cargo also provides a command called *cargo check*. This command quickly checks your code to make sure it compiles but doesn't produce an executable.

Why wouldn't you want an executable?
- Often, *cargo check* is much faster than *cargo build* because it skips the step of producing an executable. 
- If you're continually checking your work while writing the code, using *cargo check* will speed up the process of letting you know if **your project is still compiling**

# Recap about Cargo
- We can create a project using *cargo new*
- We can build a project using *cargo build*
- We can build and run a proejct in one step using *cargo run*
- We can build a project without producing a binary to check for errors using *cargo check*
- Instead of saving the result of the build in the same directory as our code, Cargo stores it in the *target/debug* directory 

# Building for Release
When your proejct is finally ready for release, you can use *cargo build --release* to compile it with optimizations. 
- Creates executable in *target/release*
