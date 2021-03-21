.. _install_pwfusion:

robotpy-pwfusion install
========================

Setup (tests/simulator)
-----------------------

.. note:: Playing With Fusion support is only available for 64-bit operating systems.

If you intend to use robotpy-pwfusion in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally. It is recommended to
install using the robotpy meta package:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install -U robotpy[pwfusion]

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install -U robotpy[pwfusion]

Setup (RoboRIO)
---------------

Even if you have robotpy-pwfusion installed locally, you **must** install it on your
robot **separately**. See below.

Python package
~~~~~~~~~~~~~~

You really don't want to compile this yourself, so don't download this from pypi
and install it. Use the RobotPy installer and run the following on your computer
while connected to the internet:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m robotpy_installer download -U robotpy[pwfusion]

.. tab:: Linux/macOS

   .. code-block:: sh

      robotpy-installer download -U robotpy[pwfusion]

Then, when connected to the roborio's network, run:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m robotpy_installer install robotpy[pwfusion]

.. tab:: Linux/macOS

   .. code-block:: sh

      robotpy-installer install robotpy[pwfusion]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
