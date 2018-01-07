.. _anatomy:

Anatomy of a robot
==================

.. note:: The following assumes you have some familiarity with python, and
          is meant as a primer to creating robot code using the python version
          of wpilib. See our :ref:`python primer <python_primer>` for a brief
          introduction to the python programming language.

This tutorial will go over the things necessary for very basic robot
code that can run on an FRC robot using the python version of WPILib.
Code that is written for RobotPy can be ran on your PC using various
simulation tools that are available.

Create your Robot code
----------------------

Your robot code must start within a file called ``robot.py``. Your code
can do anything a normal python program can, such as importing other
python modules & packages. Here are the basic things you need to know to
get your robot code working!

Importing necessary modules
---------------------------

All of the code that actually interacts with your robot's hardware is
contained in a library called WPILib. This library was originally implemented
in C++ and Java. Your robot code must import this library module, and create
various objects that can be used to interface with the robot hardware.

To import wpilib, it's just as simple as this::

	import wpilib
	
.. note:: Because RobotPy implements the same WPILib as C++/Java, you can learn
          a lot about how to write robot code from the many C++/Java focused
          WPILib resources that already exist, including FIRST's official
          documentation. Just translate the code into python.

Robot object
------------

Every valid robot program must define a robot object that inherits from either
:class:`wpilib.iterativerobot.IterativeRobot` or :class:`wpilib.samplerobot.SampleRobot`. These
objects define a number of functions that you need to override, which get
called at various times.

* :class:`wpilib.iterativerobot.IterativeRobot` functions
* :class:`wpilib.samplerobot.SampleRobot` functions

.. note:: It is recommended that inexperienced programmers use the
		  IterativeRobot framework, which is what this guide will
		  discuss.

An incomplete version of your robot object might look like this::

    class MyRobot(wpilib.IterativeRobot):

        def robotInit(self):
            self.motor = wpilib.Jaguar(1)

The ``robotInit`` function is where you initialize data that needs to be
initialized when your robot first starts. Examples of this data includes:

* Variables that are used in multiple functions
* Creating various wpilib objects for devices and sensors
* Creating instances of other objects for your robot

In python, the constructor for an object is the ``__init__`` function. Instead
of defining a constructor for your main robot object, you can override
robotInit instead. If you do decide that you want to override ``__init__``, then
you must call ``super().__init__()`` in your ``__init__`` method, or an
exception will be thrown.

Adding motors and sensors
-------------------------

Everything that interacts with the robot hardware directly must use the wpilib
library to do so. Starting in 2015, full documentation for the python version
of WPILib is published online. Check out the API documentation (:mod:`wpilib`)
for details on all the objects available in WPILib.

.. note:: 
  You should *only* create instances of your motors and other WPILib hardware
  devices (Gyros, Joysticks, Sensors, etc) either during or after robotInit is
  called on your main robot object. If you don't, there are a lot of things
  that will fail.

Creating individual devices
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let's say you wanted to create an object that interacted with a Jaguar motor
controller via PWM. First, you would read through the table (:mod:`wpilib`) and
see that there is a :class:`.Jaguar` object. Looking further, you can see that
the constructor takes a single  argument that indicates which PWM port to
connect to. You could create the `Jaguar` object that is using port 4 using the
following python code in your `robotInit` method::

    self.motor = wpilib.Jaguar(4)

Looking through the documentation some more, you would notice that to set
the PWM value of the motor, you need to call the :meth:`.Jaguar.set` function. The docs
say that the value needs to be between -1.0 and 1.0, so to set the motor
full speed forward you could do this::

    self.motor.set(1)

Other motors and sensors have similar conventions.
  
Robot drivetrain control
~~~~~~~~~~~~~~~~~~~~~~~~

For standard types of drivetrains (2 or 4 wheel, mecanum, kiwi), you'll want to
use the various included class to control the motors instead of writing
your own code to do it. For most standard drivetrains, you'll want to use one
of three classes:

* :class:`wpilib.drive.DifferentialDrive` for differential drive/skid-steer drive platforms such as 2 or 4 wheel platforms, the Kit of Parts drive base, "tank drive", or West Coast Drive.
* :class:`wpilib.drive.KilloughDrive` for Killough (Kiwi) triangular drive platforms.
* :class:`wpilib.drive.MecanumDrive` for mecanum drive platforms.

For example, when you create a :class:`.DifferentialDrive` object, you can pass in motor controller instances::

    l_motor = wpilib.Talon(0)
    r_motor = wpilib.Talon(1)
    self.robot_drive = wpilib.drive.DifferentialDrive(l_motor, r_motor)

