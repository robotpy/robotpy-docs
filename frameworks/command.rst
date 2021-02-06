.. _command_framework_docs:

Command Framework
=================

.. note:: As of 2020, the command frameworks are distributed separately from
          WPILib. See the :ref:`installation notes <install_commands>` for
          details.

          This covers the legacy command framework, docs for the new command
          framework will be coming soon!

If you're coming from C++ or Java, you are probably familiar with the :doc:`Command based robot paradigm <frc:docs/software/old-commandbased/index>`.
All of the pieces you're used to are still here, but this guide might help save
you a bit of time as you make the transition.

If you're starting Command based programming in Python and have no experience
with it in other languages, make sure you familiarize yourself with it before
proceeding. This guide only covers differences between Python and the other
languages's versions of that paradigm.

For the impatient, a `fully-working example program <https://github.com/robotpy/examples/tree/main/command-based>`_
is available. You can start with that and modify it to meet your needs.

The Basics
----------
The structure of a Command based program is simple and predictable. You inherit
from the :class:`wpilib.TimedRobot` class, configure the robot in ``robotInit()``,
and then run the ``Scheduler`` inside the ``*Periodic()`` methods.

Writing it can be done rather quickly, but the robotpy commands packages
contains a pre-built skeleton class you can inherit, meaning that your program
only needs to implement functions unique to your robot. Here is an example::

    import wpilib
    from commandbased import CommandBasedRobot

    from commands import AutonomousCommandGroup

    class MyRobot(CommandBasedRobot):

        def robotInit(self):
            '''Initialize things like subsystems'''

            self.autonomous = AutonomousCommandGroup()


        def autonomousInit(self):
            self.autonomous.start()


Setting up the scheduler and running it are handled by the CommandBasedRobot
class. You only need to write the code for ``*Init()`` methods you want to use.
Since you are overriding that class, you can easily change any functionality
that doesn't work for your robot, though :class:`the default implementation <commandbased.commandbasedrobot.CommandBasedRobot>`
should work for most cases.

Error Handling
--------------

Crashes happen. Even the most careful programmer can write a command that breaks
under unexpected conditions. Normally this will cause your program to reboot,
costing precious seconds during a competition. With this in mind, the
``Scheduler`` is run inside an exception handler. If a crash happens inside a
Command while your robot is connection to the Field Management System (i.e. -
during a competition) the exception will be caught, running commands will be
canceled, the error will be printed on the driver's station, and the Scheduler
loop will continue running normally.

If you need more advanced error handling functionality, you can override the
``handleCrash()`` method in your robot.py.

Pythonic Command Based Programming
----------------------------------

All the classes you know from C++ and Java still exists in Python, allowing you
to directly port your code with minimal changes. However, you can use some of
the advantages of Python to make a few things a bit easier.

Subsystems
~~~~~~~~~~

In C++ and Java, the recommended way of making subsystems available to Commands
is to instantiate them in the ``init()`` method of a class that subclasses
``Command``, and then use that as the base class for all of your classes. Even
in those languages that can be a bit unwieldy, especially if you want your
commands to inherit from multiple base classes.

A more appropriate method in Python is to instantiate your subsystems inside a
module, then import that module anywhere you need subsystems. Here is a simple
example of that module, which we will call ``subsystems``::

    from subsystemtype import SubsystemType

    subsystem1 = None

    def init():
        global subsystem1

        subsystem1 = SubsystemType()

You can import this module in your ``robot.py`` and then call
``subsystems.init()`` inside ``robotInit()`` before any commands are
instantiated. Then you can access your subsystem from any Command like this::

    import subsystems
    from wpilib.command import InstantCommand

    class ExampleCommand(InstantCommand):

        def __init__(self):
            self.requires(subsystems.subsystem1)

        def initialize(self):
            subsystems.subsystem1.do_something()

By using this method you can override any Command provided by WPILib or
robotpy-wpilib-utilities, with pythonic namespacing. For even better structure,
make ``subsystems`` a package that holds the code for all of your subsystems, as
demonstrated in the `example program <https://github.com/robotpy/examples/tree/main/command-based/subsystems>`_.

RobotMap
~~~~~~~~

Having a single place to store your robot's configuration can be very helpful,
and this is why most Command based robots integrate a ``RobotMap.*`` file to
store port numbers. In Python you can create a ``robotmap`` module that will act
similarly. There are many different possible ways to manage your ports:

