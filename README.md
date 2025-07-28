# minigrep
Search for words in a target file with case sensitive and insensitive options, mimicking the grep function. <br>
This project is from [The rust programming language](https://doc.rust-lang.org/book/title-page.html) and acts as the first project to get a strong handle on the basics of rust.

Notes created during the project can be found in markdown files available in the repo.
## Cloning the repo
`git clone https://github.com/CameronJules/minigrep.git`

## Running tests
This project focuses on idomatic rust and test driven development. To run tests after cloning the repo copy the following commands into the terminal (macOs).
```
cd minigrep
cargo test
```
## Running minigrep
The minigrep program takes commandline arguments to specify the search word and the path to the file being searched. <br> <br>
The search path is from the root directory. Therefore, putting the search file poem.txt in the root directory (the same direcotry as main.rs) will allow access like so:
```
cargo run -- to poem.txt
```
This will search for the word "to" in poem.txt. By default the search will be case sensitive
### Case insensitive
To run minigrep as case insensitive, the environment variable IGNORE_CASE must be set. <br> <br>
On macOs this can be done in the terminal like so:
```
IGNORE_CASE=1 cargo run -- to poem.txt
```

