.. include:: /include/include.rst
.. include:: /include/include-l2.rst
.. include:: /include/include-description.rst
|EX-REF-LOGO|

**************
 Decoder Notes
**************

|conductor| |tinkerer| |engineer| |support-button|

|EX-CS| works with virtually any decoder using its proprietary programming and ACK detection features. Still, while running locos and sending commands to the main track will always work, you may run into issues during programming operations with ACK Detection. This page describes fixes for any of the decoders users or the |DCC-EX| team found.

The NMRA specification says that a properly operating decoder will acknowledge commands by sending an "ACK pulse" that is 60mA above the idle track current for 6 milliseconds. However, despite |EX-CS| giving a wide margin or tolerance around these, some decoders are so far out of specification, that you may have to upload a change to your command station to get them to program properly.

How to test (diagnostics)

How to program (|JMRI|, |EX-WT|, serial monitor, throttles)

Commands used

other errors (gaps)

https://railsnail.uk/dcc-ex-and-hornby-tts-decoders-no-ack/

https://dccwiki.com/Decoder_Programming

mention "service mode" vs. "operations mode"

Have a link to "Programming Mobile decoders, aka. Reading and Writing CVs" and write that document

.. Todo:: `LOW - Hardware <https://github.com/DCC-EX/dcc-ex.github.io/issues/420>`_ - Finish this page -  Decoder Notes
