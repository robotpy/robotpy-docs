
.. _install_cscore:

robotpy-cscore install
======================

.. note:: cscore is not installed when you ``pip install robotpy``

RoboRIO installation
--------------------

If you have robotpy-installer on your computer, then installing ``robotpy-cscore``
is very simple::

.. tab:: Windows

   .. code-block:: sh

      # While connected to the internet
      py -3 -m robotpy_installer download robotpy-cscore

      # While connected to the network with a RoboRIO on it
      py -3 -m robotpy_installer install robotpy-cscore

.. tab:: Linux/macOS

   .. code-block:: sh
   
      # While connected to the internet
      robotpy-installer download robotpy-cscore

      # While connected to the network with a RoboRIO on it
      robotpy-installer install robotpy-cscore
    
For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

Non-roboRIO installation
------------------------

GNU/Linux packages
~~~~~~~~~~~~~~~~~~

If you're using Debian, Ubuntu, Fedora, or openSUSE, follow the instructions on
https://software.opensuse.org/download.html?project=home%3Aauscompgeek%3Arobotpy&package=python3-cscore

If you're using Arch Linux, you can follow the instructions at
https://software.opensuse.org/download.html?project=home%3Aauscompgeek%3Arobotpy&package=python-cscore
to install a precompiled package.

A full list of supported platforms is available on
https://build.opensuse.org/package/show/home:auscompgeek:robotpy/robotpy-cscore

Compiling from source
~~~~~~~~~~~~~~~~~~~~~

If you're not installing on a RoboRIO, then installation of cscore has a few
additional steps that you need to do:

1. Ensure a C++ compiler that supports C++17 is installed
2. Install Python 3 (cscore is not supported on Python 2) and development headers
3. Ensure that you have a recent version of pip3/setuptools installed (either via pip3 or using a Linux package manager)

   * It has been reported that build failures may occur with certain combinations of pip/setuptools installed
   
4. Install Numpy (either via pip3 or using a Linux package manager)
5. Install OpenCV (either via a Linux package manager or compile from source)

   * If you compile from source, you **must** enable shared library support,
     cscore cannot use a statically compiled OpenCV python module at this time
   * If you use OpenCV 4, you must also compile with ``-DOPENCV_GENERATE_PKGCONFIG=ON``
     and install ``pkgconfig`` via pip for robotpy-cscore to find it.

6. Install robotpy-cscore via pip3. It should be as easy as running
   ``pip3 install robotpy-cscore`` -- though be warned, it takes several minutes to
   compile!

.. note::

   robotpy-cscore has only been lightly tested on Windows, and installation
   of OpenCV is slightly complicated, but you may have luck using conda.  See
   `robotpy-cscore issue #17 <https://github.com/robotpy/robotpy-cscore/issues/17>`_
   for more details.

.. note:: CSCore does not support reading USB cameras on macOS.

.. warning:: Currently, robotpy-cscore does not support using the OpenCV module
             that comes with the `opencv-python <https://pypi.python.org/pypi/opencv-python>`_
             package distributed on Pypi. Long term, I'd like to get that to
             work, but it's going to take a bit of work. To track this issue,
             see https://github.com/skvark/opencv-python/issues/22

Next steps
----------

See our :ref:`cscore documentation <vision>` for examples and deployment thoughts.
