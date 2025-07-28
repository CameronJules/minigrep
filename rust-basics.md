# Rough notes for Rust basics
[Read the Book!](https://doc.rust-lang.org/book/)
## Creating a rust project
`cargo new [project-name]` this creates a new directory for the rust project with the defined project name. 

Within this project there will be two files:
1) cargo.toml , this stores meta data for the project as well as dependencies
2) src/main.rs, this is where the application code will go
Doing this creates a hello world project which can be run by moving into the directory and running `cargo run`

**cargo run**  compiles and runs the application.

## Adding dependencies 

You can either add it directly to the cargo.toml file with then name and version. `ferris-says = "0.3.1"`

or you can use `cargo add ferris-says`

to add the dependencies to the cargo.toml file. Once add you can then run `cargo build` to install the dependencies

### Using dependencies

`use dependency-name::module;`


## The main function

```
fn main() {
	code goes here
}
```

## Basic rust syntax

### Utils
- `fn main()` **always appears** at the beginning of all rust programs
- Lines must be **followed by semi-colons**
- `println!()` is a **macro used to print text**, these executing things like functions but dont always follow the same rules as a function. `print!()` can be used to print without new lines
- **Comments** are `// Single line comment` or `/* multi line comment */`

### Variables
- **set variables** using `let name = "Cam";` this cannot be reassigned by default to allow **mutability** use `let mut name = "Cam";`
- **Print placeholders**, to add variables into a print string use `println!("Hi {}, nice to meet you", name);`
- **Datatypes**, i32, f64, bool, &str, char. 
	- char is defined by `'D'`
	- string defined by `"Hello"`
	- bool is `true` `false`
	- `let age: i32 = 25;`
- **Constants** are defined using `const CAP_NAME: i32 = 1980;` when setting constants the **must have a type**

### Operators & Conditionals
- Operators are the same as other languages, includes assignment with`+=`
- Boolean operators &&, ||,  !

- If else is java like and similar to golang, 
```
  if condition {
  
  } else if {
  
  } else {
  
  }
```
- You can also use if for varaible assignment 
  `let greeting = if time < 18 {"good day"} else {"good evening"};`
- `match` is similar to switch case rather than writing lots of if else statements `match day {1 => print!("Monday)}` the default case is `_ => default`

### Loops

Rust has **3 basic loop types**
1) **loop**, which will run forever,  you must use break to stop it. You can assign a value of the loop to a result.
2) **while** loops
3) **for** loops
	1) `[range) non inclisve of end - for i in 1..6` 
	2) `inlcusive for i in 1..=6`
Rust also has break and continue


### Functions
similar to Goland and java tou must define input param types and return value types. You can ommit the return key word and leave the value without a semi colon

`fn func(a: i32, b :i32_ -> i32 {return a + b;}`

multi return values `-> (i32, i32)`

### Strings

`&str` is string slices and is used for text that cant change
`String` is for text that can change.

`let text = "Hello".to_string();`
or
`let text = String:from("hello world");`

**To change a string**

`let mut name = String::from("hello");`
`name.push_str(" world");`

you can use word.push() to add only one character

TO **concatenate** string use `format!("{} {} {}", s1, s2, s3)` or  +

**string length** `name.len()`

### Ownership

Variables own the data, and the data can only have one owner at a given time unless borrowed. This doesnt apply to simples like booleans or numbers as they get copied.

rust use this to free memory when no longer needed. when owner leaves scope the memory is freed.

```
let a = String::from("Hello");  
let b = a;  
  
// println!("{}", a); Error: **a** no longer owns the value  
println!("{}", b); // Ok: b now owns the value
```

if you want to have a and b have the same data then when reassigning you must clone

```
let a = String::from("Hello");  
let b = a.clone(); // Now both have the same value  
  
println!("a = {}", a);  // Works  
println!("b = {}", b);  // Works
```

### Borrowing
If you want to look at a value without taking ownership you can use &

```
let a = String::from("Hello");  
let b = &a;  
  
println!("a = {}", a);  
println!("b = {}", b);
```
if you want to change the referenced value use &mut

```
let mut name = String::from("John");  
let name_ref = &mut name;  
name_ref.push_str(" Doe");  
  
println!("{}", name_ref); // John Doe
```


### Data structures
to **print values in these data structures** use `println!({:?}, data);` must use **{:?}** 

