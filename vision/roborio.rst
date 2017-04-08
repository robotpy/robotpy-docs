
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

.. note:: robotpy-cscore is very new, so if you run into strange errors or
          crashes please `file a bug on github <https://github.com/robotpy/robotpy-cscore/issues>`_
          or use one of our :ref:`support channels <support>`.

.. note::  The following assumes you're writing your robot code and your image
           processing code using RobotPy. However, if you're writing your Robot
           code using Java, we do have an example which would allow you to
           launch Python image processing code from your Java Robot code. `See
           this file for details <https://github.com/robotpy/robotpy-cscore/blob/master/examples/CameraServer.java>`_.

Installation
------------

``robotpy-cscore`` can be easily installed with the RobotPy installer. See
:ref:`these instructions <install_cscore>` for details.

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
* Never import the ``wpilib`` or ``hal`` packages from your image
  
  .. warning:: ``wpilib`` may not be imported from two programs on the RoboRIO.
               If this happens, the second program will attempt to kill the first
               program.

vision file
~~~~~~~~~~~

The first step you need to do is create a file -- let's call it ``vision.py``,
and stick it in the same directory as your ``robot.py`` file. You can also put
it in a subdirectory underneath your robot code, and the robot deploy command
will copy it to the robot.

``vision.py`` must contain some function to be called, let's call it ``main``,
and at the minimum it needs to do the following operations:

* Create a CameraServer instance
* Start capturing from USB
* Get a cvSink object that images can be retrieved from
* Loop and capture images

Here's a full example::
  
    # Import the camera server
    from cscore import CameraServer
    
    # Import OpenCV and NumPy
    import cv2
    import numpy as np
    
    def main():
        cs = CameraServer.getInstance()
        cs.enableLogging()
        
        # Capture from the first USB Camera on the system
        camera = cs.startAutomaticCapture()
        camera.setResolution(320, 240)
        
        # Get a CvSink. This will capture images from the camera
        cvSink = cs.getVideo()
    
        # (optional) Setup a CvSource. This will send images back to the Dashboard
        outputStream = cs.putVideo("Name", 320, 240)
        
        # Allocating new images is very expensive, always try to preallocate
        img = np.zeros(shape=(240, 320, 3), dtype=np.uint8)
        
        while True:
            # Tell the CvSink to grab a frame from the camera and put it
            # in the source image.  If there is an error notify the output.
            time, img = cvSink.grabFrame(img)
            if time == 0:
                # Send the output the error.
                outputStream.notifyError(cvSink.getError());
                # skip the rest of the current iteration
                continue
            
            #
            # Insert your image processing logic here!
            #
            
            # (optional) send some image back to the dashboard
            outputStream.putFrame(img)

robot.py
~~~~~~~~

Next, in the ``robotInit`` function in your ``robot.py`` file, you need to add
the following line::
  
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

Multiple Cameras
----------------

cscore easily supports multiple cameras! Here's a really simple ``vision.py``
file that will get you started streaming two cameras to the FRC Dashboard
program::
    
    from cscore import CameraServer
    
    def main():
        cs = CameraServer.getInstance()
        cs.enableLogging()
        
        usb1 = cs.startAutomaticCapture(dev=0)
        usb2 = cs.startAutomaticCapture(dev=1)
        
        cs.waitForever()

One thing to be careful of: if you get USB Bandwidth errors, then you probably
need to do one of the following:

* Reduce framerate (FPS). The default is 30, but you can get by with 10 or even
  as low as 5 FPS.
* Lower image resolution: you'd be surprised how much you can do with a 160x120
  image!

Sometimes the first and second camera swap!?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
When using multiple USB cameras, Linux will sometimes order the cameras
unpredictably -- so camera 1 will become camera 0. Sometimes.
          
The way to deal with this is to tell cscore to use a specific camera by its path
on the file system. First, identify the cameras ``dev`` paths by using SSH to
access the robot and execute ``find /dev/v4l``. You should see output similar
to this:

.. code-block:: none
  
    /dev/v4l
    /dev/v4l/by-path
    /dev/v4l/by-path/pci-0000:00:1a.0-usb-0:1.4:1.0-video-index0
    /dev/v4l/by-path/pci-0000:00:1d.0-usb-0:1.4:1.2-video-index0
    /dev/v4l/by-id
    ...
  
What you need to do is figure out what paths belong to which camera, and then
when you start the camera server, pass it a name and a path via::

    usb1 = cs.startAutomaticCapture(name="cam1", path='/dev/v4l/by-id/some-path-here')
    usb2 = cs.startAutomaticCapture(name="cam2", path='/dev/v4l/by-id/some-other-path-here')

Generally speaking, if your cameras have unique IDs associated with them (you 
can tell because the by-id path has a random string of characters in it), then
using by-id paths are the best, as they'll always be the same regardness which
port the camera is plugged into.

However, if your camera does NOT have unique IDs associated with them, then
you should use the by-path versions instead. These device paths are unique to
each USB port plugged in. They should be fairly deterministic, but sometimes
with USB hubs they have been known to change.

.. note:: The Microsoft Lifecam cameras commonly used in FRC don't have unique
          IDs associated with them, so you'll want to use the ``by-path``
          versions of the links if you are using two Lifecams.
          

More information
----------------

* The `WPILib screensteps documentation for cscore <http://wpilib.screenstepslive.com/s/4485/m/24194/l/682778-read-and-process-video-cameraserver-class>`_
  may be useful to explain concepts (though some details are different)
* :ref:`robotpy-cscore API documentation <cscore_api>`
