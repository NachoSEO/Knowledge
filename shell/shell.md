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
* `wc`: word count program
  * `wc -l <file>`: count the number of lines in a file
* `sort`: sort elements
  * `sort -nk1,1`: sort the elements using the `-n` option to sort numerically and the `-k` option to sort by the first column and ending in the first column `1,1`
* `uniq`: remove duplicates
  * `uniq -c`: Precede each output line with the count of the number of times the line occurred in the input, followed by a single space.
* `head -n10`: print the first few lines (10 in this case) of a file
* `tail -n10`: print the last few (10 in this case) lines of a file
* `paste`: paste together several files
  * `paste -sd`: print the output in a single line, with the delimiter `-d` (comma by default)
* `bc`: Binary Calculator, calculates from the standard input
* `xargs <command>`: takes a list of inputs and transform them in arguments for a given command
  * `xargs open < links.txt`: open a list of URLs from a text file
* `tee`: copy the input to the output and also print the output


## Tools & Scripting
Scripts need not necessarily be written in bash to be called from the terminal. If we include the shebang the kernel will now how to interpret the file as a script.

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

#### Example functions
* `dig -t txt <domain.com>` - To see the TXT DNS records of a given domain

#### Useful args
* `$0` - Name of the script
* `$1` to `$9` - Arguments to the script. $1 is the first argument and so on.
* `$@` - All the arguments
* `$#` - Number of arguments
* `$?` - Return code of the previous command
* `$$` - Process identification number (PID) for the current script
* `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing sudo !!
* `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing Esc followed by .

### Wildcards & Curly braces
* `*` - matches any string of characters
* `?` - matches any single character
* `{}` - matches a set of characters (Common substrings)

For instance, given files foo, foo1, foo2, foo10, bar, image.jpg, image.png:
* `foo*` - matches foo, foo1, foo2
* `foo?` - matches foo, foo1, foo2, foo10
* `foo{1,2}` - matches foo1, foo2
* `image.{jpg,png}` - matches image.jpg, image.png

### Finding files & Code

#### `find`
We can find files using `find` function.

```sh
# Find all directories named src
find . -name src -type d

# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f

# Find all files modified in the last day
find . -mtime -1

# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'

# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;

# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```
#### `grep`
We can use `grep` to search for specific strings in files.

##### Useful flags
* `-i` - Case insensitive
* `-v` - Invert the search, not maching the pattern
* `-n` - Print line number
* `-w` - Print the whole line
* `-c` - Count the number of lines that match
* `-C` - To give context to the search
* `-R` - Recursive search

```sh
grep "this" demo_file.txt
# this line is the 1st lower case line in this file.
# Two lines above this line is empty.
# And this is the last line.

```
### Flow control & loops

#### Loops
```sh
for i in {1..9}; do
  echo i
done
```

#### Conditionals
All available [Bash conditionals](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)

```sh
if [["$?" -ne 0]]; then
  echo 'Do whatever'
fi
```

## Data Wrangling
To transform data from one format to another.
For example if we connect to a remote server to watch the logs: `ssh <server_name> journalctl` and we want to extract some data we'll need to perform some transformations.
Example:
1. `ssh <server_name> journalctl | grep sshd` --> to get the sshd logs in my local computer (without saving any data)
2. `ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' > ssh.log` -> we use the commas to execute that command in the server and save the output in a file in our local computer
3. `less ssh.log` --> to add pagination in the command line 

### `sed`
Stream editor. It's a command to modify texts in inputs that builds on top of the old ed editor.

#### Useful flags
* `-E` - Enable extended regexp (More modern Regexp)


#### Expressions
* `s/` -> substitue expression. It needs two parameters: the first one is the pattern to be replaced and the second one is the string to replace it with (`/` To delimit params).
  * Example: `s/.*Disconnected from//` -> to replace `Disconnected from` and all that comes before for blank. `.*` (ReGex)
  * `sed s/REGEX/SUBSTITUTION/`

If we have a log file `ssh.log` similar to this:
```
Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from invalid user whatever 46.97.239.16 port 55920
```
You could run something like this to extract just the user:
```sh
cat ssh.log | sed -E 's/^ .*? Disconnected from (invalid )?user (.*) [0-9]+ port [0-9]+$/\2/'
# whatever
```
The last `\2` means that we want to extract the second capture group, pattern inside the second parenthesis. 

### `awk`
Columnarwiser. It's a command to extract data from text files and to return in columnar format.

* `awk {print $2}`: Prints the second column.
* `awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }'`: The pattern says that the first field of the line should be equal to 1. The second field should match the given regular expression.

## Command line environment

### Alias
A shell alias is a short form for another command that your shell will replace automatically for you. There is no space around the equal sign `=`.

* `alias alias_name="command_to_alias arg1 arg2"`

Aliases do not persist shell sessions by default. To make an alias persistent you need to include it in shell startup files, like `.bashrc` or `.zshrc`

### Dotfiles
Here you can include commands that you want to run on startup, like the alias we just described or modifications to your PATH environment variable.

Some other examples of tools that can be configured through dotfiles are:

* bash - ~/.bashrc, ~/.bash_profile
* git - ~/.gitconfig
* ssh - ~/.ssh/config


### Remote machines
#### `ssh`
SSH is a secure shell, which allows you to connect to a remote computer and execute commands on it.
* `ssh foo@bar.mit.edu` - Here we are trying to ssh as user foo in server bar.mit.edu

You can also use the `-p` flag to specify a port number & execute commands:
* `ssh foobar@server ls | grep PATTERN` - Here we are trying to ssh as user foobar in server `server` and execute the command `ls` and `grep` for the pattern `PATTERN` locally.
  * If you run the command before the `ssh`and then a pipe you will run the command in the server.

You can create SSH keys to connect to remote machines without entering a password. Check (encryption)['../encryption/encryption.md'] for more information.
