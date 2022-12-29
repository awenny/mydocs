********************************
Other helpful stuff around video
********************************

Not everything needs to get explained in big documents.
Here are smaller single challenges.

Set language in matroska (mkv) files
====================================

If the mkv file is ready, but missing some metadata, language can be
set with this command::

    mkvpropedit <file> --edit track:a1 --set language=eng

The interesting part here is the track information. (a) for audio,
(v) for video and (s) for subtitle. While the number after starts with 1.
