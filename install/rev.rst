.. _install_rev:

robotpy-rev install
====================

.. note:: The robotpy-rev bindings are new for 2019, and we're still working
          through issues with them. Please try them out and report any 
          problems you may find!

Setup (tests/simulator)
-----------------------

If you intend to use robotpy-rev in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally::

    pip3 install -U robotpy-rev

Or on Windows::
    
    py -3 -m pip install -U robotpy-rev

Setup (RoboRIO)
---------------

Even if you have robotpy-rev installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Instead, you can download a pre-packaged version from our opkg
repository. Use the RobotPy installation script (comes with the RobotPy download),
and run the following on your computer while connected to the internet::

  py -3 -m robotpy_installer download-opkg robotpy-rev

Then, when connected to the roborio's network, run::

  py -3 -m robotpy_installer install-opkg robotpy-rev

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

REV Firmware and Diagnostics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

robotpy-rev supports all the control features of 
the C++ Spark Max library. Firmware, diagnostics, and other things
must be installed separately using the tools released by REV.

Refer to `the REV C++ documentation <https://www.revrobotics.com/content/sw/max/sw-docs/cpp/index.html>`_
for more details.
