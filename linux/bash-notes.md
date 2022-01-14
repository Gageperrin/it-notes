# Bash Scripting

## Sanity Check Bash List

1. Does the script start with shebang `#/bin/bash`?
2. Are there comments to describe the script and its components?
3. Are there global variables defined after initial comments at the top of the script?
4. Are all functions grouped together following global variables?
5. Are there local variables?
6. Does the main body of the shell script follow the functions?
7. Does the script exit with an explicit exit status?
8. Are exit status used at various exit points?

## Basic Syntax

```
#! /bin/bash
echo "Hello world"
```

`#!` called the shebang. What follows is used to identify the interpreter. If a shebang is not specified, the commands are executed through the shell.

A shell script needs to be executable before it can be executed (`chmod 755`). If it is not in `$PATH` its local or absolute directory needs to be specified.

Variables can be used in shell scripts through syntax `VARIABLE_NAME=variable`.

To invoke a variable set a `$` before the variable (`$VARIABLE_NAME` or `${VARIABLE_NAME}`). Curly braces are optional if the variable has whitespace on both sides and is not embedded in a string or other form of data (`${VARIABLE_NAME}s`).

To assign the output of a command to a variable enclose the command in parentheses. `CURRENT_DIR=$(pwd)`. Backticks can also be used but it is best practice to use `$()` syntax instead.

Variable names can contain letters, digits and underscores. They cannot start with a digit or include other kinds of characters.

`#` signs are ignored except with the shebang. These are used for comments. Anything following a `#` is ignored on that line.

Logical operators are `&&` for AND and `||` for OR based on the exit status of the command.

To combine multiple commands on a single line use `;`. The previous command's exit status is not evaluted, so all commands will be executed.

## Tests

Tests can be used to implement conditional logic: `[ condition ]`.

If the test returns true it exits with a 0 status, a false result returns with status 1.

Various types of tests include:
* `-d FILE` true if file is a directory
* `-e FILE` true if file exists
* `-f FILE` true if file exists and is a regular file
* `-r FILE` true if user can read file
* `-s FILE` true if file exists and is non-empty
* `-w FILE` true if user can write to file
* `-x FILE` true if user can execute file
* `-z STRING` true if string is empty
* `-n STRING` true if string is not empty
* `STRING1 = STRING2` true if strings are equal
* `STRING1! = STRING2` true if strings are not equal
* `arg1 -eq arg2` true if `arg1` is equal to `arg2`
* `arg1 -ne arg2` true if `arg1` is not equal to `arg2`
* `arg1 -lt arg2` true if `arg1` is less than `arg2`
* `arg1 -le arg2` true if `arg1` is less than or equal to `arg2`
* `arg1 -gt arg2` true if `arg1` is greater than `arg2`
* `arg1 -ge arg2` true if `arg1` is greater than or equal to `arg2`


## If statements

Syntax: 
```
if [ condition ]
then
    command 1
    command 2
elif [ condition-B ]
    command A
    command B
else
    command X
    command Y
    command Z
fi
```

It is best practice to enclose variables in quotes when performing conditional tests.

## For loop

Syntax:
```
for VARIABLE in ITEM_1 ITEM_N
do
    command 1
done
```

The item is assigned to the variable which is then run in the command. After the command, the loop iterates and assigns the next item to the variable and runs the command again. The loop completes after the last item.

Script that echoes all `.txt` file names in current directory:
```
#!/bin/bash

FILES=$(ls *txt)

for FILE in $FILES
do
    echo ${FILE}
done
```

## Positional parameters

Positional parameters are variables that contain the contents of the command line from `$0` to `$9`.

The script is stored in `$0`, the first parameter in `$1` and so on.

`$@` can be used to access all positional parameters starting at `$1`. This can be used to loop through all parameters.

## STDIN

The `read` command accepts STDIN and assigns it to a specified variable.

Syntax: `read -p "PROMPT" VARIABLE`

## Exit Status and Return Codes

Every command returns an exit status between 0 and 255. By default convention, `0` is a success code. Anything other than `0` is an error condition. `man` can be used to find the meaning of various exit statuses for each command.

