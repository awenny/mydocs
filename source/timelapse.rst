Timelapse videos
================

Just recently, I needed to create timelapse videos out of a list of short
video snippets (roughly 2 minutes each).

The source code can be found on `GitHub`_.

Basics
------

What I found on several locations on the internet helped me creating this
shell script.

What's needs to be done in two steps:

#. Create stills (as images) on a regular basis
#. Combine these stills as one video file

Apparently, there is no other way than having this two step processing.

It also helps, if you have more than one input video to process into one
timelapse video.

Details
-------

The script expects one or more folders in the current folder. One folder will
be combined into one timelapse video. Every folder must contain at least one
video file. Multiple levels of folders are not supported.

For every folder, a temporary folder will be created to receive all the images.
If the temporary folder already exists, it will take it and directly generate
the final video.

If the final timelapse video exists, this input folder will be skipped.

Commands
--------

If the script is not needed, but my investigation might be of help, here are
the commands explained, and what they do. For details on the arguments, look
at the `ffmpeg documentation`_.

Generate stills
^^^^^^^^^^^^^^^

With the following command, one input video will be used to generate images on
an regular basis::

    ffmpeg -i INPUT_VIDEO -r 1 -f image2 -vsync cfr tmpdir/%05d.png

Technically, it will save one frame per second.

Important to mention is the ``%05d``. It will be replaced by a running number
with leading zeros [#f1]_.

Unfortunately, I did not manage to insert an actual timestamp into this image
showing the actual timestamp when this scene in the video actually happened
[#f2]_.

Generate final video
^^^^^^^^^^^^^^^^^^^^

As you might already foresee is the final framerate you're using will determine
the speed of you timelapse video::

    ffmpeg -pattern_type glob -i 'tmpdir/*.png' -c:v libx264 OUTPUT_VIDEO.Extension

If you don't give an argument for targeted framerate, ``ffmpeg`` will use 25
fps [#f3]_.

.. rubric:: Footnotes

.. [#f1] Interestingly, I've received an error message from ``ffmpeg`` if this
         value is too big. So be careful with this number.
.. [#f2] I'm using a webcam generating 2 minute videos on activities. These
         original video snippets contain a correct date and time information,
         when it was captured. But the frames do not contain this information.
.. [#f3] Look at the `ffmpeg documentation`_ for details

.. _ffmpeg documentation: https://ffmpeg.org/ffmpeg.html
.. _GitHub: https://github.com/awenny/optimizevideo/blob/master/other_tools/timelapse.sh