Or you can pass in motor controller groups to use more than one controller per side::

    self.frontLeft = wpilib.Spark(1)
    self.rearLeft = wpilib.Spark(2)
    self.left = wpilib.SpeedControllerGroup(self.frontLeft, self.rearLeft)

    self.frontRight = wpilib.Spark(3)
    self.rearRight = wpilib.Spark(4)
    self.right = wpilib.SpeedControllerGroup(self.frontRight, self.rearRight)

    self.drive = wpilib.drive.DifferentialDrive(self.left, self.right)

Once you have one of these objects, it has various methods that you can use
to control the robot via joystick, or you can specify the control inputs
manually.

.. seealso:: Documentation for the :class:`wpilib.drive.DifferentialDrive`, :class:`wpilib.drive.KilloughDrive`, and :class:`wpilib.drive.MecanumDrive` objects, and the FIRST WPILib Programming Guide.

Robot Operating Modes (IterativeRobot)
--------------------------------------

During a competition, the robot transitions into various modes depending on
the state of the game. During each mode, functions on your robot class
are called. The name of the function varies based on which mode the robot is
in:

* ``disabledXXX`` - Called when robot is disabled
* ``autonomousXXX`` - Called when robot is in autonomous mode
* ``teleopXXX`` - Called when the robot is in teleoperated mode
* ``testXXX`` - Called when the robot is in test mode

Each mode has two functions associated with it. xxxInit is called when the
robot first switches over to the mode, and xxxPeriodic is called 50 times
a second (approximately -- it's actually called as packets are received
from the driver station).
 
For example, a simple robot that just drives the robot using a single
joystick might have a teleopPeriodic function that looks like this::

    def teleopPeriodic(self):
        self.robot_drive.arcadeDrive(self.stick)

This function gets called over and over again (about 50 times per second)
while the robot remains in teleoperated mode.

.. warning:: When using the IterativeRobot as your Robot class, you should
             avoid doing the following operations in the xxxPeriodic functions
             or functions that have xxxPeriodic in the call stack:
             
             * Never use :meth:`.Timer.delay`, as you will momentarily lose
               control of your robot during the delay, and it will not be
               as responsive.
             * Avoid using loops, as unexpected conditions may cause you to
               lose control of your robot.

Main block
----------

Languages such as Java require you to define a 'static main' function. In
python, because every .py file is usable from other python programs, you
need to `define a code block which checks for __main__ <http://effbot.org/pyfaq/tutor-what-is-if-name-main-for.htm>`_.
Inside your main block, you tell WPILib to launch your robot's code using
the following invocation::
    
    if __name__ == '__main__':
        wpilib.run(MyRobot)
        
This simple invocation is sufficient for launching your robot code on the
robot, and also provides access to various RobotPy-enabled extensions that
may be available for testing your robot code, such as pyfrc and robotpy-frcsim.

Putting it all together
-----------------------

If you combine all the pieces above, you end up with something like this
below, taken from one of the samples in our github repository::

    #!/usr/bin/env python3
    """
        This is a good foundation to build your robot code on
    """
    
    import wpilib
    import wpilib.drive
    
    
    class MyRobot(wpilib.IterativeRobot):
    
        def robotInit(self):
            """
            This function is called upon program startup and
            should be used for any initialization code.
            """
            self.left_motor = wpilib.Spark(0)
            self.right_motor = wpilib.Spark(1)
            self.drive = wpilib.drive.DifferentialDrive(self.left_motor, self.right_motor)
            self.stick = wpilib.Joystick(1)
    
        def autonomousInit(self):
            """This function is run once each time the robot enters autonomous mode."""
            self.auto_loop_counter = 0
    
        def autonomousPeriodic(self):
            """This function is called periodically during autonomous."""
    
            # Check if we've completed 100 loops (approximately 2 seconds)
            if self.auto_loop_counter < 100:
                self.drive.arcadeDrive(-0.5, 0)  # Drive forwards at half speed
                self.auto_loop_counter += 1
            else:
                self.drive.arcadeDrive(0, 0)  # Stop robot
    
        def teleopPeriodic(self):
            """This function is called periodically during operator control."""
            self.drive.arcadeDrive(self.stick.getY(), self.stick.getX())
    
        def testPeriodic(self):
            """This function is called periodically during test mode."""
            pass
    
    
    if __name__ == "__main__":
        wpilib.run(MyRobot)

There are a few different python-based robot samples available, and you
can find them in `our github examples repository <https://github.com/robotpy/examples>`_.

Next Steps
----------

This is a good foundation for building your robot, next you will probably want
to know about :ref:`running_robot_code`. 