`$?` accesses the return code of the previous executed command.


## Functions

Functions are used to create replicable code that needs only to be written once. A function must be defined before use and have parameter support.

A function can be created with:
```
functionName() {

}
```

`functionName()` can also be written `function functionName()`. Invoke the function with `functionName`.

Functions also accept positional parameters, just like shell scripts. `$0` still calls the entire script itself, not a particular function. Use `return 1` to have the function exit with a status code of `1`.

By default, variables are global but have to be defined before used. A local variable can only be accessed within the function and is set by prepending the variable assignation with `local`. Only functions can have local variables. It is best practice to keep variables local to each script.


## Wildcards

A wildcard is a character or string used for pattern matching. Globbing expands the wildcard pattern into a list of files and/or directories (i.e. paths). Wildcards can be used with most commands. Escape a wildcard character with `\`. Best practice to not include wildcard characters in file name.

Characters:
* `*` matches zero or more characters.
* `?` matches exactly one character.
* `[]` matches any of the characters listed between the brackets. Matches exactly one character.
* `[!]` excludes characters listed between the brackets. Matches exactly on character.


Named Character Classes:
* `[[:alpha:]]` matches upper and lower case alphabetic characters
* `[[:alnum:]]` matches alphanumeric characters
* `[[:digit:]]` matches digits `0-9`
* `[[:lower:]]` matches lower case alphabetic characters
* `[[:upper:]]` matches upper case alphabetic characters
* `[[:space:]]` matches whitespace.


## Case Statements

Alternative to if statement and is easier to read when the same kind of comparison is made multiple times.

Syntax:
```
case "$VAR" in 
    pattern_1)
        # Commands
        ;;
    pattern_N)
        ;;
esac
```


## Logging

Linux uses `syslog` by default. Facilities include `kern`, `user`, `mail`, `daemon`, `auth`, `local0`, `local7`. Severities include `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`. Log file locations are configurable.

The `logger` command generates `syslog` messages. By default it creates user notice messages.

Flags:
* `-p` sets severity.
* `-t` tags the message, usually with the name of the script. 
* `-i` includes `pid` in the log message.


## While Loops

Syntax:
```
while [CONDITION]
do
    # command
done
```

An infinite loop must be canceled with `Ctrl+C` or the `kill` command.

The command `read -p` can be used to integrate interactive prompts into the loop. If the response to the prompt does not match an expected message, then the loop restarts. Use the `break` command to exit a loop before the default end. Use `continue` to restart the loop at the next iteration before the loop completes.


## Debugging

A bug is an error. Debugging determines the root of unexpected behavior.

Built-in debugging includes the x-trace `-x` which prints commands as they execute, including substitutions and expansions. Add it to the end of a shebang or run `set -x` then `set +x` to stop debugging.

`-e` can be used to exit on error and can be combined with other options.

`-v` prints shell input lines as they are read, without substitutions or expansions.

Manual debugging can be created with variables like `DEBUG`.

`PS4` can be used to control what displays before a line when using the `-x` option. The default is just `+`.


## Sed

Stream editor. A stream is data that travels from process to another, one file to another, or one device to another. Sed performs text transformations on streams. Sed is used programmatically, not interactively. Sed is most commonly used as a find and replace utility.

Syntax: `sed 's/[original text]/[replacement text]' [target file]`

To replace the nth occurrence of a pattern use `sed 's/[original]/[replacement]/n [target file]`.

Other syntax options include:
* `/g` is a global replace instead of just the first match.
* `-i.[txt]` inserts the appended text to the end of the filename.
* `/w [filename]` writes to a new file with the specified name.
* `s#` instead of `s/` changes the delimiter character to `#`, any character can be used as a delimiter.
* `/d` deletes a line.
* `sed '/^$/d` deletes any line starting with `$`. Can be used for various pattern matching.
* `;` can be used to separate multiple commands within one `sed` command.
* `sed '1 s/[origin]/[replacement]` only executes the command against line number `n` with the address of `n`.

It is very common to pipe a `cat` command into `sed`. Addresses can also be based on pattern matching `/[text]/ s/...` or have multiple addresses specifications with a `,`.