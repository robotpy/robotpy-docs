
.. _install_commands:

.. _install_commandsv1:

Commands install
================

As of 2020, the command frameworks are distributed as separate packages. The
legacy command framework is available in robotpy-commands-v1.

.. note:: The new command framework is not yet available for RobotPy 2020.

Setup (tests/simulator)
-----------------------

If you intend to use the command framework in your *robot tests* or in the
simulator, you must install this package locally::

    pip3 install -U robotpy-commands-v1

Or on Windows::
    
    py -3 -m pip install -U robotpy-commands-v1

Setup (RoboRIO)
---------------

Even if you have robotpy-commands-v1 installed locally, you **must** install it on your
robot **separately**.

Use the RobotPy installer and run the following on your computer while connected
to the internet::

  py -3 -m robotpy_installer download-opkg commands-v1

Then, when connected to the roborio's network, run::

  py -3 -m robotpy_installer install-opkg commands-v1

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
