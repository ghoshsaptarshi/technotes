---
title: Introduction to bash
description: 
permalink: 
aliases: 
tags: 
draft: true
date: 2025-06-30
cssclasses:
---
> bash is the GNU Project's shell. Bash stands for **B**ourne **A**gain **SH**ell. It is an sh-compatible shell that incorporates useful features from the Korn shell (ksh) and C shell (csh). It is intended to conform to the IEEE POSIX P1003.2/ISO 9945.2 Shell and Tools standard. It offers functional improvements over sh for both programming and interactive use. In addition, most sh scripts can be run by Bash without modification.

The improvements offered by bash include:
- Command line editing
- Unlimited size command history
- Job Control
- [[Glossary_S#Shell|#Shell]] Functions and Aliases
- Indexed [[Glossary_A#Array|#Array]] of unlimited size
- [[Glossary_I#Integer|Integer]] arithmetic in any base from 2 to 64

[Official Homepage](https://www.gnu.org/software/bash/)

Since, bash is part of GNU's ecosystem, it is used in-conjunction with the tools/utilities of [[linux/coreutils/index]] package.

>[!info]
>Anything that we can run normally on the command line can be put into a [[Glossary_S#Script|script]] and it will do exactly the same thing. Similarly, anything we put into a script can also be run normally on the command line and it will do exactly the same thing.

#### Sample Bash Script
```bash
#!/bin/bash
# Hello World Bash script
echo "Hello World"
echo $PATH
```
##### Output
```bash
user@host$ chmod +x script.sh
user@host$ ./script.sh
Hello World
user@host$
```

## Parts
A typical bash script starts with a shebang symbol along with the interpreter module path like below -
```bash
#!/bin/bash
```

![[Glossary - S#Shebang]]

> [!warning]
The shebang must be on the very first line of the file before any of the commands is written. There must also be no spaces before the # or between the ! and the path to the interpreter

After the shebang line, rest of the file contains the executable statements for the shell like below -

```bash
echo "Hello World"
echo $PATH
```

These statements can be comprised of commands from [[linux/coreutils/index]] package, installed programs or [[scripting/bash/Functions#Functions|Bash Functions]]. Sometimes, installed can be comments also like -

```bash
# Hello World Bash script
```

Comments are identified by a `#` symbol at the start of the line.

> [!tip]
> It is a good practice to put comments in the script explaining what it does. It helps later during any kind of updates or juts for understanding.

In order to execute the script, we need to make it executable by issuing `chmod +x scriptName.sh` from the cmd line. Since, the script is most likely not in any of the folders defined in `$PATH` , we need to put `./` in front of the script to make Bash understand that it's an absolute path.

> [!note] Formatting Bash Script
> - Formatting a bash script is important
> - Spaces (or a lack of it thereof) are important

## Working of Bash Script
This is just a little bit of background knowledge. It's not necessary to understand this in order to write scripts but it can be useful to know before getting into more complex scripts (and scripts that call and rely on other scripts).

In the realm of linux (and computers in general) we have the concept of programs and processes. A program is a blob of binary data consisting of a series of instructions for the CPU and possibly other resources (images, sound files and such) organised into a package and typically stored on your hard disk. When we say we are running a program we are not really running the program but a copy of it which is called a process. What we do is copy those instructions and resources from the hard disk into working memory (or RAM). We also allocate a bit of space in RAM for the process to store variables (to hold temporary working data) and a few flags to allow the operating system (OS) to manage and track the process during it's execution.

**Essentially a process is a running instance of a program.**

There could be several processes representing the same program running in memory at the same time. For example I could have two terminals open and be running the command [[cp]] in both of them. In this case there would be two cp processes currently existing on the system. Once they are finished running the system then destroys them and there are no longer any processes representing the program cp.

At the terminal we have a bash process running in order to give us the Bash shell. If we start a script, it doesn't actually run in that process but instead starts a new process to run inside. This impacts the variables.


## [[00 - Introduction to Bash|Introduction to Bash]]

## [[01 - Variables in Bash|Variables in Bash]]

## [[02 - Input|Input]]

## [[03 - Arithmetic|Arithmetic]]

## [[04 - Decision Control|Decision Control]]

## [[05 - Loops|Loops]]

## [[06 - Functions|Functions]]

## [[07 - User Interface|User Interface]]

## [[08 - Additional Tips|Some additional Tips]]