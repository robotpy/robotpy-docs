.. _robotpy_components:

RobotPy Components
==================

New in 2021, RobotPy now provides a meta installation package that makes it
simpler to install and upgrade RobotPy. The meta package allows you to run
``pip install robotpy`` and this install all of the core RobotPy packages. This
meta package is used both for installation on your computer and on your 
robot.

Optional/Vendor Components
--------------------------

If you install just the ``robotpy`` package via pip, then all of the core 
RobotPy wrappers around WPILib will be installed. However there are several
groups of optional components that you can install. 
Vendor categories:

* ``adi`` - Analog Devices 6-axis accelerometer support
* ``ctre`` - Cross The Road Electronics motor controllers
* ``navx`` - Kauai Labs NavX MXP Robotics Navigation 
* ``rev`` - REV Robotics motor controllers and color sensors

Optional WPILib component categories:

* ``commands2`` - WPILib Commands framework (2020+)
* ``commands`` - WPILib old Commands framework
* ``sim`` - WPILib extra simulation support

Install all vendors and WPILib optional components:

* ``all``

Using components
----------------

Component categories are represented as 'extra requirements' for the RobotPy
package. Pip allows you to install extra requirements by putting the names
of the categories in brackets.

Let's say that you wanted to install the latest version of the NavX software
along with command based programming. You would do this

.. code-block:: sh

    pip3 install -U robotpy[navx,commands]

Or if you wanted to install everything:

.. code-block:: sh

    pip3 install -U robotpy[all]


RoboRIO vs Computer
-------------------

The RobotPy meta package is used for installation on both the RoboRIO and
on your computer.
