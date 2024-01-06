
.. _faq:

FAQ
===

Here you can find answers to some of the most frequently asked questions
about RobotPy.

Should our team use RobotPy?
----------------------------

What we often recommend teams to do is to take their existing code for their
existing robot, and translate it to RobotPy and try it out first in the
robot simulator, followed by the real robot. This will give you a good taste
for what developing code for RobotPy will be like.

Related questions for those curious about RobotPy:

* :ref:`is_legal`
* :ref:`is_stable`
* :ref:`is_fast`

Installing and Running RobotPy
------------------------------

How do I install RobotPy?
~~~~~~~~~~~~~~~~~~~~~~~~~

See our :ref:`getting started guide <getting_started>`.

What version of Python do RobotPy projects use?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When running RobotPy on a FIRST Robot, our libraries/interpreters use
Python 3. This means you should reference the `Python 3.x
documentation <https://docs.python.org/3/>`__ instead of the Python
2.x documentation.

-  RobotPy WPILib on the roboRIO uses the latest version of Python 3 at kickoff.
   In 2024, this was Python 3.12.  When using pyfrc or similar projects,
   you should use a Python 3.8 or newer interpreter (the latest is recommended).
-  RobotPy 2014.x is based on Python 3.2.5.

`pynetworktables <https://github.com/robotpy/pynetworktables>`__ is
compatible with Python 3.5 or newer, since 2019.
Releases prior to 2019 are also compatible with Python 2.7.

What happens when my code crashes?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An exception will be printed out to the console, and the Driver Station
log may receive a message as well. It is highly recommended that you
enable NetConsole for your robot, so you can see these messages.

Is WPILib available?
~~~~~~~~~~~~~~~~~~~~

Of course! Just ``import wpilib``. Class and function names are identical
to the Java version. Check out the :ref:`Python WPILib API Reference <wpilib_api>`
for more details.

As of 2020, the API mostly matches the C++ version of WPILib, except that protected
functions are prefixed with an underscore (but are availble to all Python code).

From 2015-2019, almost all classes and functions from the Java WPILib are available
in RobotPy's WPILib implementation.

Prior to 2015, the API matched the C++ version of WPILib.

Is Command-based programming available?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Of course! Check out the :mod:`commands2 <commands2>` package.

Are Vendor libraries available?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We encourage vendors to make Python versions of their libraries available. Since
Python support has only been official since 2024, not all vendors do this. If
you are a vendor, please reach out to our team and we'd be happy to assist.

The RobotPy project also provides unofficial wrappers for vendor libraries that don't
take a lot of effort to create and maintain.

Competition
-----------

.. _is_legal:

Is RobotPy competition-legal?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As of 2024, Python is officially supported for use in FRC.

.. _is_stable:

Is RobotPy stable?
~~~~~~~~~~~~~~~~~~

Yes! Teams have been
using RobotPy since 2010, and the maintainer of RobotPy is a member of the
WPILib team. Much of the time when bugs are found, they are found in the
underlying WPILib, instead of RobotPy itself.

One caveat to this is that because RobotPy doesn't have a beta period like
WPILib does, bugs tend to be found during the first half of competition season.
However, by the time build season ends, RobotPy is just as stable as any of
the officially suported languages.

How often does RobotPy get updated?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

RobotPy is a community project, and updates are made whenever community members
contribute changes and the developers decide to push a new release.

Historically, RobotPy tends to have frequent releases at the beginning of build
season, with less frequent releases as build season goes on. We try hard to avoid
WPILib releases after build season ends, unless critical bugs are found.

Performance
-----------

.. _is_fast:

Is RobotPy fast?
~~~~~~~~~~~~~~~~

It's fast enough.

We've not yet benchmarked it, but it's almost certainly just as fast as
Java for typical WPILib-using robot code. RobotPy uses the native C++
WPILib, and thus the only interpreted portions are your specific robot
actions. If you have particularly performance sensitive code, you can
write it in C++ and use pybind11 wrappers to interface to it from Python.

RobotPy Development
-------------------

Who created RobotPy?
~~~~~~~~~~~~~~~~~~~~

RobotPy was created by Peter Johnson, programming mentor for FRC Team
294, `Beach Cities Robotics <http://www.bcrobotics.org/>`_. He was
inspired by the `Lua port for the
cRIO <http://redmine.zombiezen.com/projects/greyhoundlua/>`__ created by
Ross Light, FRC Team 973. Peter is a member of the FIRST WPILib team,
and also created the `ntcore <https://github.com/wpilibsuite/ntcore/>`_
and `cscore <https://github.com/wpilibsuite/cscore/>`_ libraries.

The current RobotPy maintainer is `Dustin
Spicuzza <http://github.com/virtuald>`_, also a member of the FIRST WPILib team.

Current RobotPy developers include:

* Dustin Spicuzza (`@virtuald <https://github.com/virtuald>`_)
* David Vo (`@auscompgeek <https://github.com/auscompgeek>`_)
* Vasista Vovveti (`@TheTripleV <https://github.com/TheTripleV>`_)

How can I help?
---------------

RobotPy is an open project that all members of the FIRST community can
easily and quickly contribute to. If you find a bug, or have an idea
that you think others can use:

-  Test and report any issues you find.
-  Port and test a useful library.
-  Write a Python module and share it with others (and contribute it to
   the
   `robotpy-wpilib-utilities <https://github.com/robotpy/robotpy-wpilib-utilities>`__
   package!)
