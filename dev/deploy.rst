
.. _internal_deploy:

Deploy process details
======================

When the code is uploaded to the robot, the following steps occur:

* SSH/sftp operations are performed as the ``lvuser`` user (this is REALLY important, don't use the ``admin`` user!)
* pyfrc does some checks to make sure the environment is setup properly
* The directory containing ``robot.py`` is recursively copied to the the directory ``/home/lvuser/py``
* The files ``robotCommand`` and ``robotDebugCommand`` are created
* ``/usr/local/frc/bin/frcKillRobot.sh -t -r`` is called, which causes any existing robot code to be killed, and the new code is launched

If you wish for the code to be started up when the roboRIO boots up, you need to
make sure that "Disable RT Startup App" is **not** checked in the roboRIO's web
configuration.

These steps are compatible with what C++/Java does when deployed by eclipse,
so you should be able to seamlessly switch between python and other FRC
languages!

.. _manual_code_deploy:

How to manually run code
------------------------

.. note:: Generally, you shouldn't need to use this process.

If you don't have (or don't want) to install pyfrc, running code manually is
pretty simple too. 

1. Make sure you have RobotPy installed on the robot
2. Use scp or sftp (Filezilla is a great GUI product to use for this) to copy
   your robot code to the roboRIO
3. ssh into the roboRIO, and run your robot code manually

::

	python3 robot.py run 

Your driver station should be able to connect to your code, and it will be able
to operate your robot!

.. note:: This is good for running experimental code, but it won't start the
          code when the robot starts up. Use pyfrc to do that.
