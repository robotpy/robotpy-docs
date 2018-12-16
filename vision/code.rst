
.. _vision_code:

Custom Image processing
=======================

vision file
-----------

.. warning:: If you merely wish to display a single camera stream and do not
             want to process the images, do **NOT** use this code. Instead,
             see one of the following sections about automatic streaming:

             * :ref:`RoboRIO automatic streaming <vision_roborio_automatic>`
             * :ref:`non-RoboRIO automatic streaming <vision_other_automatic>`

The first step you need to do is create a file -- let's call it ``vision.py``.
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

This code will work both on a RoboRIO and on other platforms. The exact mechanism
to run it differs depending on whether you're on a RoboRIO or a coprocessor:

* :ref:`RoboRIO <vision_roborio_runcustom>`
* :ref:`Other <vision_other_runcustom>`

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
* :ref:`CSCore Troubleshooting <troubleshooting_cscore>`
