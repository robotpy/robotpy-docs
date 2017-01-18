
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

The `FIRST screensteps documentation <https://wpilib.screenstepslive.com/s/4485>`_
contains information on what the current versions are, and how to go about
updating the software.

You should also have the latest version of the RobotPy software packages:

* Do you have the latest version of pyfrc?

.. warning:: Make sure that the version of WPILib on your computer matches the
   version installed on the robot! You can check what version you have locally
   by running::
      
      Windows: py -3 -m pip list
      
      Linux/OSX: pip3 list

1. Did you run the deploy command to put the code on the robot?
2. Make sure you have the latest version of pyfrc! Older versions **won't** work.
3. Read any error messages that pyfrc might give you. They might be useful. :)

Problem: no module named 'wpilib'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you're on your local computer, did you :ref:`install pyfrc via pip <install_pyfrc>`?

If you're on the roboRIO, did you :ref:`install RobotPy <install_robotpy>`?

Problem: pyfrc cannot connect to the robot, or appears to hang
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Can you ping your robot from the machine that you're deploying code from? If not, pyfrc isn't going to be able to connect to the robot either.
2. Try to ssh into your robot, using `PuTTY <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_ or the ``ssh`` command on Linux/OSX. The username to use is ``lvuser``, and the password is an empty string. If this doesn't work, pyfrc won't be able to copy files to your robot
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

    NetworkTables.initialize(server='10.xx.xx.2')

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


