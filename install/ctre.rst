.. _install_ctre:

robotpy-ctre install
====================


Setup (tests/simulator)
-----------------------

If you intend to use robotpy-ctre in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally::

    pip3 install -U robotpy-ctre

Or on Windows::
    
    py -3 -m pip install -U robotpy-ctre

Setup (RoboRIO)
---------------

Even if you have robotpy-ctre installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Instead, you can download a pre-packaged version from our opkg
repository. Use the RobotPy installation script (comes with the RobotPy download),
and run the following on your computer while connected to the internet::

  py -3 installer.py download-opkg python37-robotpy-ctre

Then, when connected to the roborio's network, run::

  py -3 installer.py install-opkg python37-robotpy-ctre

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

NI Web Dashboard (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

CTRE Phoenix can integrate with the NI Web Dashboard on the RoboRIO. This is not required to
run robotpy-ctre on the RoboRIO, but it can be a useful diagnostic tool. To install this, you
will need to use the CTRE Lifeboat tool to install it separately.

Refer to `the CTRE documentation <https://github.com/CrossTheRoadElec/Phoenix-Documentation#installing-phoenix-framework-onto-your-frc-robot>`_
for more details.
