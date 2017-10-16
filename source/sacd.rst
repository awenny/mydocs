Getting started with SACD
=========================

SACDs are a bit boring to use with mobile devices and media servers. So, it's
interesting to convert them into a modern space saving format without losing
any quality.

This guide will show, how to split an SACD ISO into separate tracks in FLAC
format.

Command line processing
-----------------------

I found the tool ``sacd_extract`` for macOS. It's easy to run::

    sacd_extract -i <inputfile.iso> -m -s

It will extract all tracks in individual files. Unfortunately, it does not
support input from <stdin>.

For SACDs it might be important, to use the ``-m`` to extract multi-channel
tracks. By default, ``sacd_extract`` will extract dual channel tracks.

Then a::

    dsf2flac -i <inputfile.dsf>

will convert the DSF file into FLAC format.

Graphical User Interface
------------------------

The - basically very powerful - GUI tool ``XLD`` can be used to do the same.
But I've experienced issues transcoding the ISOs. What renders this tool
useless.
