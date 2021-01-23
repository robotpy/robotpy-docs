
.. _install_robotpy:

Robot Installation
==================

These instructions will help you get RobotPy installed on your RoboRIO, which will
allow you to write robot code using Python. If you install RobotPy on your
RoboRIO, you are still able to deploy C++ and Java programs without any conflicts.

.. note:: If you're looking for instructions to use NetworkTables from Python,
          you probably want the :ref:`pynetworktables installation documentation
          <install_pynetworktables>`.

Install requirements
--------------------

.. warning:: This guide assumes that your RoboRIO has the current legal RoboRIO
             image installed. If you haven't done this yet, see :doc:`the WPILib
             documentation <frc:docs/zero-to-robot/step-3/imaging-your-roborio>`
             for imaging instructions. To image the RoboRIO for RobotPy, you
             only need to have the latest FRC Game Tools installed.

RobotPy is truly cross platform, and can be installed from Windows, most Linux
distributions, and from Mac macOS also. To install/use the installer, you must
have Python 3.6+ installed. You should install the installer via pip (requires
internet access) by installing the core RobotPy components (see the 
:ref:`computer installation <install_computer>` section for more details).

On Windows::
  
  py -3 -m pip install robotpy
  
On Linux/macOS::

  pip3 install robotpy

Install process
---------------

The RoboRIO robot controller is typically not connected to a network that has
internet access, so there are two stages to installing RobotPy.

* First, you need to connect your computer to the internet and use the installer
  to download the packages to your computer.
* Second, disconnect from the internet and connect to the network that the RoboRIO
  is on

The details for each stage will be discussed below. You can run the installer via
python. This is slightly different on Windows/macOS/Linux.

Install Python on a roboRIO
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: This step only needs to be done once. 

As of 2021, installing Python and the RobotPy packages are separated into 
two different steps. Once you are connected to the internet, you can run this
to download Python for roboRIO onto your computer.

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer download-python

    Linux/macOS: robotpy-installer download-python

Once everything has downloaded, you can switch to your Robot's network, and
use the following commands to install.

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer install-python

    Linux/macOS: robotpy-installer install-python

It will ask you a few questions, and copy the right files over to your robot
and set things up for you. 

Installing RobotPy on a roboRIO
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The RobotPy installer supports downloading wheels from PyPI and the RobotPy
website and installing them on the roboRIO. The ``download`` and ``install``
commands behave similar to the ``pip`` command, including allowing use of 
a 'requirements.txt' file if desired.

As mentioned above, installation needs to be done in two steps (download then
install). Once you are connected to the internet:

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer download robotpy

    Linux/macOS: robotpy-installer download robotpy

.. seealso:: This command only downloads the core RobotPy packages. See additional
             details for installing :ref:`optional/vendor components
             <robotpy_components>`

Once everything has downloaded, you can switch to your Robot's network, and
use the following commands to install.

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer install robotpy

    Linux/macOS: robotpy-installer install robotpy

The robotpy installer uses pip to download and install packages, so you can 
replace ``robotpy`` above with the name of a pure python package as published
on PyPI.

.. note:: If you need Python packages that require compilation, the RobotPy 
          project distributes some commonly used packages. See the
          `roborio-wheels <https://github.com/robotpy/roborio-wheels/>`_
          project for more details.

Upgrading RobotPy on a roboRIO
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``download`` and ``install`` commands support some pip options, so to 
upgrade you can use the ``-U`` flag on the commands mentioned above to
download the latest versions of RobotPy.

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer download -U robotpy

    Linux/macOS: robotpy-installer download -U robotpy

The robotpy installer can tell you what packages you have installed on a 
roboRIO:

.. code-block:: sh

    Windows:     py -3 -m robotpy_installer list

    Linux/macOS: robotpy-installer list
