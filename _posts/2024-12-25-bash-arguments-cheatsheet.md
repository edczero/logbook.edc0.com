---
title: Bash Arguments Cheatsheet
author: edc0
#date: 2024-10-30 14:10:00 +05:30
media_subpath: /logbook/images/
categories: [Bash, Snippets]
tags: [bash, cheatsheet, command-line, arguments]
render_with_liquid: false
---

While `getopt` can be used to access positional and optional arguments, Non ecchanted version of getopt has some limitation

- can't handle arguments that are empty strings
- can't handle arguments with embedded whitespace
- Bash builtin getopts. This does not support long option names with the double-dash prefix. It only supports single-character options.
- BSD UNIX implementation of standalone getopt command (which is what MacOS uses). This does not support long options either.

More recent getopt versions (OR **enhanced getopt from util-linux or formerly GNU glibc**) don't have these limitations. On OS X try `brew install gnu-getopt`,  `brew install util-linux`

However arguments can be parsed without using getopt or getopts (notice the `s`).


### Space-Separated (e.g., --option argument)

```sh
# Reference: https://stackoverflow.com/questions/192249/
#!/bin/bash

POSITIONAL_ARGS=()

while [[ $# -gt 0 ]]; do
  case $1 in
    -e|--extension)
      EXTENSION="$2"
      shift # past argument
      shift # past value
      ;;
    -s|--searchpath)
      SEARCHPATH="$2"
      shift # past argument
      shift # past value
      ;;
    --default)
      DEFAULT=YES
      shift # past argument
      ;;
    -*|--*)
      echo "Unknown option $1"
      exit 1
      ;;
    *)
      POSITIONAL_ARGS+=("$1") # save positional arg
      shift # past argument
      ;;
  esac
done

set -- "${POSITIONAL_ARGS[@]}" # restore positional parameters

echo "FILE EXTENSION  = ${EXTENSION}"
echo "SEARCH PATH     = ${SEARCHPATH}"
echo "DEFAULT         = ${DEFAULT}"
echo "Number files in SEARCH PATH with EXTENSION:" $(ls -1 "${SEARCHPATH}"/*."${EXTENSION}" | wc -l)

if [[ -n $1 ]]; then
    echo "Last line of file specified as non-opt/last argument:"
    tail -1 "$1"
fi
```

Then execute as
```sh
chmod +x /tmp/demo-space-separated.sh

/tmp/demo-space-separated.sh -e conf -s /etc /etc/hosts
```