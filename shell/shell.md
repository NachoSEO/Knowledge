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

## Tools & Scripting