##### Arrays 
these are fixed size and cannot be changed once created. you can reassign values via regular indexing. All data is of same type `let arr = [1,2,3];`

loop through `for a in arr` no need to borry if types are simple

##### Tuples
Also cannot be changed but can hold any value. This can be packed and unpacked. so if you have a tple the variables can be resplit
`let person = ("John", 30);`
`let (name, age) = person`

elements accessed with indexes `person.0` would get the name


##### Vectors
These are dynamic arrays **created** by `let mut fruits = vec!["apple", "banana"];` **vec!**
element are also accessed by indexes

**add element** `fruits.push("cherry");`

**remove** elements f**rom the right** `fruits.pop();`

**remove by index** `fruits.remove(0)`

**loop** through `for f in &fruits {}`

to edit values as you iterate use &mut fruits

##### Hashmaps
```
// Import HashMap  
use std::collections::HashMap;  
  
fn main() {  
  // Create a HashMap called capitalCities  
  let mut capitalCities = HashMap::new();  
  
  // Add keys and values (Country, City)  
  capitalCities.insert("England", "London");  
  capitalCities.insert("Germany", "Berlin");  
  capitalCities.insert("Norway", "Oslo");  
  
  println!("{:?}", capitalCities);  
}
```


**Accessing hashmap values**
**use .get()** on the keyword

**Some**, `Some(value)` allows you to directly assing into a for loop. this is an option enum where it either takes a value or is None.

**Remove values**, using `remove(key)`

to loop through a hashmap use
```
for (key, value) in &hashmap {}
```


##### Structs
allows you to group datavalues together into a fixed form, similar to go structs
```
struct Person {  
  name: String,  
  age: u32,  
  can_vote: bool,  
}
```

##### Enums
https://www.w3schools.com/rust/rust_enums.php
This creates a type that can be one of few values, each value is called a variant.

[[Command Line Program - grep example]]
**impl** is used to define methods and associated functions for structs and enums

```
struct Config {
	name: String,
	age: i32,
}

impl Config {
	fn new(args) -> Config {
		// do logic and return config struct
	}
}

let config = Config::new(args);
```

notice how this is the constructor for hashmaps. it should be noted that programmers expect `new` not to fail so if there is a case that the instantiattion could fail `build` should be used instead

### Error handling

You can use panic() for non recoverable situations

`Result<Config, &'static str>`  can also be returned, this is an enum that is either Ok or Err string.

Note: `'static str` means it has a lifetime for the entire duration of the program. 

`unwrap_or_else` is defined on the result enum and gives logic to either unwrap the return value or run code to handle the error.

You can return a result where the error implements type trait and is dynamic allowing varied range of errors to be returned.  now instead of using expect and returning a basic error `?` can be added after will return any error from the function called.

**Summary:**
Errors can be handled in may ways. In rust errors are recoverable on unrecoverable. Each of which is handled differently unlike other language that use except

1) The `panic!` macro can be used to stop the program during un recoverable errors. This does however give information that might be unnecessary to the users.
2) `.expect` can be used after a function to return a message based on the `Option<T>` . The option from a function returns a value or None, if it is none then the expect message will be shown
3) `Result<returnType, &'static str>` This returns the value or a static str which means it will live for the duration of the program. From in the function you simply return Err("message") if something goes wrong
4) At a higher level there may be many errors that can be thrown by a function not just one so you need a dynamic return which can be done using traits. `Result <(), Box<dyn Error>>`. This returns the unit type meaning the function doesnt ususally return anything it just runs other functions. and then the trait object implements type error. 


### Traits
These are like abstract classes, any object that implements the trait will have a specific set of functions defined by the trait.

### Lifetime parameters
When you are returning values from a function you might get a lifetime error. This could be because the return value defined could be a reference to input variables that have different lifetimes and rust doesn't know which one to use when evaluating the code. Generic lifetime params can be added to allow the borrow checker to perform analysis. 
Done using `'` and a reference. So `'a`

Defining the function:
`fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {}`
says that all of the variables have a lifetime tied to `'a` so you say that whatever is returned is guaranteed to live at least as long as `'a`

Note that knowing which input we want to keep referencing after the function is how we know where to put the lifetime parameter. 
Example:
```
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {

Vec![]

}
```
This function will return lines from the content, so the lifetime parameter needs to be on content. More generally lifetime params are needed when you function returns a reference that is related to an input reference, and rust cant figure out how long the returned reference should be valid.
