
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

.. note:: The installer is included when you install the RobotPy meta package,
          so if you installed that then you likely already have it

To install/use the installer, you must have Python 3.6+ installed. You should install
the installer via pip.

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m pip install robotpy-installer

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         pip3 install robotpy-installer

To upgrade the installed version of the installer, you need to add the ``-U``
flag to pip.

Executing the installer
~~~~~~~~~~~~~~~~~~~~~~~

Once you have the installer program installed, to execute the installer do:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer [command..]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer [command..]

Python
------

These commands allow you to install/upgrade Python on your roboRIO. Once Python
is installed, it's likely that you won't need to upgrade it.

download-python
~~~~~~~~~~~~~~~~

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download-python

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer download-python

This will update the cached Python package to the newest versions available.

install-python
~~~~~~~~~~~~~~~

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer install-python

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer install-python

.. note:: You must already have Python downloaded (via ``download-python``), or
          this command will fail.

Python Packages
---------------

If you want to use a python package hosted on Pypi in your robot code, these
commands allow you to easily download and install those packages.

.. note:: If you need Python packages that require compilation, the RobotPy 
          project distributes some commonly used packages. See the
          `roborio-wheels <https://github.com/robotpy/roborio-wheels/>`_
          project for more details.

download
~~~~~~~~

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download PACKAGE [PACKAGE ..]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer download PACKAGE [PACKAGE ..]

Specify python package(s) to download, similar to what you would pass the
'pip install' command. This command does not install files on the robot, and
must be executed from a computer with internet access.

You can run this command multiple times, and files will not be removed from 
the download cache.

You can also use a `requirements.txt` file to specify which packages should
be downloaded.

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download -r requirements.txt

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer download -r requirements.txt

install
~~~~~~~

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer install PACKAGE [PACKAGE ..]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer install PACKAGE [PACKAGE ..]

Copies python packages over to the roboRIO, and installs them. If the
package already has been installed, it will be reinstalled.

You can also use a `requirements.txt` file to specify which packages should
be downloaded.


.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer install -r requirements.txt

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer install -r requirements.txt

.. warning:: The 'install' command will only install packages that have been
             downloaded using the 'download' command, or packages that are
             on the robot's pypi cache.

.. warning:: If your robot does not have a python3 interpeter installed, this
             command will fail. Run the `install-python` command first.


