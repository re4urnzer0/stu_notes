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