# The Missing Semester of Your Computer Science Education

## Shell

### What is Shell?

**Shell** is a computer program that exposes system's services to a human user or other programs.

$$\mathrm{User} \Leftrightarrow \mathrm{Shell} \Leftrightarrow \mathrm{Kernel}$$

### Using Shell(Linux)
```
re4urnzer0@things:/home/re4urnzer0
--> 
```
This is the main textual interface to the shell.
- `re4urnzer0` means **user's name**
- `things` means **machine's name**
- `/home/re4urnzer0` means **current working directory**

### Some examples

#### `date`
Prints the current date and time.

#### `echo` (with arguments)
Simply prints out its arguments.
(If arguments include *whitespace*, you should quote the argument with `'` or `"`, or escape just the relevant characters with `\`.)
```
echo My Photos (It will create two directory, /My and /Photos)

echo My\ Photos (equals the following formats)
echo 'My Photos'
echo "My Photos"
```

### About Programming Environment
When you run commands in your shell, you just writing a small bit of code that your shell interprets.

All of them (date, echo...) are included in **$PATH**, it consults some *environment variables*.

### Navigating in Shell
`pwd` -> **present work directory**
`cd` -> **change direcotry**
  - `.` -> **current direcotry**
  - `..` -> **parent direcotry**
  - `~` -> **home directory**
  - `-` -> **directory before switch**

`ls` -> **listing what lives in a given directory**
  - `-l` -> **use a long listing format**
(About permissions, `d` means this is a *directory*, `r` means *read permission*, `w` means *write permission*, `x` means *execute permission*.)

#### Other Programs
`cp`, `mv`......

### Ask for help for some shell programs
We use the `man` program to get more information about program's arguments, inputs, outputs or how it works in general.

### Connecting Programs
In the shell, programs have two primary "streams" associated with them: their **input stream** and **output stream**.
That means we can **rewire** those streams by using `>` and `<`.
- `>` -> rewire **output stream**
- `<` -> rewire **input stream**

We can use `>>` to **append to a file**.

### Shell Scirpting

Most shells have their own scripting language with *variables, control flow and its own syntax.*
For this section we will focus on **bash** scripting since it is the most common.

#### basics on Bash scripting
- Assign variables in Bash:
  ```
  foo=bar
  echo "$foo"
  # prints bar
  echo '$foo'
  # prints $foo
  ```
  (Strings in bash can be defined with `'` and `"` delimiters, but they are **not equivalent.** Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimiterd strings will.)
- Functions with arguments
  ```
  mcd () {
    mkdir -p "$1"
    cd "$1"
  }
  ```
  `$1` means the *first argument* to the script/function.
  Unlike other scripting languages, bash uses a variety of special variables to refer to arguments, error codes, and other relevant variables. More information about special variables in <a href="https://tldp.org/LDP/abs/html/special-chars.html">Here.</a>
  Some examples:
  - `$0` **Name of the script**
  - `$1` to `$9` **Arguments to the script.** *`$1` is the first argument and so on.*
  - `$@` **All the arguments**
  - `$#` **Number of arguments**
  - `$?` **Return code of the previous command**
  - `$$` **Process identification number(PID) for the current script**
  - `!!` **Entire last command, including arguments.** *A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`*
  - `$_` **Last argument from the last command.** If you are in interactive shell, you can also quickly get this value by typing `Esc`  followed by `.` or `Alt+.`

