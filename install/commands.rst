
.. _install_commands:

.. _install_commandsv1:

Commands install
================

The WPILib command framework is distributed separately from WPILib, and is
called robotpy-commands-v2. The instructions below discuss installing the
new command framework.

Setup (tests/simulator)
-----------------------

If you intend to use the command framework in your *robot tests* or in the
simulator, you must install this package locally:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install -U robotpy[commands2]

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install -U robotpy[commands2]

Setup (RoboRIO)
---------------

Even if you have robotpy-commands-v2 installed locally, you **must** install it 
on your robot **separately**.

Use the RobotPy installer and run the following on your computer while connected
to the internet:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m robotpy_installer download robotpy[commands2]

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 -m robotpy_installer install robotpy[commands2]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
