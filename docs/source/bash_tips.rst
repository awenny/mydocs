.. highlight:: bash

***********
 Bash tips
***********

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

Syncing partial music library from my Linux to a micro SD card on my Mac
========================================================================

I have my music collection on my Linux box in my cellar. To to listen to Hi-Res
music on the go - also at home because I've quite exceptional head phones, I've
purchase an Android Hi-Res-Audio-Player. This one uses micro SD cards for the
music. As my music library is bigger than one micro SD card, I need to split it
up.

.. hint::

    Of course it works for other cards or mass storage devices you plug into
    your Mac as well. I'm just using the term "SD card" from now on.

While I have two special folders for *Classical* and *Compilations*, all other
artists are in same folder taking up more space than one SD card - with 128 GB
- BTW.

What I'm doing now is, to create a mirror folder structure on my Linux box to
just include the artists I'd like to have on my SD card.

There is one folder with the full library (``/home/Music``) and another one to
mirror a subset of it for the SD cards (``/home/Music.Sync``). Within this
``Music.Sync`` folder, I also keep folders for the names of the multiple SD
cards (SD1, SD2, ...).

The files are organized as:

* Artist
    * Album
        files

And I usually copy all albums of the artist to my SD card.

Step 1: Prepare the mirror sync folder
--------------------------------------

Now, with this command on the Linux box, I keep the mirror for the SD card
up-to-date (in case new albums have been added, changed or removed (highly
unlikely))::

    cd /home/Music.Sync/SD1
    ls -1 | while read DIR
    do
        # remove the mirrored artist
        rm -rf "${DIR}"
        # now copy the folder structure for one artist and hard link the files
        cp -al /home/Music.Sync/SD1/"${DIR}" .
    done

This only works properly, if you have your mirror folder and original library
on same file system, because the ``-l`` option in the ``cp`` command will not
copy the files, they will be hard linked. Not to take up this space on the
Linux box again.

You will see if it's correct if this command just runs for a few seconds.

I will manually invoke the above ``cp`` command if I want a new artist synced
to my SD card.

.. hint::

    Use the command ``du -s --si`` to see if you're exceeding the size of your
    micro SD card. The ``--si`` is important to mimic same calculation of GB
    as Mac is using. A power of 1024 vs 1000. You can compare it with the
    result of ``du -sh``, what is less than the other. And Mac is using a power
    of 1000, what shows a different free space than Linux in general.

Step 2: Sync it to the SD card plugged into my Mac
--------------------------------------------------

On the Mac, I plug in my micro SD card (with an adapter of course). This will
automatically mount it to ``/Volumes/SD1``.

And this small script will sync everything via SSH directly to the SD card::

    #!/usr/bin/env bash

    SSHSyncBaseDir="remote:/home/Music.Sync"
    TargetBaseDir="/Volumes"

    ssh ${SSHSyncBaseDir%%:*} "ls -1 \"${SSHSyncBaseDir##*:}\"" | while read DIR
    do
        # If this device is not mounted, than silently skip
        [ -d "${TargetBaseDir}/${DIR}" ] || continue

        rsync --progress -rtvhe ssh --size-only --iconv=utf-8-mac,utf-8 --delete-before --force-delete ${SSHSyncBaseDir}/${DIR}/ "${TargetBaseDir}/${DIR}"

    done

Tips for the rsync options
--------------------------

It took me some time, until I had all options properly sorted out.

Character set of file names
~~~~~~~~~~~~~~~~~~~~~~~~~~~

While ``rsync`` is not able to automatically identify file name encoding
between Linux and Mac, you need to help.

Use the ``--iconv=utf-8-mac,utf-8`` where the first parameter is for LOCAL.
I'm starting it from my Mac, then it's the first. And the second parameter is
for REMOTE.

If you don't do that, than ``rsync`` will delete and retransfer some files
because of the file name character set. The same applies for the directory
names as well.

Delete before transfer
~~~~~~~~~~~~~~~~~~~~~~

As you're going to transfer to a device with limited space, you want to use it
as efficiently as possible. It will be important to first delete files you
don't need anymore, to free up the space for other files you're going to
transfer.

Use the option ``--delete-before``.

File attributes
~~~~~~~~~~~~~~~

As the file attributes (like last modified, accessed, etc) appear to be
different in details between the Mac and Linux, I had to use the option
``--size-only``, otherwise ``rsync`` would have transferred some files over
and over again, even if nothing changed (at least in my own view).

Meaning: running the sync script. After it finished running it again. I'd
expect nothing to be transferred the second time. But apparently, it did.

So, do yourself something good, and use this option :)

Blanks in folder names
~~~~~~~~~~~~~~~~~~~~~~

I've seen issues with the folder names I had to specify in my shell script
above. And I didn't manage to mask the blanks properly, so I've decided to go
without blanks in those folder names.

The folder and file names, ``rsync`` is transferring in the end are working
fine, just only the base folders I've specified on the command line making
issues.
