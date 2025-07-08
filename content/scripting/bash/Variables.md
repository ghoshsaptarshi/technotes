---
title: Bash Variables
description: 
permalink: 
aliases: 
tags: 
draft: true
date: 
cssclasses:
---
Like all other scripting languages, bash allows the use of [[Glossary - V#Variable|variables]] in scripts. Variables may have their value set in a few different ways. The most common are to set the value directly and for its value to be set as the result of processing by a command or program. To read the variable we place its name, preceded by a $ sign, anywhere in the script we would like.
Before Bash interprets (or runs) every line of our script it first checks to see if any variable names are present. For every variable it has identified, it replaces the variable name with its value. Then it runs that line of code and begins the process again on the next line.

## Syntax
- When referring to or reading a variable we place a $ sign before the variable name
- When setting a variable we leave out the $ sign
- Variable names can be all uppercase, all lowercase, or a mixture
- A variable may be placed anywhere in a script (or on the command line for that matter) and, when run, Bash will replace it with the value of the variable. This is made possible as the substitution is done before the command is run.

## Command Line Arguments
When we run a program on the command line, we often run with some arguments. For example - `ls -l /etc`. 
Here `-l` and `/etc` are both command line arguments to the command `ls`. We can do similar with our bash scripts. For example -

#### Sample Script - CLI Arguments
```bash
#!/bin/bash
echo "Hello " $1
```

#### Execution - CLI Arguments
```sh
user@host$ ./script.sh Saptarshi
Hello Saptarshi
user@host$
```

To do this we use the variables `$1` to represent the first command line argument, `$2` to represent the second command line argument and so on. These are automatically set by the system when we run our script so all we need to do is refer to them.

## Built-in System Variables
Some of the system defined variables in Bash -

| Variable    | Description                                               |
| ----------- | --------------------------------------------------------- |
| `$0`        | The name of the Bash script                               |
| `$1` - `$9` | The first 9 arguments to the Bash script                  |
| `$#`        | Count of arguments were passed to the Bash script         |
| `$@`        | All the arguments supplied to the Bash script             |
| `$?`        | Exit status of the most recently run process              |
| `$$`        | Process ID of the current script                          |
| `$USER`     | username of the user running the script                   |
| `$HOSTNAME` | hostname of the machine the script is running             |
| `$SECONDS`  | Number of seconds since the script was started            |
| `$RANDOM`   | Returns a different random number each time it's referred |
| `$LINENO`   | Returns the current line number in the Bash script        |


## User Defined Variables
User defined variables can be set as `variable=value` in a bash script.
- Formatting is important
	- There is no space on either side of the equals ( = ) sign
	- There's no `$` sign from the beginning of the variable name when setting it
- Variable names may be uppercase or lowercase or a mixture of both.

>[!important]
**Bash is case sensitive**

- Under normal circumstances Bash uses a space to determine separate items.
- When we want variables to store more complex values, we need to make use of quotes.
- When we enclose our content in quotes (either single or double) we are indicating to Bash that the contents should be considered as a single item
	-   Single quotes will treat every character literally
	-   Double quotes will allow you to do substitution (that is include variables within the setting of the value)


Example -
```sh
user@bash$ myvar='Hello World'
user@bash$ echo $myvar
Hello World
user@bash$ newvar="More $myvar"
user@bash$ echo $newvar
More Hello World
user@bash$ newvar='More $myvar'
user@bash$ echo $newvar
More $myvar
```

## Command Substitution
Command substitution allows us to take the output of a command or program (what would normally be printed to the screen) and save it as the value of a variable.
To do this we place the command within brackets, preceded by a $ sign.

Example #1 -
```sh
user@bash$ myvar=$(ls)
user@bash$ echo $myvar
Desktop Documents Downloads Dropbox
```

Example #2 -
```sh
user@bash$ myvar=$( ls /etc | wc -l )
user@bash$ echo $myvar
196
user@bash$ echo There are $myvar entries in /etc
There are 196 entries in /etc
```

## Exporting Variables
See [[00 - Introduction to Bash#Working of Bash Script|working or Bash script]]

Code for ```script1.sh``` 

```sh
#!/bin/bash
# demonstrate variable scope 1.
var1=blah
var2=foo

# Let's verify their current value
echo $0 :: var1 : $var1, var2 : $var2

export var1
./script2.sh

# Let's see what they are now
echo $0 :: var1 : $var1, var2 : $var2
```

Code for ``script2.sh``

```sh
#!/bin/bash
# demonstrate variable scope 2
# Let's verify their current value
echo $0 :: var1 : $var1, var2 : $var2

# Let's change their values
var1=flop
var2=bleh
```

Execution -
```sh
user@bash$ ./script1.sh
script1.sh :: var1 : blah, var2 : foo
script2.sh :: var1 : blah, var2 :
script1.sh :: var1 : blah, var2 : foo
```