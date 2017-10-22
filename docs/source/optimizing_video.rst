**************************************
Optimizing videos in codec and quality
**************************************

These days, you often download videos from the internet to keep in your library
for later watching. But many videos are encoded with poor video settings and
not optimal video codec.

I keep lots of documentations, I download from public video libraries.

Experience proofs, you can optimize file size by the half or even less than
50 %, if using a codec like `HEVC`_ (x265). And current hardware usually supports
this "new" codec, what requires slightly more processing powers than eg. x264.
The idea behind it
==================

What I wanted to do is to define a watch folder, and once I add a new video
file to it, a process in the background will identify it and run a re-encoding
on it. But also keeping track to not process it again and again.

.. _HEVC: https://de.wikipedia.org/wiki/High_Efficiency_Video_Coding

The source code can be found on `GitHub`_.

Solution
========
.. _GitHub: https://github.com/awenny/optimizevideo
