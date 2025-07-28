# Minigrep rough notes
These notes come from here: https://doc.rust-lang.org/book/ch12-00-an-io-project.html
The notes follow through as code changes. Some notes will cover code that doesn't exist as it has been iteratively improved, it has been noted down as there are still take aways and examples to learn from.
## Accepting Arguments

To accept arguments we use the args module from std::env, we dont directly import std::env:args as it could cause confusion about if it is a function we defined or imported.

We collect the arguments into a vector string and assign the values we want to variables making sure to borrow the values.

`dbg!()` is debugging tool to print the expression and its value to the standard err (output to terminal) along with the file name and line number, this lets you peek into what is happening at that point.

## Reading files
read files in using `std::fs` and the function is `read_to_string()`

Here `expect` can be used to show error messages if nothing is returned.

This can be used any time you have an option.
for example:
```
let name = Some("Alice");
let unwrap = name.expect("Name missing");

println!("{unwap}")
```

## Refactoring

Ideally we have **separation of concerns** as the program grows the **function code should be put in lib.rs** and **imported to main.rs** where functions are called and run.


### Maintainability.
We want to correctly handle command line arguments in a way that clearly has purpose.
	1) When we **refactor args parsing into a function** we return a tuple only to immediately unpack. This is a sign that the structure needs to be improved. To do this we can **use a struct and return it,** accessing the values in the struct rather than unpacking or using indexes that can get messy. **groups values by their use rather than random unpacking**
	2) We use clone to take the string values from the args vector because of the ownership of the variable. **Cloning creates a deep copy** allowing it to now be its own value in config. you can borrow the value. but it will then be tied to the lifetime of the vector it was cloned from.
	3) Note: `let path = "some/path"; println!("{path}");` is allowed as a named variable but field access isnt allowed if the variable is not basic. so you must use the long method. `println!("{}", config.path);`
	4) We initially had a parse_config function, but since it **instantiates the configs** now we are using a struct, the **idomatic way is to create a constructor**` impl Config { fn new() {}}`. **impl** is used to **define methods and associated functions** for types like struct and enum.

### Error Handling
we want to account for the known issues that can happen in the program and if they are not recoverable then we should panic with a clear message

Depending on the function instead of panic we might want to return a result that can then be handled if it is erroneous.

You can use panic() for non recoverable situations

`Result<Config, &'static str>`  can also be returned, this is an enum that is either Ok or Err string.

Note: `'static str` means it has a lifetime for the entire duration of the program. 

`unwrap_or_else` is defined on the result enum and gives logic to either unwrap the return value or run code to handle the error.

A further error handling can be done using trait objects


### Applying separation of concerns
Move all program logic into a single run function to be called in main. This is everything not involved in setting up configurations.

Extract all code (functions and structs ect) that isnt the main function into the src/lib.rs

to then make the functions accessible add pub infront of the function.

Any access to struct variables also need to have `pub` in front of them to be accessed externally.

then `use minigrep::Config` this is the name of whatever pub code we want to use and have made available from within lib.rs


## Search function with test driven development

in the code mark test with `#[cfg(test)]`

```
#[cfg(test)]

mod tests {
	use super::*;
	
	
	#[test]
	fn one_result() {
	
		let query = "duct";
		
		let contents = "\	
Rust:
safe, fast, productive.
Pick three.";
  
		assert_eq!(vec!["safe, fast, productive."], search(query, contents));
		
		}

}
```

Define a test for how you want the function to work then develop the code just enough to pass the test, then continue. See above how tests are defined

Here we need to define the search function also using lifetime references to ensure the borrow checker can check how long variables live and are valid.

To implement search iterate through lines and check if each has the search query using `.contains(query)`.
if it does push the line to the result vector.

## Using environment variables

We use env::var to get environment variables, this returns a result of Ok() or Err. we use is_ok() to check if the var is set and then proceed accordingly.

this can be set in command line by writing them before the cargo run line

`IGNORE_CASE=1 cargo run -- to poem.txt`

## Print errors to standard error
Doing this ensures that if we save the output to a file only successful data will be saved to the file instead of error messages. Use `eprintln!()` to print to standar out
