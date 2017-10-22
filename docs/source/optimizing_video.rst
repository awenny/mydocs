**************************************
Optimizing videos in codec and quality
**************************************

These days, you often download videos from the internet to keep in your library
for later watching --- or archiving. But many videos are encoded with poor
video settings and/or not good video codec.

I keep lots of documentations, I download from public video libraries.

Experience proofs, you can optimize file size by the half or even less than
50 %, if using a codec like `HEVC`_ (x265). *I've even seen shrinking the file
size with no noticeable degrading in quality to 10 % sometimes.* And current
hardware usually supports this "new" codec, what requires slightly more
processing powers than eg. x264 --- what is also a very good coded BTW.

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


.. _HEVC: https://de.wikipedia.org/wiki/High_Efficiency_Video_Coding
.. _GitHub: https://github.com/awenny/optimizevideo
