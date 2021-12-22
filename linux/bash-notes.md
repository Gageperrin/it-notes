# Bash Scripting

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

`$?` contains the return code of the previous executed command.

