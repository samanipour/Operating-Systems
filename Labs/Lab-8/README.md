# Lab-8: Introduction to Shell Scripts

---

## 1. Introduction

### 1.1 Overview
The primary benefit of BASH scripting is that you can use everything that is available in the
BASH shell within a BASH script. Because your Linux distribution has hundreds (maybe even
thousands) of commands, each of which can be used in a BASH script, BASH shell scripting can
be a very powerful tool.

### 1.2 Objectives
- Learn the basics of writing and running shell scripts.
- Understand quoting, variables, conditionals, loops, and subshells.
- Master common shell utilities like `basename`, `awk`, and `sed`.

---

## 2. Basics of BASH Scripting
To start a BASH script, enter the following as the first line of the script file:

`#!/bin/bash`

This special sequence is called the sha-bang, and it tells the system to execute this code as a
BASH script.

Comments in a BASH script start with a # character and extend to the end of the line.
For example:

```bash
echo "hello world" #prints "hello" to the screen
```

As shown in the preceding example, you can use the echo command to display information to
the user who is running a program. The arguments to the echo command can contain any text
data and can also include the value of a variable:

```bash
echo "The answer is $result"
```

After creating your BASH script and saving it, you then make it executable:
```bash
[student@OCS ~]$ more hello.sh
#!/bin/bash
#hello.sh
echo "hello world!"
```

```bash
[student@OCS ~]$ chmod a+x hello.sh
```

Now it can be run as a program by using the following syntax:
```bash
[student@OCS ~]$ ./hello.sh
hello world!
```

Note the need to place ./ before the name of the command. This is because the command
might not be in one of the directories specified by the $PATH variable:

```bash
[student@OCS ~]$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/
games
```

To avoid the need to include ./ whenever you want to run your script, you can modify the
$PATH variable to include the directory in which your script is stored. Typically, regular users
create a “bin” directory in their home directory and place scripts in this location:

```bash
[student@OCS ~]$ mkdir bin
[student@OCS ~]$ cp hello.sh bin
[student@OCS ~]$ PATH="$PATH:/home/student/bin"
[student@OCS ~]$ hello.sh
hello world!
```


In addition to the built-in variables that were discussed , variables are available in
BASH scripts that represent the arguments being passed into the script. For example, consider
the following execution of a script called test.sh:
```bash
[student@OCS ~]$ test.sh Bob Sue Ted
```

The values “Bob,” “Sue,” and “Ted” are assigned to variables within the script. The first argument
(“Bob”) is assigned to the $1 variable, the second argument is assigned to the $2 variable,
and so on. Additionally, all arguments collectively are assigned to the $@ variable.
For additional details regarding these positional parameters variables, or anything related the
BASH scripting, consult the man page for bash:

```bash
[student@OCS ~]$ man bash
```

# Conditional Expressions

Several conditional statements are available for the BASH shell, including the if statement:
```bash
if [ cond ]
then
statements
elif [ cond ]
then
statement
else
statements
fi
```

Note the following:
- An else if is spelled elif and is optional.
- After the if and elif, you need a then statement. However, after an else, do not
include a then statement.
- End the if statement with the word if spelled backwards: fi


### Sample if statement

```bash
#!/bin/bash
#if.sh
color=$1
if [ "$color" = "blue" ]
then
echo "it is blue"
elif [ "$color" = "red" ]
then
echo "it is red"
else
echo "no idea what this color is"
fi
```

## Quoting variables
Get in the habit of putting double quotes around your variables in BASH scripts. This is
important in the event the variable hasn’t been assigned a value. For example, suppose the
script in the example was execute with no arguments. The result would be that the color variable
is unassigned and the resulting conditional statement would be if [ "" = "blue" ]
The result would be false, but without the quotes around $color, the result would be an
error message and the script would exit immediately. This is because the resulting conditional
statement would be missing one of its key components after the value of $color has been
returned: if [ = "blue" ]

This syntax performs an implicit call of a BASH command named test that you can use to
perform several comparison tests. This can include integer (numeric) comparisons, string comparisons,
and file testing operations. For example, use the following syntax to test whether the string
value that is stored in the $name1 variable does not equal the string stored in the $name2 variable:
```bash
[ "$name1" != "$name2" ]
```

### Important Note
The spacing around the square brackets is very important. There should be a space before and
after each square bracket. Without these, an error message occurs.

In addition to determining whether two strings are equal or not equal to each other, you might
also find the -n option useful.This option determines whether a string is not empty, which is
useful when testing user input.

### Testing user input

