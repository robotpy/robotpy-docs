
.. _running_robot_code:

Running robot code
==================

Now that you've created your first Python robot program, you probably want to
know how to run the code. The process to run a python script is slightly
different for each operating system.

How to execute the script
-------------------------

.. tab:: Windows

   On Windows, you will typically execute your robot code by opening up the
   command prompt (cmd), changing directories to where your robot code is,
   and then running this:

   .. code-block:: sh

      py -3 -m robotpy

.. tab:: Linux/macOS

   On Linux/macOS, you will typically execute your robot code by opening up the
   Terminal program, changing directories to where your robot code is, and
   then running this:

   .. code-block:: sh

      python3 -m robotpy

Commands
--------
    
When you run your code without additional arguments, you'll see an error message
saying something like ``robot.py: error: the following arguments are required:
command``. RobotPy tools install various commands that you can run from your
robot code. To discover the various features that are installed, you can use the
``--help`` command:

.. tab:: Windows

   .. code-block:: batch

      py -3 -m robotpy --help

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 -m robotpy --help

.. note:: RobotPy supports an extension mechanism that allows advanced users the
          ability to create their own custom ``robotpy`` commandline options.
          For more information, see :ref:`robotpy_extension_options`

Next steps
----------

There are two ways you can run the code: on the robot, and on the simulator:

* :ref:`deploy`
* :ref:`simulator`

.. note:: If you're just starting out with RobotPy, you'll probably find it faster
          (and more instructive) to start playing with your code in the simulator
          before you actually deploy it to a robot.
