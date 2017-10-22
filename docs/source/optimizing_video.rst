**************************************
Optimizing videos in codec and quality
**************************************

These days, you often download videos from the internet to keep in your library
for later watching --- or archiving. But many videos are encoded with poor
video settings and/or no good video codec.

I keep lots of documentation videos, I download from various public video
sources.

Experience proofs, you can improve file size to the half or even less, if
using a codec like `HEVC`_ (h265). *I've even seen shrinking the file
size with no noticeable degrade in quality to 10 % sometimes.* And current
hardware usually supports this "new" codec, requiring slightly more
processing power than eg. h264 --- what is also a very good codec, BTW.

The idea behind it
==================

What I wanted to do is to define a watch folder, and once I add a new video
file to it, a process in the background will identify it and run a re-encoding
on it. But also keeping track to not process it again and again.

The source code can be found on `GitHub`_.

Solution
========

Initially, I've created a shell script where you need to process the files
explicitly, and keep track if it is already optimized.

But that was not very convenient.

So, I've created a python program using SQLite to store the metadata. And with
crontab to execute it on a regular basis to identify new incomings and process
them.

Features
========

This is a short list of noticeable features. For full details, consult the
main `GitHub`_ repository documentation and script itself.

* Keep track of processed video files with metadata before/after
* Configurable encoding parameters per watch folder
* Specified watch folder can have multi level subfolders
* No audio re-encoding (by default), but could be set with parameters
* All configuration is done on the command line or with the SQLite data file
* SQLite data file will be created first time if it doesn't exist
* If you understand SQL, you can change the metadata directly

.. _HEVC: https://de.wikipedia.org/wiki/High_Efficiency_Video_Coding
.. _GitHub: https://github.com/awenny/optimizevideo
