# Shell
Textual interface

Partly based on [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)

## Basic commands
* `date`: prints the current date
* `echo`: prints out its arguments. we can add `'` or `"`
  * `echo $PATH`: list of all environment variables
  * `../../bin/echo hello`: Will print `hello` because instead of giving the program `echo` we give it the `$PATH`
* `which <command>`: find out which file is executed for a given program name
* `pwd`: Print currenty directory
* `cd`: move to specific directories
  * `cd ..`: move to the directory before
* `man <command>`: To see the manual of that program
* `cat <file>`: To see the contents of that file in the shell
* `touch <file>`: To create a file
* `echo whatever > file.txt`: Will add `whatever`to `file.txt``
  * The simplest form of redirection is `< file` and `> file`
  * `>>`: To append to a file
  * `echo | date -r <file> > last-modified.txt`: Add to `last-modified.txt`the last modification date of the `<file>`
* `|`: pipe operator. Lets you chain programs
* `sudo`: it lets you “do” something “as su” (short for “super user”, or “root”)
* `sh`:There are several shells, of which bourne is the old standard, installed on all unix systems, and generally the one you can guarantee will exist.
  * If we have a file with `#!/bin/sh` as a first line, it only will work with the Bourne Shell, so we'll need to run the command `sh <file>`.
  * Also `#!` is called a [Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))
* We cand send to the STDERR with `2>` instead of `>`

## Tools & Scripting

### Template strings
```sh
foo=bar
echo "value is $foo"
# value is bar
echo 'value is $foo'
# value is $foo

dir=$(pwd)
echo "I'm in $dir"
# I'm in users/desktop/temp
```

### Functions
We can create shell functions like these (in the CLI or in files):
```sh
mcd() {
  mkdir -p "$1"
  cd "$1"
}
```
This function will create a directory using the first argument `"$1"`and then it will move to it.

We can add this functions to the CLI using `source <file>` in order that the shell remembers it

We can use short-circuit with `||`and `&&`.

#### Useful args
* `$0` - Name of the script
* `$1` to `$9` - Arguments to the script. $1 is the first argument and so on.
* `$@` - All the arguments
* `$#` - Number of arguments
* `$?` - Return code of the previous command
* `$$` - Process identification number (PID) for the current script
* `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing sudo !!
* `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing Esc followed by .

### Flow control & loops

#### Loops
```sh
for i in {1..9}; do
  echo i
done
```

#### Conditionals
```sh
if [["$?" -ne 0]]; then
  echo 'Do whatever'
fi
```