.. _install_rev:

robotpy-rev install
====================

Setup (tests/simulator)
-----------------------

.. note:: REV SPARK support is only installable on 64-bit Windows and Linux. REV
          does not provide OSX binaries -- contact their support and let them
          know this is something your team wants!

If you intend to use robotpy-rev in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally. It is recommended to
install using the robotpy meta package::

    pip3 install -U robotpy[rev]

Or on Windows::
    
    py -3 -m pip install -U robotpy[rev]

Setup (RoboRIO)
---------------

Even if you have robotpy-rev installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Use the RobotPy installer and run the following on your computer
while connected to the internet::

  py -3 -m robotpy_installer download robotpy[rev]

Then, when connected to the roborio's network, run::

  py -3 -m robotpy_installer install robotpy[rev]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

REV Firmware and Diagnostics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

robotpy-rev supports all the control features of 
the C++ Spark Max library. Firmware, diagnostics, and other things
must be installed separately using the tools released by REV.

Refer to `the REV C++ documentation <https://www.revrobotics.com/content/sw/max/sw-docs/cpp/index.html>`_
for more details.
