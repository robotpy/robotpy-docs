
.. _install_robotpy:

Robot Installation
==================

These instructions will help you get RobotPy installed on your RoboRIO, which will
allow you to write robot code using Python. If you install RobotPy on your
RoboRIO, you are still able to deploy C++ and Java programs without any conflicts.

.. note:: If you're looking for instructions to use NetworkTables from Python,
          you probably want the :ref:`pynetworktables installation documentation
          <install_pynetworktables>`.

Automated installation
----------------------

.. warning:: This guide assumes that your RoboRIO has the current legal RoboRIO
             image installed. If you haven't done this yet, see `the screensteps
             documentation <https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html>`_
             for imaging instructions. To image the RoboRIO for RobotPy, you
             only need to have the latest FRC Game Tools installed.

RobotPy is truly cross platform, and can be installed from Windows, most Linux
distributions, and from Mac macOS also. To install/use the installer, you must
have Python 3.6+ installed. You should install the installer via pip (requires
internet access). The recommended way to install this is bundled in :ref:`pyfrc <install_pyfrc>`, but the following will show individual installation

On Windows::
  
  py -3 -m pip install robotpy-installer
  
On Linux/macOS::

  pip3 install robotpy-installer

There are two stages to installing RobotPy. First, download the artifacts.
Open up a command line, change to a directory you'd like to store the 
downloaded artifacts, and run this::

    Windows:   py -3 -m robotpy_installer download-python
               py -3 -m robotpy_installer download robotpy
	
	Linux/macOS: robotpy-installer download-python
               robotpy-installer download robotpy

Once everything has downloaded, you can switch to your Robot's network, and
use the following commands to install.

	Windows:   py -3 -m robotpy_installer install-python
             py -3 -m robotpy_installer install robotpy
	
	Linux/macOS: robotpy-installer install-robotpy

It will ask you a few questions, and copy the right files over to your robot
and set things up for you. 

Next, you'll want to create some code (or maybe use one of our `examples <https://github.com/robotpy/examples>`_),
and upload it to your robot! Refer to our :ref:`programmers_guide` for more
information.

Upgrading
~~~~~~~~~

From the same directory that you unzipped previously, you can run the same 
installer script to upgrade your robotpy installation. You need to do it in
two phases, one while connected to the internet to download the new release,
and one while connected to the Robot's network.

When connected to the internet::

    Windows:   py -3 -m robotpy_installer download-robotpy
	
	Linux/macOS: robotpy-installer download-robotpy
	
Then connect to the Robot's network::

	Windows:   py -3 -m robotpy_installer install-robotpy
	
	Linux/macOS: robotpy-installer install-robotpy

If you want to use a beta version of RobotPy (if available), you can add the 
--pre argument to the download/install command listed above.
