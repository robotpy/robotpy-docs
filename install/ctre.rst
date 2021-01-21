.. _install_ctre:

robotpy-ctre install
====================


Setup (tests/simulator)
-----------------------

.. note:: CTRE is only installable on 64-bit Windows, 64-bit Linux, and OSX

If you intend to use robotpy-ctre in your *robot tests* or via the robot 
*simulator*, you must install this package locally. It is recommended to
install using the robotpy meta package::

    pip3 install -U robotpy[ctre]

Or on Windows::
    
    py -3 -m pip install -U robotpy[ctre]

Setup (RoboRIO)
---------------

Even if you have robotpy-ctre installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Use the RobotPy installer and run the following on your computer
while connected to the internet::

  py -3 -m robotpy_installer download -U robotpy[ctre]

Then, when connected to the roborio's network, run::

  py -3 -m robotpy_installer install robotpy[ctre]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

NI Web Dashboard (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

CTRE Phoenix can integrate with the NI Web Dashboard on the RoboRIO. This is not required to
run robotpy-ctre on the RoboRIO, but it can be a useful diagnostic tool. To install this, you
will need to use the CTRE Lifeboat tool to install it separately.

Refer to `the CTRE documentation <https://phoenix-documentation.readthedocs.io/en/latest/ch05_PrepWorkstation.html#workstation-installation>`_
for more details.
