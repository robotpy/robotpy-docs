
.. _vision_roborio:

On the RoboRIO 
==============

.. _vision_gil_warning:

.. warning:: Image processing is a CPU intensive task, and because of the Python
             Global Interpreter Lock (GIL) **we do NOT recommend using robotpy-cscore
             directly in your robot process**. Don't do it. Really.
             
             Instead, we provide easy to use ways to launch your camera/image
             processing code from your Python robot code, and it won't break
             simulation either! See below for details.

             For more information on the GIL and its effects, you may wish to
             read the following resources:
             
             * `Python Wiki: Global Interpreter Lock <https://wiki.python.org/moin/GlobalInterpreterLock>`_
             * `Efficiently Exploiting Multiple Cores with Python <http://python-notes.curiousefficiency.org/en/latest/python3/multicore_python.html>`_

.. note::  The following assumes you're writing your robot code and your image
           processing code using RobotPy. However, if you're writing your Robot
           code using Java, we do have an example which would allow you to
           launch Python image processing code from your Java Robot code. `See
           this file for details <https://github.com/robotpy/robotpy-cscore/blob/master/examples/CameraServer.java>`_.

Installation
------------

``robotpy-cscore`` can be easily installed with the RobotPy installer. See
:ref:`these instructions <install_cscore>` for details.

.. _vision_roborio_automatic:

Automatic camera streaming
--------------------------

If you do not wish to modify or process the images from your camera, and *only*
wish to stream a single camera via HTTP to a dashboard, then you only need to
add the following to your ``robotInit`` function::
          
    wpilib.CameraServer.launch()

That's it! You should be able to connect to the camera using SmartDashboard,
the default LabVIEW Dashboard, or if you point your browser at
``http://roborio-XXXX-frc.local:1181``.

The `quick vision example <https://github.com/robotpy/examples/tree/master/cscore-quick-vision>`_
can be found in the RobotPy examples repository.

Image processing
----------------

Because the GIL exists (:ref:`see above <vision_gil_warning>`), RobotPy's WPILib
implementation provides a way to run your image processing code in a separate
process. This introduces a number of rules that your image processing code must
follow to efficiently and safely run on the RoboRIO:

* Your image processing code must be in its own file
* Never import the ``cscore`` package from your robot code, it will just waste
  memory
* Never import the ``wpilib`` or ``hal`` packages from your image processing
  code
  
  .. warning:: ``wpilib`` may not be imported from two programs on the RoboRIO.
               If this happens, the second program will attempt to kill the first
               program.

vision file
~~~~~~~~~~~

The first step you need to do is create a file -- let's call it ``vision.py``,
and stick it in the same directory as your ``robot.py`` file. You can also put
it in a subdirectory underneath your robot code, and the robot deploy command
will copy it to the robot.

.. seealso:: :ref:`vision_code`

.. _vision_roborio_runcustom:

robot.py
~~~~~~~~

Once you have written your cscore code, in the ``robotInit`` function in
your ``robot.py`` file you need to add the following line::
  
    wpilib.CameraServer.launch('vision.py:main')

The parameter provided to launch is of the form ``FILENAME:FUNCTION``. For example,
if your code was located in the ``camera`` subdirectory in a file called
``targeting.py``, and your function was called ``run``, then you would do::
  
    wpilib.CameraServer.launch('camera/targeting.py:run')
  
Important notes
~~~~~~~~~~~~~~~
  
* Your image processing code will be launched via a stub that will setup logging
  and initialize pynetworktables to talk to your robot code
* The child process will NOT be launched when running the robot code in
  simulation or unit testing mode
* If your image processing code contains a ``if __name__ == '__main__':`` block,
  the code inside that block will NOT be executed when the code is launched from
  ``robot.py``
* The camera code will be killed when the ``robot.py`` program exits. If you
  wish to perform cleanup, you should register an atexit handler.

The `intermediate vision example <https://github.com/robotpy/examples/tree/master/cscore-intermediate-vision>`_
can be found in the RobotPy examples repository.

More information
----------------

* The `WPILib screensteps documentation for cscore <http://wpilib.screenstepslive.com/s/4485/m/24194/l/682778-read-and-process-video-cameraserver-class>`_
  may be useful to explain concepts (though some details are different)
* :ref:`CSCore Troubleshooting <troubleshooting_cscore>`
