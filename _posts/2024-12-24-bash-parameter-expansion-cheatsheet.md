---
title: Bash Parameter Expansion Cheatsheet
author: edc0
#date: 2024-10-30 14:10:00 +05:30
media_subpath: /logbook/images/
categories: [Bash, Snippets]
tags: [bash, cheatsheet, command-line, parameter-expansion]
render_with_liquid: false
---


Bash scripting is a powerful tool for automating tasks on Unix-like systems. One of the most valuable but sometimes overlooked features in bash is preter expansion. preter expansion allows you to manipulate the values of variables in various ways, making your scripts more dynamic and flexible. Here, we’ll explore some “cheat codes” or shortcuts that you can use while working with bash preter expansion to enhance your scripting skills.

### Default Values

Suppose you want to ensure that a variable always has a value, even if none is provided. You can use preter expansion to specify a default value:

```bash
echo ${variable:-"Default Value"}
```

### Set Up the Test Directory

```sh
mkdir test
cd test
touch file1 file2 file3
```

### Basic parameter Expansion

```md
$ pr1=hello
$ echo $pr1		hello			
$ echo ${pr1}a	helloa		the braces separate the name
$ echo ${pr2}					Nothing here
```


### Default Values

```md
$ echo ${pr2:-file*}	file1 file2 file3	all files in the dir
$ echo ${pr2:-$pr1} 	hello			uses $pr1's value...
$ echo $pr2		(nothing)		…but didn't change $pr2
$ echo ${pr3:=$pr1} 	hello			uses $pr1's value...
$ echo $pr3		hello			... and assigns it to $pr3
```

### Substring Manipulations

Keep in mind that the substitutions etc are expanded, not literals, so you can use wildcards and other pattern syntaxes in them (for example, the “he*l” below used to strip “hell” from the value).

```md
$ echo ${pr1:2}		llo	substring from 2
$ echo ${pr1:2:2}	ll	substring from 2, len 2
$ echo ${pr1#he}	llo	strip shortest match from start
$ echo ${pr1#hel*}	lo	strip shortest match from start
$ echo ${pr1#he*l}	lo	strip shortest match from start
$ echo ${pr1##he*l}	o	strip longest match from start
$ echo ${pr1%l*o}	hel	strip shortest match from end
$ echo ${pr1%%l*o}	he	strip longest match from end
$ echo ${pr1/l/p}	helpo	replace as few as possible
$ echo ${pr1//l/p}	heppo	replace as many as possible
```

### Miscellaneous

```md
$ echo ${!pr*}	pr1 pr2 pr3	parameter names starting with...
$ echo ${#pr1}	5		length of parameter value
```


### Example Uses

```sh
file="/path/to/example.txt"

# Extract filename
filename="${file##*/}"

# Extract extension
extension="${file##*.}"

# Extract basename (filename without extension)
basename="${filename%.*}"

echo "Full file path: $file"
echo "File name: $filename"
echo "File extension: $extension"
echo "Base name: $basename"
```

```yml
Full file path: /path/to/example.txt
File name: example.txt
File extension: txt
Base name: example
```