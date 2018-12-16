
Other platforms
===============

``robotpy-cscore`` is a great solution for running image processing on a
coprocessor such as the Raspberry Pi or on the Driver Station. However, we 
do not provide precompiled packages at this time and you will need to compile
binary packages for your platform.

Installation
------------

See :ref:`installing robotpy-cscore <install_cscore>`.

.. _vision_other_automatic:

Automatic camera streaming
--------------------------

If you do not wish to modify or process the images from your camera, and *only*
wish to stream a single camera via HTTP to a dashboard, then you can use the
cscore ``__main__`` module to start a CameraServer automatically::

    $ python3 -m cscore

That's it! You can point your browser to that host at port 1181, and you should
see the cscore default webpage.

.. _vision_other_runcustom:

Running a custom cscore program
-------------------------------

.. seealso:: :ref:`vision_code` 

The easiest way to launch a cscore program is via the cscore ``__main__``
module helper. It will automatically configure python logging for you, and 
also provides useful exception handling and NetworkTables configuration
for you automatically. 

You can instruct the ``__main__`` module to run your custom code via a command
line parameter. The parameter is of the form ``FILENAME:FUNCTION``. For
example, if your code was located in a file called ``targeting.py``, and your
function was called ``run``, then you would do:

.. code:: sh

    $ python3 -m cscore targeting.py:run

Examples
~~~~~~~~

See the `intermediate_cameraserver.py` example in the
`robotpy-cscore examples folder <https://github.com/robotpy/robotpy-cscore/tree/master/examples>`_

Launching your script at startup
--------------------------------

TODO: Add section about launching your script at coprocessor startup

Viewing streams via the LabVIEW dashboard or Shuffleboard
---------------------------------------------------------

The LabVIEW dashboard and Shuffleboard both retrieve information about the
camera stream via NetworkTables. If you use the :class:`cscore.CameraServer`
class to manage your streams (which you should!) it will automatically publish
the correct information to NetworkTables. In order for the dashboard program
to receive the NetworkTables information, you need to tell your cscore 
program to connect to a NetworkTables server (your robot), and the robot
code must be actually running.

If you're using the cscore ``__main__`` module to launch your code you can
tell it configure NetworkTables to connect to your robot. Use the
``--robot`` or ``--team`` options:

.. code:: sh

    $ python3 -m cscore --team XXXX

Or if running custom code:

.. code:: sh

    $ python3 -m cscore --team XXXX vision.py:run

If you're writing your own custom code/launcher, at some point you should 
initialize NetworkTables and point it at your robot::

    import networktables
    networktables.startClientTeam(1234)

More information
----------------

* The `WPILib screensteps documentation for cscore <http://wpilib.screenstepslive.com/s/4485/m/24194/l/682778-read-and-process-video-cameraserver-class>`_
  may be useful to explain concepts (though some details are different)
* :ref:`CSCore Troubleshooting <troubleshooting_cscore>`
