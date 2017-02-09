.. _vision:

Camera & Vision
===============

The RobotPy project provides `robotpy-cscore <https://github.com/robotpy/robotpy-cscore/>`_,
which are python bindings for `cscore <https://github.com/wpilibsuite/cscore>`_,
a high performance camera access and streaming library introduced by FIRST in
2017. It can be used to:

* Stream a USB/HTTP camera to SmartDashboard or the LabVIEW dashboard via HTTP
* Capture images from USB or HTTP camera, modify them using OpenCV/Numpy, and
  send them via HTTP to SmartDashboard, the LabVIEW dashboard, or a web browser.

``robotpy-cscore`` is intended to be usable on any platform supported by OpenCV
and Numpy, and is a more flexible and powerful alternative to solutions such as
mjpg-streamer.

.. note:: cscore is potentially useful outside of the FIRST Robotics Competition,
          as it has very high performance and ease of use compared to other
          solutions.

.. toctree::
    :maxdepth: 2
    
    roborio
    other
    limitations