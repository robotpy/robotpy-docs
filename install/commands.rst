
.. _install_commands:

.. _install_commandsv1:

Commands install
================

As of 2020, the command frameworks are distributed as separate packages. The
latest command framework is available as robotpy-commands-v2, and the
legacy command framework is available in robotpy-commands-v1.

The instructions below discuss installing the new command framework. To install
the legacy framework replace ``commands2`` with ``commands``.

Setup (tests/simulator)
-----------------------

If you intend to use the command framework in your *robot tests* or in the
simulator, you must install this package locally:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m pip install -U robotpy[commands2]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         pip3 install -U robotpy[commands2]


Setup (RoboRIO)
---------------

Even if you have robotpy-commands-v2 installed locally, you **must** install it 
on your robot **separately**.

Use the RobotPy installer and run the following on your computer while connected
to the internet:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download -U robotpy[commands2]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         python3 -m robotpy_installer install robotpy[commands2]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
