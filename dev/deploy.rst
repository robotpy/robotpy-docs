
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

Deploy Artifacts
----------------

During the deploy process, robotpy will generate a ``deploy.json`` that can provide
your robot with extra information regarding how code was deployed. It contains information that could
be used for example, to alert you if your hash contains the git ``-dirty`` flag, or assist in debugging
an issue by exploring who, when and with what code was deployed with.

The ``deploy.json`` is not present on the dev filesystem and is copied over via sftp at deploy time.
It contains the following keys:

.. code-block:: json

   {
    "git-desc": "2022.1-8-gb4fc2aca-dirty",
    "git-hash": "b4fc2aca399810f1fe28faf23314cd422a6db920",
    "git-branch": "feat/working_code",
    "deploy-host": "MyLaptop",
    "deploy-user": "me",
    "deploy-date": "2018-6-10T02:40:55",
    "code-path": "/home/me/robots/MyRobotCode"
   }

If you do not manage your code with git, use another VCS, or do not have git installed locally and on your
path in the usual location, the git tag will not be present.

You can use ``./robotpy.py deploy-info`` to connect to the robot and fetch the deploy.json.

Example code:

.. code-block:: python

   #!/usr/bin/env python3

   import os
   import json
   import wpilib.deployinfo


   class MyRobot(wpilib.TimedRobot):
      def robotInit(self):
         data = wpilib.deployinfo.getDeployData()

         print(data)

   if __name__ == "__main__":
      wpilib.run(MyRobot)


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
