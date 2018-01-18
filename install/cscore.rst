
.. _install_cscore:

robotpy-cscore install
======================

RoboRIO installation
--------------------

If you have robotpy-installer on your computer, then installing ``robotpy-cscore``
is very simple::
   
   # While connected to the internet
   robotpy-installer download-opkg python36-robotpy-cscore
   
   # While connected to the network with a RoboRIO on it
   robotpy-installer install-opkg python36-robotpy-cscore
    
For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

Compile from source
-------------------

If you're not installing on a RoboRIO, then installation of cscore has a few
additional steps that you need to do:

1. Install a newer C++ compiler that supports C++11
   
   .. note:: robotpy-cscore has not been tested on Windows

2. Install Python 3 (cscore is not supported on Python 2) and development headers
3. Install Numpy (either via pip3 or using a Linux package manager)
4. Install OpenCV (either via a Linux package manager or compile from source)

   * If you compile from source, you **must** enable shared library support, 
     cscore cannot use a statically compiled OpenCV python module at this time
     
5. Install pybind11 via pip3: ``pip3 install "pybind11>=2.2"``
6. Install robotpy-cscore via pip3. It should be as easy as running
   ``pip3 install robotpy-cscore`` -- though be warned, it takes several minutes to
   compile!

In the future we may provide binaries that can be installed on commonly used
platforms, but there are a number of technical challenges that need to be solved
and so that won't be until at least 2018 if not later.

.. warning:: Currently, robotpy-cscore does not support using the OpenCV module
             that comes with the `opencv-python <https://pypi.python.org/pypi/opencv-python>`_
             package distributed on Pypi. Long term, I'd like to get that to
             work, but it's going to take a bit of work. To track this issue,
             see https://github.com/skvark/opencv-python/issues/22
