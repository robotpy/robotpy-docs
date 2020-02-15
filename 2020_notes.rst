
.. _2020_notes:

2020 Notes
==========

Here are things to know about 2020 that are different from prior years. If you
find things that are different that aren't in this list, please submit a bug
report.

Why is everything so different this year?
-----------------------------------------

From 2015-2020, RobotPy maintained a pure python version of WPILib and bindings
around third party vendor libraries. However, maintaining RobotPy is a lot of
work, and as WPILib and third party vendors add even more features it was
becoming slowly impossible to keep up.

In 2020, we switched to using mostly automatically generated wrappers around
C++ libraries. Ideally, once completed, this will significantly lower the
amount of work needed to update/maintain RobotPy in the future. Unfortunately
in the short term, there's a lot of work needed to get there (however -- we're
most of the way there as of this writing!).

See this `github issue <https://github.com/robotpy/robotpy-wpilib/issues/605>`_ 
for a longer discussion about this.

2020 has been a bit bumpy, but with your help hopefully we can smooth out
the rough spots and make 2021 a seamless transition!

Upgrading from prior years
--------------------------

Before installing 2020 software (or after if you forgot), you should uninstall
the following packages manually:

.. code-block:: sh

    py -3 -m pip uninstall robotpy-hal-sim robotpy-hal-base

After that, you can install pyfrc et al in the normal way:

.. code-block:: sh

    py -3 -m pip install --upgrade pyfrc

See :ref:`Installing pyfrc <install_pyfrc>` for more details about installing
RobotPy on your computer.

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

Where is physics and tests?
---------------------------

We are working on the 2020 implementation, and then once completed will
focus on adding support for these.

Some community members have expressed interest in fixing support for
testing/physics, but the work still has not been completed.

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
