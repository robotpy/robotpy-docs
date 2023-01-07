Design
======

.. _robotpy_extension_options:

Adding options to robot.py
--------------------------

When :func:`wpilib.run` is called, that function determines available commands
that can be run, and parses command line arguments to pass to the commands.
Examples of commands include:

* Running the robot code
* Running the robot code, connected to a simulator
* Running unit tests on the robot code
* And lots more!

python setuptools has a feature that allows you to extend the commands available
to robot.py without needing to modify WPILib's code. To add your own command,
do the following:

* Define a setuptools entrypoint in your package's setup.py (see below)
* The entrypoint name is the command to add
* The entrypoint must point at an object that has the following properties:
    * Must have a docstring (shown when ``--help`` is given)
    * Constructor must take a single argument (it is an argparse parser which options can be added to)
    * Must have a 'run' function which takes two arguments: options, and robot_class. It must
      also take arbitrary keyword arguments via the ``**kwargs`` mechanism. If it receives arguments
      that it does not recognize, the entry point must ignore any such options.

If your command's run function is called, it is your command's responsibility
to execute the robot code (if that is desired). This sample command 
demonstrates how to do this::

    class SampleCommand:
        '''Help text shown to user'''

        def __init__(self, parser):
            pass

        def run(self, options, robot_class, **static_options):
            # runs the robot code main loop
            robot_class.main(robot_class)

To register your command as a robotpy extension, you must add the following
to your setup.py setup() invocation::

    from setuptools import setup

    setup(
          ...
          entry_points={'robotpy': ['name_of_command = package.module:CommandClassName']},
          ... 
          )
