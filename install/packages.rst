
.. _install_packages:

RoboRIO Package Installer
=========================

.. note:: This is not the RobotPy installation guide, see :ref:`install_robotpy`
          if you're looking for that!

Most FRC robots are not placed on networks that have access to the internet,
particularly at competition arenas. The RobotPy installer is designed for 
this type of 'two-phase' operation -- with individual steps for downloading
and installing packages separately.

The RobotPy installer supports downloading external packages from the python
package repository (pypi) via pip, and installing those packages onto the robot.
We cannot make any guarantees about the quality of external packages, so use
them at your own risk.

.. note:: If your robot is on a network that has internet access, then you
          can manually install packages via opkg or pip. However, if you use
          the RobotPy installer to install packages, then you can easily
          reinstall them on your robot in the case you need to reimage it.

          If you choose to install packages manually via pip, keep in mind that
          when powered off, your roboRIO does not keep track of the correct
          date, and as a result pip may fail with an SSL related error message.
          To set the date, you can either:

          * Set the date via the web interface 
          * You can login to your roboRIO via SSH, and set the date via the
            date command::

              date -s "2015-01-03 00:00:00"

Each of the commands supports various options, which you can read about by
invoking the --help command.

Installing/Executing the installer
----------------------------------

To install/use the installer, you must have Python 3 installed. You can install
the installer via pip (recommended) or you can use ``installer.py`` directly
(see below).

On Windows::
  
  py -3 -m pip install robotpy-installer
  
On Linux/OSX::

  pip3 install robotpy-installer

To upgrade the installed version of the installer, you need to add the ``-U``
flag to pip.

Executing the installer
~~~~~~~~~~~~~~~~~~~~~~~

Once you have the installer program installed, to execute the installer on
Windows you can do::
  
  py -3 -m robotpy_installer [command..]
  
On Linux/OSX::

  robotpy-installer [command..]

Download and use installer.py
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can download the file directly, or use the version that comes with the
RobotPy download files. Once you have obtained the ``installer.py`` file, to
execute the file:

On Windows::

  py -3 installer.py [command..]
  
On OSX/Linux::

  python3 installer.py [command..]


RobotPy
-------

These commands allow you to install/upgrade RobotPy without needing to download
a new version from github.

install-robotpy
~~~~~~~~~~~~~~~

::

	robotpy-installer install-robotpy

This will copy the appropriate RobotPy components to the robot, and install
them. If the components are already installed on the robot, then they will
be reinstalled. You must already have the RobotPy components downloaded (via
``download-robotpy``), or this command will fail.

download-robotpy
~~~~~~~~~~~~~~~~

::

	robotpy-installer download-robotpy

This will update the cached RobotPy packages to the newest versions available.

Python Packages
---------------

If you want to use a python package hosted on Pypi in your robot code, these
commands allow you to easily download and install those packages.

.. note:: Some binary packages such as NumPy need to be installed as opkg
          packages

download-pip
~~~~~~~~~~~~

::

	robotpy-installer download-pip PACKAGE [PACKAGE ..]

Specify python package(s) to download, similar to what you would pass the
'pip install' command. This command does not install files on the robot, and
must be executed from a computer with internet access.

You can run this command multiple times, and files will not be removed from 
the download cache.

You can also use a `requirements.txt` file to specify which packages should
be downloaded.

::

	robotpy-installer download-pip -r requirements.txt

install-pip
~~~~~~~~~~~

::

	robotpy-installer install PACKAGE [PACKAGE ..]

Copies python packages over to the roboRIO, and installs them. If the
package already has been installed, it will be reinstalled.

You can also use a `requirements.txt` file to specify which packages should
be downloaded.

::

	robotpy-installer install-pip -r requirements.txt

.. warning:: The 'install' command will only install packages that have been
             downloaded using the 'download' command, or packages that are
             on the robot's pypi cache.

.. warning:: If your robot does not have a python3 interpeter installed, this
             command will fail. Run the `install-robotpy` command first.

.. _install_ipk:

IPK (binary) packages
---------------------

The RobotPy project maintains a number of custom binary packages that are useful
for FRC teams. For a list of packages that you can install, see
`https://www.tortall.net/~robotpy/feeds/2017/ <https://www.tortall.net/~robotpy/feeds/2017/>`_.
These commands can be used to install those and other precompiled Linux packages
that NI makes available.

download-opkg
~~~~~~~~~~~~~

::

    robotpy-installer download-opkg PACKAGE [PACKAGE ..]

Downloads an ipk file from the RobotPy and NI's online opkg repositories, along
with the dependencies for the package.

install-opkg
~~~~~~~~~~~~

::

    robotpy-installer install-opkg PACKAGE [PACKAGE ..]

Copies ipk files over to the roboRIO, and installs them and their dependencies.
If the package already has been installed, it will do nothing.

.. warning:: The ``install-opkg`` command will only install packages that have
             been downloaded using the ``download-opkg`` command, or packages
             that are already in the robot's opkg cache
