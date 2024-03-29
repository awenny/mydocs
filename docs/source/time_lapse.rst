*****************
Time lapse videos
*****************

Just recently, I needed to create time lapse videos out of a list of short
video snippets (roughly 2 minutes each).

The result can be found on `GitHub`_.

It does only process video. Audio is --- of course --- omitted. Also, it does
not mux audio tracks automatically into the final time lapse video. If you want
your music, then you need to post process it with your tool of choice.

Basics
======

What I found on several locations on the internet helped me creating this
shell script.

What needs to be done in two steps. Apparently, ``ffmpeg`` cannot process the
time lapse video directly from normal speed videos. That's why you need two
steps until final result:

#. Create stills (as images) on a regular basis
#. Combine these stills as one time lapse video file

It also helps, if you have more than one input video to process into one
time lapse video (aka concatenate them into one).

Details
=======

The script expects one or more folders in the current folder. One folder will
be combined into one time lapse video. Every folder must contain at least one
video file. Multiple levels of folders are not supported.

For every folder, a temporary folder will be created to receive all the images.
If the temporary folder already exists, it will take it and directly generate
the final time lapse video.

If the final time lapse video exists, this input folder will be skipped.

Commands
========

If the script is not needed, but my investigation might be of help, here are
the commands explained, and what they do. For details on the arguments, look
at the `ffmpeg documentation`_.

Generate stills
---------------

With the following command, one input video will be used to generate images on
a regular basis::

    ffmpeg -i INPUT_VIDEO -r 1 -f image2 -vsync cfr tmpdir/%05d.png

Technically, it will save one frame per second.

Unfortunately, I did not manage to insert an actual timestamp into this image
showing the actual timestamp when this scene in the video actually happened
[#f1]_.

.. note:: Look at the ``%05d``. It will be replaced by a running number with
          leading zeros [#f2]_. You can also prefix it with the input video
          file name, it will mitigate the issue, if you have **more than one**
          input video file for one final time lapse video.

Generate final time lapse video
-------------------------------

As you might already foresee is, the final frame rate you're using will
determine the speed of your time lapse video::

    ffmpeg -pattern_type glob -i 'tmpdir/*.png' -c:v libx265 -crf 26 -pix_fmt yuv420p OUTPUT_VIDEO.Extension

If you don't give an argument for targeted frame rate, ``ffmpeg`` will use 25
fps [#f3]_.

.. rubric:: Footnotes

.. [#f1] I'm using a webcam generating a 2 minute video on activity. These
         original video snippets contain a correct date and time information
         in their metadata, when it was captured. But the frames do not contain
         this information. That's why I think it needs to be calculated with
         ``ffmpeg`` somehow.
.. [#f2] Interestingly, I've received an error message from ``ffmpeg`` if this
         value is too big. So be careful with this number.
.. [#f3] Look at the `ffmpeg documentation`_ for details
.. [#f4] Use the comand ``ffmpeg -encoders`` to list all available codecs.

.. _ffmpeg documentation: https://ffmpeg.org/ffmpeg.html
.. _GitHub: https://github.com/awenny/optimizevideo/blob/master/other_tools/timelapse.sh
