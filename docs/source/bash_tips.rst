.. highlight:: bash

*********
Bash tips
*********

I use bash very often, as I'm automating a lot. But if you combine existing
Linux commands, it's very obvious to use a shell.

In my case it's bash.

Sort order of glob functionality in file system
===============================================

If using a for-loop with standard bash globbing functionality, the sort order
might not be what you would expect.

This example::

    for FILE in *.{mp4,m4v,mkv}
    do
        echo "${FILE}"
    done

will technically be translated into::

    for FILE in *.mp4 *.m4v *.mkv
    do
        echo "${FILE}"
    done

If you have the files:

* 1.mkv
* 2.mp4
* 8.m4v
* 9.m4v

it will be translated into::

    for FILE in 2.mp4 8.m4v 9.m4v 1.mkv
    do
        echo "${FILE}"
    done

ending up in this sort order:

#. 2.mp4
#. 8.m4v
#. 9.m4v
#. 1.mkv

But the question is, if this is what you want?

Maybe, this method makes more sense, if you want to sort excluding the file
extension::

    find . -type f -print -maxdepth 1 | egrep -i "(mp4|m4v|mkv)$" | sort | while read FILE
    do
        echo "${FILE}"
    done

Enhance the experience on the command line
==========================================

This is a collection of tools enhancing your (mostly) interactive command line.

https://github.com/alebcay/awesome-shell

https://github.com/herrbischoff/awesome-command-line-apps

Command line options for shell scripts (getopts)
================================================

I always need a template for ``getopts`` if I need to implement command line
options to shell scripts. So this is a short example::

    #!/bin/bash

    usage() { echo "Usage: $0 [-s <45|90>] [-p <string>] [-a]" 1>&2; exit 1; }

    flag_a=false

    while getopts ":s:p:a" o; do
        case "${o}" in
            s)
                s=${OPTARG}
                ((s == 45 || s == 90)) || usage
                ;;
            p)
                p=${OPTARG}
                ;;
            a)
                flag_a=true
                ;;
            *)
                usage
                ;;
        esac
    done

    shift $((OPTIND-1))

    if ${flag_a}
    then
        echo "Do something"
    fi

The colon (:) after each option specifies an option value. The colon at the
beginning turns it into silent error reporting useful for productive scripts.

And a documentation here: http://wiki.bash-hackers.org/howto/getopts_tutorial
