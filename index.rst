RobotPy Documentation
=====================

.. image:: robotpy.png
   :align: right

Welcome! RobotPy is a project created by a community of FIRST mentors and
students dedicated to developing python-related projects for the FIRST Robotics
Competition. This documentation site contains information about various projects
that RobotPy supports, including guides and API references.

As of 2024 Python is an officially supported language for programming robots in FRC, and we have moved much of our installation and usage documentation over to :doc:`a section of the WPILib documentation <frc:docs/software/python/index>`.

.. note:: Please read our :ref:`Upgrade Notes<upgrade_notes>` page for things that have changed this season that you should be aware of.

Getting Started
---------------

* The best place to start is the :doc:`zero-to-robot documentation <frc:docs/zero-to-robot/introduction>`.
* Also see :doc:`frc:docs/software/python/index`

Projects
--------

The primary reason RobotPy exists is to support teams that want to write their
FRC robot code using Python, and we have several projects related to this:

* `mostrobotpy <https://github.com/robotpy/mostrobotpy>`_: provides the ability to use the core WPILib libraries from Python
* `pyfrc <https://github.com/robotpy/pyfrc>`_: provides unit testing and realtime robot simulation
* `robotpy-wpilib-utilities <https://github.com/robotpy/robotpy-wpilib-utilities>`_: Community focused extensions for WPILib

Additionally, RobotPy is home to several projects that are useful for all teams,
even if they aren’t writing their robot code in python:

* :ref:`pyntcore <robotpy:ntcore_api>`: python bindings for NetworkTables that you can use to communicate with a dashboard and/or your robot.
* `pynetworktables <https://github.com/robotpy/pynetworktables>`_: legacy NetworkTables implementation that you can use to communcate with SmartDashboard and/or your robot.
* :ref:`robotpy-cscore <robotpy:cscore_api>`: Python bindings for cscore, a powerful camera/streaming library
* :ref:`robotpy-apriltag <robotpy:robotpy_apriltag_api>`: Python bindings for the WPILib apriltag library
* `roborio-vm <https://github.com/robotpy/roborio-vm>`_: Scripts to create a QEMU virtual machine from the RoboRIO image file

We are in the process of migrating much of our documentation to the :doc:`WPILib documentation site <frc:index>`.

.. toctree::
    :caption: RobotPy
    :maxdepth: 2

    upgrade_notes
    frameworks/index
    guidelines
    testing
    troubleshooting
    support
    faq
    
.. include:: _sidebar.rst.inc

.. toctree::
    :caption: RobotPy Developers
    :maxdepth: 2

    dev/index

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

