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
```bash
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
  ```bash
  foo=bar
  echo "$foo"
  # prints bar
  echo '$foo'
  # prints $foo
  ```
  (Strings in bash can be defined with `'` and `"` delimiters, but they are **not equivalent.** Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimiterd strings will.)
- Functions with arguments
  ```bash
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

- Program Return Code
Commmands will return *output* using `stdout`, *error* with `stderr`.
The **Return Code** or **exit status** is *the way scripts/commands have to communicate how execution went.*
Generally, A value of `0` means *everything went OK*; **anything different from 0** means *an error occurred.*
Some examples like:
  ```bash
  false || echo "Oops, fail"
  # Oops, fail

  true || echo "Will not be printed"
  # 

  true && echo "Things went well"
  # Things went well

  false && echo "Will not be printed"
  #

  true ; echo "This will always run"
  # This will always run

  false ; echo "This will always run"
  # This will always run
  ```
- Command substitution
**Get the output of a command as a variable.**
Whenever you place `$( CMD )` it will execute `CMD`, get the output of the command and substitute it in place. For example, if you do `for file in $(ls)`, the shell will first call `ls`, and then iterate over those values. A lesser known similar feature is **process substitution**. Be like `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file's name. This is useful when commands expect values to be passed by file instead of by `STDIN`. A example: `diff <(ls foo) <(ls bar)` will show differences between files in dirs `foo` and `bar`.

A complete example:
  ```bash
  #!/bin/bash

  echo "Starting program at $(date)" # Date will be substituted

  echo "Running program $0 with $# arguments with pid $$"
  # $0 -> name of the script
  # $# -> number of the arguments
  # $$ -> PID of the current script

  for file in "$@"; do
      grep foobar "$file" > /dev/null 2> /dev/null
      # When pattern is not found, grep has exit status 1
      # We redirect STDOUT and STDERR to a null register since we don't care about them
      if [[ $? -ne 0 ]]; then
          echo "File $file does not have any foobar,  adding one"
          echo "# foobar" >> "$file"
      fi
  done
  ```

This program will iterate through the arguments we provide, `grep` for the string `foobar`, and append it to the file as a comment if it's not found.
(When performing **comparisons** in bash, **try to use double brackets `[[ ]]` in favor of simple brackets `[ ]`**. More details in <a href=http://mywiki.wooledge.org/BashFAQ/031>Here.</a>)

- Globbing (expanding expressions by carrying out filename expansion)
  - Wildcard
    - `?`: match **one** character
    - `*`: match **any amount of** characters
  - Curly braces `{}`
  Whenever you have a **common substring** in a series of commands, you can curly braces for bash to expand this automatically. (Frequently using in *moving or converting files.*)
  Simple example:
  ```bash
  convert iamge.{png, jpg}
  # will expand to 
  convert image.png image.jpg

  cp /path/to/project/{foo,bar,baz}.sh /newpath
  # will expand to 
  cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

  # Globbing techniques can also be combined
  mv *{.py,.sh} folder
  # will move all *.py and *.sh files

  mkdir foo bar
  # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... /h
  touch {foo, bar}/{a...h}

  touche foo/x bar/y
  #show difference between files in foo and bar
  diff <(ls foo) <(ls bar)
  #Outputs
  #< x
  #---
  #> y
  ```
Scripts **not necessarily be written in bash** to be called from the terminal. We can use other program language to call bash script, belike:
```python
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```
The kernel knows to execute this script with a python interpreter instead of a shell command because the first line we called <a href=https://en.wikipedia.org/wiki/Shebang_(Unix)>shebang</a>.

Some differences between shell functions and scripts that you should keep in mind are:
- Functions have to be in the same languages as the shell, while script can be written in any language. This is why including a shebang for scripts is important.
- Functions are loaded once when their definition is read. Scripts are loaded every time they are executed. This makes functions slightly faster to load, but whenever you change them you will have to reload their definition.
- Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g.change your current directory, whereasa scripts can't. Scripts will be passed by value environment variables that have been exported using `export`
- As with any programming language, functions are powerful construct to achieve modularity, code reuse, and clarity of shell code. Often shell scripts will include their own function definitions

### Shell Tools

#### Finding how to use commands
- Using `man` or `TLDR pages`
```bash
man rm
# or 
tldr rm
```

#### Finding files
We can use `find` to find files or directories.
```bash
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.taz.gz'
```
