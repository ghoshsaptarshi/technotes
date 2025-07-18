---
title: Bash vs ZSH
description: 
permalink: 
aliases: 
tags: 
draft: true
date: 2025-07-05
cssclasses:
---
# sh vs zsh

First, some important things:

1. **[[bash]] isn't going away**. If you're already using bash, nothing will change for you. All that changes is that zsh will be the default login shell for _new_ accounts, and even then, you can select bash instead.
2. **Scripts are not affected**. What changes is the shell for interactive use, i.e. the shell in terminals (and also a few other things that use the login shell, such as crontabs). If you have a script in a file with execution permissions, starting with a [shebang line](https://en.wikipedia.org/wiki/Shebang_(Unix)) such as `#!/bin/bash` or `#!/bin/sh` or `#!/usr/bin/env bash`, it'll keep working exactly as before.
3. Zsh's syntax is not completely compatible with bash, but it's close. A lot of code will keep working, for example typical aliases and functions. The main differences are in interactive configuration features.

Now, assuming you're considering switching to zsh, which has been a possibility for years, here are the main differences you'll encounter. This is not an exhaustive list!

### Main differences for interactive use

**Configuration files**: [bash reads](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html#Bash-Startup-Files) (mainly) `.bashrc` in non-login interactive shells (but macOS starts a login shell in terminals by default), `.profile` or `.bash_profile` in login shells, and `.inputrc`. [Zsh reads](http://zsh.sourceforge.net/Doc/Release/Files.html) (mainly) `.zshrc` (in all interactive shells) and `.zprofile` (in login shells). This means that none of your bash customizations will apply: you'll need to port them over. You can't just copy the files because many things will need tweaking.

**Key bindings** use completely different syntax. Bash uses [`.inputrc` and the `bind` builtin](https://www.gnu.org/software/bash/manual/html_node/Readline-Init-File.html) to bind keys to [readline commands](https://www.gnu.org/software/bash/manual/html_node/Bindable-Readline-Commands.html). Zsh uses [the `bindkey` builtin](http://zsh.sourceforge.net/Doc/Release/Zsh-Line-Editor.html#index-bindkey) to bind keys to [zle widgets](http://zsh.sourceforge.net/Doc/Release/Zsh-Line-Editor.html#Standard-Widgets). Most readline commands have a zsh equivalent, but it isn't always a perfect equivalence.

Speaking of key bindings, if you use Vi(m) as your in-terminal editor but not as your command line mode in the shell, you'll notice zsh defaults to vi editing mode (i.e. with command and insert modes) if `EDITOR` or `VISUAL` is set to `vi` or `vim`. `bindkey -e` switches to emacs mode (i.e. where you can always type directly).

**Prompt**: bash sets the prompt (mainly) from [`PS1`](https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Variables.html#index-PS1) which contains [backslash escapes](https://www.gnu.org/software/bash/manual/html_node/Controlling-the-Prompt.html#Controlling-the-Prompt). Zsh sets the prompt mainly from [`PS1`](http://zsh.sourceforge.net/Doc/Release/Parameters.html#index-PS1) which contains [percent escapes](http://zsh.sourceforge.net/Doc/Release/Prompt-Expansion.html#Prompt-Expansion). Although the concepts are similar, the escape codes are completely different. The functionality of bash's [`PROMPT_COMMAND`](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-PROMPT_005fCOMMAND) is available in zsh via the [`precmd`](http://zsh.sourceforge.net/Doc/Release/Functions.html#index-precmd) and [`preexec`](http://zsh.sourceforge.net/Doc/Release/Functions.html#index-preexec) [hook functions](http://zsh.sourceforge.net/Doc/Release/Functions.html#Hook-Functions). Zsh has more convenience mechanisms to build fancy prompts including a [prompt theme mechanism](http://zsh.sourceforge.net/Doc/Release/User-Contributions.html#Prompt-Themes).

The basic **command line history** mechanisms (navigation with Up/Down, search with Ctrl+R, history expansion with `!!` and friends, last argument recall with Alt+. or `$_`) work in the same way, but there are a lot of differences in the details, too many to list here. You can copy your `.bash_history` to `.zsh_history` if you haven't changed a shell option that changes the file format.

**Completion**: both shells default to a basic completion mode that mostly completes command and file names, and switch to a fancy mode by including `bash_completion` on bash or by running `compinit` in zsh. You'll find some commands that bash handles better and some that zsh handles better. Zsh is usually more precise, but sometimes gives up where bash does something that isn't correct but is sensible. To specify possible completions for a command, zsh has three mechanisms:

- The [“old” completion mechanism with `compctl`](http://zsh.sourceforge.net/Doc/Release/Completion-Using-compctl.html#Completion-Using-compctl) which you can forget about.
- The [“new” completion mechanism with `compadd`](http://zsh.sourceforge.net/Doc/Release/Completion-System.html#Completion-System) and [lots of functions that begin with underscore](http://zsh.sourceforge.net/Doc/Release/Completion-Widgets.html#Completion-Widgets) and a [powerful but complex user configuration mechanism](http://zsh.sourceforge.net/Doc/Release/Completion-Widgets.html#Completion-Widgets).
- An [emulation to support bash completion functions](http://zsh.sourceforge.net/Doc/Release/Completion-System.html#index-bashcompinit) which you can enable by running `bashcompinit`. The emulation isn't 100% perfect but it usually works.

Many of bash's [**`shopt`** settings](https://www.gnu.org/software/bash/manual/html_node/The-Shopt-Builtin.html#The-Shopt-Builtin) have a corresponding [`setopt`](http://zsh.sourceforge.net/Doc/Release/Options.html#Options) in zsh.

Zsh doesn't treat **`#`** as a comment start on the command line by default, only in scripts (including `.zshrc` and such). To enable interactive comments, run [`setopt interactive_comments`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-INTERACTIVE_005fCOMMENTS).

### Main differences for scripting

(and for power users on the command line of course)

In bash, `$foo` takes the value of `foo`, splits it at whitespace characters, and for each whitespace-separated part, if it contains wildcard characters and matches an existing file, replaces the pattern by the list of matches. To just get the value of `foo`, you need `"$foo"`. The same applies to command substitution `$(foo)`. In zsh, **`$foo`** is the value of `foo` and **`$(foo)`** is the output of `foo` minus its final newlines, with two exceptions. If a word becomes empty due to expanding empty unquoted variables, it's removed (e.g. `a=; b=; printf "%s\n" one "$a$b" three $a$b five` prints `one`, an empty line, `three`, `five`). The result of an unquoted command substitution is split at whitespace but the pieces don't undergo wildcard matching.

Bash **arrays** are indexed from 0 to (length-1). Zsh arrays are indexed from 1 to length. You can make 0-indexing the default with [`setopt ksh_arrays`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-KSH_005fARRAYS). Zsh requires fewer braces (unless `ksh_arrays` is enabled). For example, suppose `a=(first second third "" last)`.

| Functionality              | Bash syntax        | Idiomatic zsh syntax       | Expansion                                    |
| -------------------------- | ------------------ | -------------------------- | -------------------------------------------- |
| First element              | `${a[0]}`          | `$a[1]`                    | `first`                                      |
| Second element             | `${a[1]}`          | `$a[2]`                    | `second`                                     |
| Last element               | `${a[${#a[@]}-1]}` | `$a[-1]`                   | `last`                                       |
| Length                     | `${#a[@]}`         | `$#a`                      | `5`                                          |
| All the elements           | `"${a[@]}"`        | `"${a[@]}"` or `"${(@)a}"` | `first` `second` `third` (empty word) `last` |
| All the non-empty elements |                    | `$a`                       | `first` `second` `third` `last`              |

Bash has extra **[wildcard patterns](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html#Pattern-Matching)** such as `@(foo|bar)` to match `foo` or `bar`, which are only enabled with `shopt -s extglob`. In zsh, you can enable [these patterns](http://zsh.sourceforge.net/Doc/Release/Expansion.html#ksh_002dlike-Glob-Operators) with `setopt ksh_glob`, but there's also a simpler-to-type [native syntax](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Operators) such as `(foo|bar)`, some of which requires `setopt extended_glob` (do put that in your `.zshrc`, and it's on by default in completion functions). Zsh has `**/` for recursive directory traversal (as does modern bash but not the bash 3.2 that ships with macOS).

In bash, by default, if a **wildcard pattern doesn't match** any file, it's left unchanged. In zsh, by default, you'll get an error, which is usually the safest setting. If you want to pass a wildcard parameter to a command, use quotes. You can switch to the bash behavior with [`setopt no_nomatch`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-NOMATCH). You can make non-matching wildcard patterns expand to an empty list instead with [`setopt null_glob`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-NULL_005fGLOB).

In bash, the **right-hand side of a pipeline** runs in a subshell. In zsh, it runs in the parent shell, so you can write things like `somecommand | read output`.

### Some nice zsh features

Here are a few nice zsh features that bash doesn't have (at least not without some serious elbow grease). Once again, this is just a selection of the ones I consider the most useful.

**[Glob qualifiers](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Qualifiers)** allow matching files based on metadata such as their time stamp, their size, etc. They also allow tweaking the output. The syntax is rather cryptic, but it's extremely convenient. Here are a few examples:

- `foo*(.)`: only regular files matching `foo*` and symbolic links to regular files, not directories and other special files.
- `foo*(*.)`: only executable regular files matching `foo*`.
- `foo*(-.)`: only regular files matching `foo*`, not symbolic links and other special files.
- `foo*(-@)`: only dangling symbolic links matching `foo*`.
- `foo*(om)`: the files matching `foo*`, sorted by last modification date, most recent first. Note that if you pass this to `ls`, it'll do its own sorting. This is especially useful in…
- `foo*(om[1,10])`: the 10 most recent files matching `foo*`, most recent first.
- `foo*(Lm+1)`: files matching `foo*` whose size is at least 1MB.
- `foo*(N)`: same as `foo*`, but if this doesn't match any file, produce an empty list regardless of the setting of the `null_glob` option (see above).
- `*(D)`: match all files including dot files (except `.` and `..`).
- `foo/bar/*(:t)` (using a [history modifier](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Modifiers)): the files in `foo/bar`, but with only the base name of the file. E.g. if there is a `foo/bar/qux.txt`, it's expanded as `qux.txt`.
- `foo/bar/*(.:r)`: take regular files under `foo/bar` and remove the extension. E.g. `foo/bar/qux.txt` is expanded as `foo/bar/qux`.
- `foo*.odt(e\''REPLY=$REPLY:r.pdf'\')`: take the list of files matching `foo*.odt`, and replace `.odt` by `.pdf` (regardless of whether the PDF file exists).

Here are a few useful zsh-specific **[wildcard patterns](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Operators)**.

- `foo*.txt~foobar*`: all `.txt` files whose name starts with `foo` but not `foobar`.
- `image<->.jpg(n)`: all `.jpg` files whose base name is `image` followed by a number, e.g. `image3.jpg` and `image22.jpg` but not `image-backup.jpg`. The glob qualifier `(n)` causes the files to be listed in numerical order, i.e. `image9.jpg` comes before `image10.jpg` (you can make this the default even without `-n` with [`setopt numeric_glob_sort`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-NUMERIC_005fGLOB_005fSORT)).

To **mass-rename files**, zsh provides a very convenient tool: the [`zmv` function](http://zsh.sourceforge.net/Doc/Release/User-Contributions.html#index-zmv). Suggested for your `.zshrc`:

```bash
autoload zmv
alias zcp='zmv -C' zln='zmv -L'
```

Example:

```dart
zmv '(*).jpeg' '$1.jpg'
zmv '(*)-backup.(*)' 'backups/$1.$2'
```

Bash has a few [ways to apply transformations when taking the value of a variable](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html#Shell-Parameter-Expansion). Zsh has [some of the same and many more](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Parameter-Expansion).

Zsh has a number of little convenient features to **change directories**. Turn on [`setopt auto_cd`](http://zsh.sourceforge.net/Doc/Release/Options.html#index-AUTO_005fCD) to change to a directory when you type its name without having to type `cd` (bash also has this nowadays). You can use the [two-argument form to `cd`](http://zsh.sourceforge.net/Doc/Release/Shell-Builtin-Commands.html#index-cd) to change to a directory whose name is close to the current directory. For example, if you're in `/some/where/foo-old/deeply/nested/inside` and you want to go to `/some/where/foo-new/deeply/nested/inside`, just type `cd old new`.

To assign a value to a variable, you of course write `VARIABLE=VALUE`. To **edit the value of a variable** interactively, just run [`vared VARIABLE`](http://zsh.sourceforge.net/Doc/Release/Zsh-Line-Editor.html#index-vared).

### Final words

Zsh comes with a configuration interface that supports a few of the most common settings, including canned recipes for things like case-insensitive completion. To (re)run this interface (the first line is not needed if you're using a configuration file that was edited by `zsh-newuser-install`):

```zsh
autoload -U zsh-newuser-install
zsh-newuser-install
```

Out of the box, with no configuration file at all, many of zsh's useful features are disabled for backward compatibility with 1990's versions. `zsh-newuser-install` suggests some recommended features to turn on.

There are many zsh configuration frameworks on the web (many of them are [on Github](https://github.com/topics/zsh-configuration)). They can be a convenient way to get started with some powerful features. The flip side of the coin is they often lock you in doing things the way the author does, so sometimes they'll prevent you from doing things the way you want. Use them at your own risk.

The zsh manual has a lot of information, but it's often written in a way that's terse and hard to follow, and has few examples. Don't hesitate to search for explanations and examples online: if you only use the part of zsh that's easy to understand in the manual, you'll miss out. Two good resources are [the zsh-users mailing list](https://www.zsh.org/cgi-bin/mla/wilma/users) and [Unix Stack Exchange](https://unix.stackexchange.com/questions/tagged/zsh). An [extensive collection of articles on switching to zsh on the mac](https://scriptingosx.com/2019/06/moving-to-zsh/) can be found on scriptingosx.com and [a useful Ruby script to bring your command history with you](https://gist.github.com/goyalankit/a1c88bfc69107f93cda1), can be found on Github.