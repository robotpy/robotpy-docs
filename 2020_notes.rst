
.. _2020_notes:

2020 Notes
==========

Here are things to know about 2020 that are different from prior years. If you
find things that are different that aren't in this list, please submit a bug
report.

.. note:: Most importantly, the API documentation has NOT been upgraded for
          2020 yet. However, our goal is that most API should be identical/close
          to the 2019 API. If you find something that is different, please file
          a bug report!

Why is everything so different this year?
-----------------------------------------

There's 

Upgrading from prior years
--------------------------

You should uninstall the following packages manually:

.. code-block:: sh

    py -3 -m pip uninstall robotpy-hal-sim robotpy-hal-base

Windows-specific notes
----------------------

* The `Visual Studio 2019 redistributable <https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads>`_
  package is required to be installed.

* CTRE and REV do not support simulation in 32-bit programs, so you must have
  a 64-bit Python installed.

OSX-specific notes
------------------

CTRE and REV do not support simulation on OSX, so these packages do not work 
at this time. You can work around this by checking for an import error::

    try:
        import ctre
    except ImportError:
        ctre = None
    
    ... 

    if ctre is not None:
        .. 

A mock solution could be provided for simulation. If you're interested
in developing something, contact us!

Class inheritance
-----------------

When inheriting from RobotPy provided objects, you *must* call the super
constructor otherwise your code will crash with a difficult to diagnose
error::

    from wpilib.command import Command

    class MyCommand(Command):
        def __init__(self):
            Command.__init__(self)

Where is physics and tests?
---------------------------

We are working on the 2020 implementation, and then once completed will
focus on adding support for these.

Some community members have expressed interest in fixing support for
testing/physics, but the work still has not been completed.
