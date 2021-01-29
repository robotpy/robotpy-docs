
.. _2020_notes:

2020+ Notes
===========

Here are things to know about 2020-2021 that are different from prior years. If you
find things that are different that aren't in this list, please submit a bug
report.

Why is everything so different this year?
-----------------------------------------

From 2015-2020, RobotPy maintained a pure python version of WPILib and bindings
around third party vendor libraries. However, maintaining RobotPy is a lot of
work, and as WPILib and third party vendors add even more features it was
becoming slowly impossible to keep up.

In 2020, we switched to using mostly automatically generated wrappers around
C++ libraries. This has significantly lowered the amount of work needed to
update/maintain RobotPy -- previously it took several volunteers over a month
or two to do updates each year, and now a single person can do much of the
work required within a week or so.

See this `github issue <https://github.com/robotpy/robotpy-wpilib/issues/605>`_ 
for a longer discussion about this.

2020 was a bit bumpy, and in 2021 we're focusing on cleaning things up so that
2022 is super smooth!

Upgrading from 2019 or prior
----------------------------

Before installing 2020 software (or after if you forgot), you should uninstall
the following packages manually:

.. code-block:: sh

    py -3 -m pip uninstall robotpy-hal-sim robotpy-hal-base

After that, you can install RobotPy in the normal way:

.. code-block:: sh

    py -3 -m pip install --upgrade robotpy

See :ref:`Computer Installation <install_computer>` for more details about installing
RobotPy on your computer.

Windows-specific notes
----------------------

* The `Visual Studio 2019 redistributable <https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads>`_
  package is required to be installed.

* CTRE and REV do not support simulation in 32-bit programs, so you must have
  a 64-bit Python installed.

Linux specific notes
--------------------

Linux requires Ubuntu 18.04 or a distribution with an equivalent (or newer)
glibc installation. See :ref:`linux installation page <install_linux>` for
more information.

Command framework
-----------------

As of 2020, the command frameworks are distributed as separate packages. 
See the :ref:`install page <install_commands>` for details.

* :ref:`Legacy Commands API Documentation <command_v1_api>`

Class inheritance
-----------------

When inheriting from RobotPy provided objects, and you provide your own
``__init__`` function, you *must* call the base class' ``__init__`` 
function. If you don't have your own ``__init__`` function, there is
no need to add one, Python will do the right thing automatically.

Here are some examples using the command framework objects, but this 
applies to any RobotPy object that you might inherit from::

    from wpilib.command import Command

    class GoodCommand(Command):
        
        # no custom __init__, nothing extra required

        def foo(self):
            pass

    class AlsoGoodCommand(Command):

        def __init__(self):

            # Call this first!!
            # -> this is calling the base class, so if this is a Command it 
            #    calls Command.__init__, a subsystem would call Subsystem.__init__,
            #    and so on.
            Command.__init__(self)

            # custom stuff here
            self.my_cool_thing = 1
    
    class BadCommand(Command):
        def __init__(self):
            self.my_cool_thing = 1

            # BAD!! you forgot to call Command.__init__, which will result
            # in a difficult to diagnose crash!
    
    class MaybeBadCommand(Command):
        def __init__(self):
            # This is not recommended, as it may fail in some cases 
            # of multiple inheritance. See below
            super().__init__()
            self.my_cool_thing = 1

The `pybind11 documentation <https://pybind11.readthedocs.io/en/stable/advanced/classes.html#overriding-virtual-functions-in-python>`_
recommends against using ``super().__init__()``:

    Note that a direct ``__init__`` constructor should be called, and ``super()``
    should not be used. For simple cases of linear inheritance, ``super()``
    may work, but once you begin mixing Python and C++ multiple inheritance,
    things will fall apart due to differences between Python’s MRO and C++’s
    mechanisms.

What happened to physics and tests?
-----------------------------------

Test support is still not available for 2020+.

The simulation 'physics' support for 2020 has been significantly overhauled
to integrate with the WPILib HAL/Simulation support. As of pyfrc 2020.1.0,
the physics support has been updated and should work with the integrated
field widget that comes with WPILib.

.. note:: 2021 support needs some work still

All of the physics example projects have been updated for 2020, but here
are some particularly useful demos:

* `Basic physics demo <https://github.com/robotpy/examples/tree/main/physics/src>`_
* `NavX rotate to angle <https://github.com/robotpy/examples/tree/main/navx-rotate-to-angle-arcade>`_

Additionally, see the :ref:`PyFRC API docs <physics_model>` for more information.

My code segfaulted and there's no Python stack trace!
-----------------------------------------------------

.. note:: If you are using the Command framework, be sure to upgrade to 
          at least version 2020.2.2.2, as this fixes an issue that could
          cause a crash in command-based code.

We are still working through the bugs, and when you find something like this
here's what you can do:

First, figure out where the code is crashing. Traditional debugging techniques
apply here, but a simple way is to just delete and/or comment out things until
it no longer fails. Then add the last thing back in and verify that the code 
still crashes.

Advanced users can compile a custom version of the robotpy libraries with
symbols and use gdb to get a full stack trace (documentation TBD).

Once you've identified where it crashes, file a bug on github and we can help
you out.

Common causes
~~~~~~~~~~~~~

Python objects are reference counted, and sometimes when you pass one directly
to a C++ function without retaining a reference a crash can occur::

    class Foo:
        def do_something(self):
            some_function(Thing())

In this example, ``Thing`` is immediately destroyed after some_function returns
(because there are no references to it), but some_function (or something else)
tries to use the object after it is destroyed. This causes a segfault or memory
access exception of some kind.

These are considered bugs in RobotPy code and if you report an issue on github
we can fix it. However, as a workaround you can retain a reference to the thing
that you created and that often resolves the issue::

    class Foo:
        def do_something(self):
            self.thing = Thing()
            some_function(self.thing)
