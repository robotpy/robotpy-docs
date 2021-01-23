
.. _troubleshooting:

Troubleshooting
===============

.. contents:: :local:

Robot Code
----------

Problem: I can't run code on the robot!
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are lots of things that can go wrong here. It is very important to have
the latest versions of the FIRST robot software installed:

* Robot Image
* Driver Station + Tools

The `FIRST WPILib documentation <https://docs.wpilib.org>`_
contains information on what the current versions are, and how to go about
updating the software.

You should also have the latest version of the RobotPy software packages:

* Do you have the latest version of pyfrc?

.. warning:: Make sure that the version of WPILib on your computer matches the
   version installed on the robot! You can check what version you have locally
   by running::
      
      Windows: py -3 -m pip list
      
      Linux/macOS: pip3 list

1. Did you run the deploy command to put the code on the robot?
2. Make sure you have the latest version of pyfrc! Older versions **won't** work.
3. Read any error messages that pyfrc might give you. They might be useful. :)

Problem: no module named 'wpilib'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you're on your local computer, did you :ref:`install robotpy via pip <install_computer>`?

If you're on the roboRIO, did you :ref:`install RobotPy <install_robotpy>`?

Problem: no module named ...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you're using a non-WPILib vendor library, it must be installed separately.

* :ref:`install_ctre`
* :ref:`install_navx`
* :ref:`install_rev`

If you're on your local computer, did you :ref:`install robotpy via pip <install_computer>`?

If you're on the roboRIO, did you :ref:`install RobotPy <install_robotpy>`?

Problem: pyfrc cannot connect to the robot, or appears to hang
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Can you ping your robot from the machine that you're deploying code from? If not, pyfrc isn't going to be able to connect to the robot either.
2. Try to ssh into your robot, using `PuTTY <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_ or the ``ssh`` command on Linux/macOS. The username to use is ``lvuser``, and the password is an empty string. If this doesn't work, pyfrc won't be able to copy files to your robot
3. If all of that works, it might just be that you typed the wrong hostname to connect to. There's a file called ``.deploy_cfg`` next to your ``robot.py`` that pyfrc created. Delete it, and try again.


Problem: I deploy successfully, but the driver station still shows 'No Robot Code'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Did you use the ``--nc`` option to the deploy command? Your code may have crashed, and the output should be visible on netconsole.
2. If you can't see any useful output there, then ssh into the robot and run ``ps -Af | grep python3``. If nothing shows up, it means your python code crashed and you'll need to debug it. Try running it manually on the robot using this command:: 
    
    python3 /home/lvuser/py/robot.py run

Problem: When I run deploy, it complains that the WPILib versions don't match
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Not surprisingly, the error message is correct.

During deployment, pyfrc does a number of checks to ensure that your robot is setup properly for running python robot code. One of these checks is testing the WPILib version number against the version installed on your computer (it's installed when you install pyfrc).

You should either:

* Upgrade the RobotPy installation on the robot to match the newer version on your computer. See the `RobotPy install guide <http://robotpy.readthedocs.org/en/latest/getting_started.html#upgrading>`_ for more info.
* Upgrade the pyfrc installation on your computer to match the version on the robot. Just run::

      pip3 install pyfrc --upgrade

If you `really` don't want pyfrc to do the version check and need to deploy the code `now`, you can specify the ``--no-version-check`` option. However, this isn't recommended.

.. _troubleshooting_nt:

pynetworktables
---------------

isConnected() returns False!
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Keep in mind that NetworkTables does not immediately connect, and it will
connect/disconnect as devices come up and down. For example, if your program
initializes NetworkTables, sends a value, and exits -- that almost certainly
will fail.

Ensure you're using the correct mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you're running pynetworktables as part of a RobotPy robot -- relax,
pynetworktables is setup as a server automatically for you, just like in
WPILib!

If you're trying to connect to the robot from a coprocessor (such as a
Raspberry Pi) or from the driver station, then you will need to ensure that
you initialize pynetworktables correctly. 

Thankfully, this is super easy as of 2017. Here's the code::

    from networktables import NetworkTables

    # replace your team number below
    NetworkTables.startClientTeam(1234)

Don't know what the right hostname is? That's what the next section is for...

Use static IPs when using pynetworktables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. seealso:: :ref:`networktables_guide`


Problem: I can't determine if networktables has connected
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make sure that you have enabled python logging (it's not enabled by default)::
   
   # To see messages from networktables, you must setup logging
   import logging
   logging.basicConfig(level=logging.DEBUG)

Once you've enabled logging, look for messages that look like this::

    INFO:nt:CONNECTED 10.14.18.2 port 40162 (...)

If you see a message like this, it means that your client has connected to the
robot successfully. If you don't see it, that means there's still a problem.
Usually the problem is that you set the hostname incorrectly in your call to
``NetworkTables.initialize``.

.. _troubleshooting_cscore:

cscore
------

Problem: I can't view my cscore stream via a dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, make sure that your stream is actually working. Connect with a web
browser to the host that the stream is running on on the correct port (if
you are using CameraServer, this will be output via a python logging
message). The default port is 1181.  

The LabVIEW dashboard and Shuffleboard both receive information about
connecting to the stream via NetworkTables. This means that both your
cscore code and the dashboard need to be connected to your robot, and your
robot's code needs to be running. If you have python logging enabled,
then your cscore code should output a message like this if it's connected
to a robot::

    INFO:nt:CONNECTED 10.14.18.2 port 40162 (...)

If it's connected to NetworkTables, then you can use something like the
TableViewer to view the contents of NetworkTables and see if the correct
URL is being published. Look under the 'CameraPublisher' key.

Problem: My image processing code is running at 100% CPU usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You should only encounter this if running your own image processing code. 
If you're just streaming a camera, this should never happen and is a bug.
When doing image processing, there's a few ways you can use too much
CPU, particularly if you do it on a RoboRIO. Here are some thoughts:

* Resizing images is really expensive, don't do that. Instead, set the
  resolution of your camera via the API provided by cscore
* Preallocate your image buffers. Most OpenCV functions will optionally take a
  final argument called 'dst' that it will write the result of the 
  image processing operation to. If you don't provide a 'dst' argument,
  then it will allocate a new image buffer each time. Because image buffers
  can be really large, this adds up quickly.
* Try a really small resolution like 160x120. Most image processing
  tasks for FRC are still perfectly doable at small resolutions. 
* If your framerate is over 10fps, consider bringing it down and see
  if that helps.

Problem: It still doesn't work!
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please `file a bug on github <https://github.com/robotpy/robotpy-cscore/issues>`_
or use one of our :ref:`support channels <support>`.
