
.. _vision_limitations:

Limitations
===========

opencv-python
-------------

``robotpy-cscore`` is not currently able to use the ``opencv-python`` package
on pypi, you will need to install python bindings for OpenCV some other way.

USB Camera Support
------------------

The cscore library currently only supports USB cameras on Linux. On other
platforms you will want to use OpenCV to open a VideoCamera object. You can
check at runtime to see if cscore is compiled with USB camera support by
checking for the presence of a 'UsbCamera' class. If it's not there, it's
not supported.

See the ``cvstream.py`` example for using OpenCV in this manner.