```bash
[student@OCS ~]$ more name.sh
#!/bin/bash
#name.sh
echo "Enter your name"
read name
if [ -n "$name" ]
then
echo "Thank you!"
else
echo "hey, you didn't give a name!"
fi
[student@OCS ~]$./name.sh
Enter your name

Bo
Thank you!
[student@OCS ~]$./name.sh
Enter your name
hey, you didn't give a name!
```
### Integer Comparisons

If you want to perform integer (numeric) comparison operations, use the following:
- -eq True if values are equal to each other.
- -ne True if values are not equal to each other.
- -gt True if first value is greater than second value.
- -lt True if first value is less than second value.
- -ge True if first value is greater than or equal to second value.
- -le True if first value is less than or equal to second value.

### File Test Comparisons

You can also perform test operations on files and directories to determine information about
their status. These operations include:

- -d True if “file” is a directory.
- -f True if “file” is a regular file.
- -r True if “file” exists and is readable by the user running the script.
- -w True if “file” exists and is writable by the user running the script.
- -x True if “file” exists and is executable by the user running the script.
- -L True if “first” value is less than or equal to second value.

# Flow Control Statements
In addition to if statements, the BASH scripting language has several other flow
control statements:

The while loop—Executes a block of code repeatedly as long as the conditional
statement is true.
- The until loop—Executes a block of code repeatedly as long as the conditional
statement is false. Essentially the opposite of a while loop.
- The case statement—Similar to an if statement but provides an easier branching
method for multiple situations.
- The for loop—Executes a block of code for each item of a list of values.

## The while loop
The following code segment prompts the user for a five-digit number. If the user complies, the
program will continue because the condition of the while loop will be false. However, if the
user provides incorrect data, the condition of the while loop will be true and the user will be
prompted for the correct data again:

```bash
echo "Enter a five-digit ZIP code: "
read ZIP
while echo $ZIP | egrep -v "^[0-9]{5}$" > /dev/null 2>&1
do
echo "You must enter a valid ZIP code – five digits only!"
echo "Enter a five-digit ZIP code: "
read ZIP
done
echo "Thank you"
```
The `egrep` command from the previous example may be a bit tricky to understand. To begin
with, the regular expression pattern is matching a value that is exactly five digits. The –v
option is used to return a value if the pattern is not found. So, if $ZIP contains a valid
five-digit number, then egrep returns a false result because it is trying to find lines that don’t
contain a five-digit number. The egrep command returns a true result if the $ZIP contains
something besides a five-digit number.

Why the > /dev/null 2>&1? Because we don’t want to display anything from the egrep
command, just make use of its true/false return value. All OS commands return true or false3
when executed, and that is what is needed here. Any STDOUT or STDERR from the command is
unnecessary and only serves to confuse matters if it is displayed to the user.

## The for Loop

A for loop enables you to perform an operation on a set of items. For example, the following
command, when run as the root user, creates five user accounts:
```bash
for person in bob ted sue nick fred
do
useradd $person
done
```

### Loop Control
Like most languages, BASH scripting provides a way to prematurely exit a loop or to stop the
current iteration of a loop and start a new iteration of a loop. Use the break command to
immediately exit a while, until, or for loop. Use the continue command to stop the current
iteration of a while, until, or for loop and start the next iteration.

# The case Statement
A case statement is designed for when you want to perform multiple conditional checks.
Although you could use an if statement with multiple elif statements, the syntax of
if/elif/else is often more cumbersome than a case statement.
```bash
The syntax for a case statement is:
case var in
cond1) cmd
cmd;;
cond2) cmd
cmd;;
esac
```

For the preceding syntax example, var represents a variable’s value that you want to
conditionally check. For example, consider the following code:
```bash
name="bob"
case $name in
ted) echo "it is ted";;
bob) echo "it is bob";;
*) echo "I have no idea who you are"
esac
```

The “condition” is a pattern that uses the same matching rules as file wildcards. An * matches
zero or more of any character, a ? matches a single character, and you can use square brackets
to match a single character of a specific range. You can also use a |character to represent “or.”
For example:

```bash
answer=yes
case $answer in
y|ye[sp]) echo "you said yes";;
n|no|nope) echo "you said no";;
*) echo "bad response";;
esac
```

# User Interaction
Using actual user input, which the read statement can gather, would make more sense:

```bash
read answer
```
The read statement prompts the user to provide information and reads that data (technically
from STDIN) into a variable that is specified as the argument to the read statement. 

```bash
read answer
case $answer in
y|ye[sp]) echo "you said yes";;
n|no|nope) echo "you said no";;
*) echo "bad response";;
esac
```
