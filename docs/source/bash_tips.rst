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

it will result in::

    for FILE in 2.mp4 8.m4v 9.m4v 1.mkv
    do
        echo "${FILE}"
    done

ending in this sort order:

* 2.mp4
* 8.m4v
* 9.m4v
* 1.mkv

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

