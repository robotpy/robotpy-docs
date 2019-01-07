
.. _deploy:

Deploying to the robot
----------------------

.. contents:: :local:

The easiest way to install code on the robot is to use the deploy command
provided by pyfrc. This command will first run any unit tests on your robot
code, and if they pass then it will upload the robot code to the roboRIO.
Running the tests is really important, it allows you to catch errors in your
code before you run it on the robot. 

1. Make sure you have RobotPy installed on the robot (:ref:`RobotPy install guide <install_robotpy>`)
2. Make sure you have pyfrc installed (:ref:`pyfrc install guide <install_pyfrc>`)
3. Once that is done, you can just run the following command and it will upload the code and start it immediately.

.. code-block:: sh
    
    Windows:   py -3 robot.py deploy

    Linux/macOS: python3 robot.py deploy

You can watch your robot code's output (and see any problems) by using the
netconsole program (you can either use NI's tool, or `pynetconsole <https://github.com/robotpy/pynetconsole>`_.
You can use netconsole and the normal FRC tools to interact with the running
robot code.

If you're having problems deploying code to the robot, check out the
:ref:`troubleshooting section <troubleshooting>`

Immediate feedback via Netconsole
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Note that when you run the deploy command like that, you won't get any feedback
from the robot whether your code actually worked or not. If you want to see the
feedback from your robot without launching a separate NetConsole window, a
really useful option is ``--nc``. This will cause the deploy command to show
your program's console output, by launching a netconsole listener.

.. code-block:: sh

    Windows:   py -3 robot.py deploy --nc
    
    Linux/macOS: python3 robot.py deploy --nc

Skipping Tests
~~~~~~~~~~~~~~

Now perhaps your tests are failing, but you really need to upload the code, and
don't care about the tests. That's OK, you can still upload code to the robot:

.. code-block:: sh

    Windows:   py -3 robot.py deploy --skip-tests

    Linux/macOS: python3 robot.py deploy --skip-tests

Starting deployed code at boot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you wish for the deployed code to be started up when the roboRIO boots up,
you need to make sure that "Disable RT Startup App" is **not** checked in the
roboRIO's web configuration. See the
`FIRST documentation <http://wpilib.screenstepslive.com/s/4485/m/24166/l/262266-roborio-webdashboard>`_
for more information.

Manually deploying code
~~~~~~~~~~~~~~~~~~~~~~~

Generally, you you just use the steps above. However, if you really want to,
then see :ref:`manual_code_deploy`.

Next Steps
~~~~~~~~~~

Let's talk about :ref:`the robot simulator <simulator>` next.

