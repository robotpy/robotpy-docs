
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
             documentation <http://wpilib.screenstepslive.com/s/4485/m/13503/l/144984-imaging-your-roborio>`_
             for imaging instructions. To image the RoboRIO for RobotPy, you
             only need to have the latest FRC Update Suite installed.

RobotPy is truly cross platform, and can be installed from Windows, most Linux
distributions, and from Mac macOS also. To install/use the installer, you must
have Python 3.6+ installed. You should install the installer via pip (requires
internet access).

On Windows::
  
  py -3 -m pip install robotpy-installer
  
On Linux/macOS::

  pip3 install robotpy-installer

There are two stages to installing RobotPy. First, download the artifacts.
Open up a command line, change to a directory you'd like to store the 
downloaded artifacts, and run this::

    Windows:   py -3 -m robotpy_installer download-robotpy
	
	Linux/macOS: robotpy-installer download-robotpy

Once everything has downloaded, you can switch to your Robot's network, and
use the following commands to install.

	Windows:   py -3 -m robotpy_installer install-robotpy
	
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


Manual installation (release)
-----------------------------

.. warning:: This isn't recommended, so you're on your own if you go this route.
             
If you really want to do this, it's not so bad, but then you lose out on
the benefits of the automated installer -- in particular, this method requires
internet access to install the files on the roboRIO in case you need to reimage
your roboRIO.

* Connect your roboRIO to the internet
* SSH in, and copy the following to /etc/opkg/robotpy.conf::

    src/gz robotpy http://www.tortall.net/~robotpy/feeds/2020

* Run this::

    opkg update
    opkg install python38-wpilib netconsole-host

.. note:: When powered off, your roboRIO does not keep track of the correct
          date, and as a result pip may fail with an SSL related error message.
          To set the date, you can either:

          * Set the date via the web interface 
          * You can login to your roboRIO via SSH, and set the date via the
            date command::

          		date -s "2015-01-03 00:00:00"

Upgrading requires you to run the same commands, but with the appropriate
flags set to tell pip3/opkg to upgrade the packages for you.