1.) Raw variables::

    drive_front_left = 1
    drive_front_right = 2
    drive_rear_left = 3
    drive_rear_right = 4

2.) Dictionary::

    drive = {
        'front_left': 1,
        'front_right': 2,
        'rear_left': 3,
        'rear_right': 4
    }

3.) Object Properties::

    class PortList():
        pass

    drive = PortList()

    drive.front_left = 1
    drive.front_right = 2
    drive.rear_left = 3
    drive.rear_right = 4

Whichever method you choose, you can utilize it simply by importing::

    import robotmap
    from wpilib.command import Subsystem

    class DriveSubsystem(Subsystem):
        def __init__():
            front_left_motor = robotmap.drive_front_left

Flow Control
--------------

:class:`Command groups <wpilib.command.CommandGroup>`
are great tools for writing complex behaviors, especially for the autonomous
period. A few commands can be strung together effortlessly, creating a readable
flow of behavior. It is possible to run multiple commands at the same time using
the parallel scheduling, or force them into order with sequential scheduling.

:class:`Conditional commands <wpilib.command.ConditionalCommand>`
are a great tool for adding logic to a robotics program. With their introduction
it is possible to choose which ``Command`` to run based on arbitrarily complex
conditions.

Using these two great tools together, however, can be frustrating. If you
attempt to use a ``ConditionalCommand`` inside a ``CommandGroup``, you can no
longer see the complete flow of your logic in a single file. Instead, you must
look at a separate ``ConditionalCommand`` class. And that ``ConditionalCommand``
will reference one or two other commands, which might be command groups with
more conditional commands. As the number of files grow, your project directory
become more cluttered and tracing the flow becomes more of a chore.

It's not that you should not encapsulate behaviors. If you have a ``Command``
that
is called from multiple places, it should be in its own class, but often these
conditional commands and command groups are created to facilitate a single
complex behavior. The ``flowcontrol`` module is meant to address this common
issue. It allows a programmer to use common programming idioms that will be
automatically converted to conditional commands and command groups.

An example::

    import commandbased.flowcontrol as fc
    from wpilib.command import CommandGroup
    from wpilib import DriverStation

    from .drivecommand import DriveCommand
    from .turncommand import TurnCommand

    def noTarget():
        # Arbitrary logic here
        return False

    class Autonomous(CommandGroup):
        ds = DriverStation.getInstance()

        self.addSequential(DriveCommand(24))

        @fc.IF(lambda: ds.getAlliance() == ds.Alliance.Red)
        def turnLeft(self):
            self.addSequential(TurnCommand(90))

        @fc.ELSE
        def turnRight(self):
            self.addSequential(TurnCommand(-90))

        self.addSequential(DriveCommand(12))

        @fc.WHILE(noTarget)
        def turnAround(self):
            self.addSequential(TurnCommand(180))

When the above ``CommandGroup`` is instantiated, the decorators from the
``flowcontrol`` module will automatically build the correct series of
conditional commands and command groups to perform the described steps. The
``flowcontrol`` module provides the following functions:

``IF(condition)``
    A decorator that turns the function it decorates into a
    ``CommandGroup``, and calls that in a ``ConditionalCommand`` if its argument
    returns a ``True`` value. The argument to ``IF`` can be any Python callable,
    including a lambda or class method. It will be evaluated when the
    ``ConditionalCommand`` is started.
``ELIF(condition)``
    Like ``IF``, but it will only happen if all previous
    ``IF`` and ``ELIF`` decorator's conditions returned ``False`` and its
    condition returns ``True``.
``ELSE``
    Follows one or more ``IF`` and ``ELIF`` decorated functions, and only runs if
    all previous conditions returned ``False``.
``WHILE(condition)``
    Creates a ``CommandGroup`` out of the function it decorates, and runs that
    ``CommandGroup`` repeatedly as long as its condition returns ``True``.
``BREAK()``
    This function is not a decorator. It can be placed inline with the
    ``addSequential`` and ``addParallel`` directives of a ``CommandGroup``. When
    this function is encountered, the containing loop will be canceled and
    execution will continue after the loop. If a number is passed to ``BREAK``,
    that many levels of loops will be canceled.
``RETURN()``
    Like ``BREAK``, this is not a decorator. When it is encountered the base
    ``CommandGroup`` in the file will be canceled. Nothing after it will be
    executed.

.. seealso:: :ref:`magicbot_framework_docs`
