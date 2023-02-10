.. _install_pathplannerlib:

robotpy-pathplannerlib install
==============================

Setup (tests/simulator)
-----------------------

If you intend to use robotpy-pathplannerlib in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally. It is recommended to
install using the robotpy meta package:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install -U robotpy[pathplannerlib]

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install -U robotpy[pathplannerlib]

Setup (RoboRIO)
---------------

Even if you have robotpy-pathplannerlib installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Use the RobotPy installer and run the following on your computer
while connected to the internet:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m robotpy_installer download robotpy[pathplannerlib]

.. tab:: Linux/macOS

   .. code-block:: sh

      robotpy-installer download robotpy[pathplannerlib]

Then, when connected to the roborio's network, run:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m robotpy_installer install robotpy[pathplannerlib]

.. tab:: Linux/macOS

   .. code-block:: sh

      robotpy-installer install robotpy[pathplannerlib]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
