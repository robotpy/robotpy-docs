.. _install_adi:

robotpy-adi install
===================

Setup (tests/simulator)
-----------------------

If you intend to use robotpy-adi in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m pip install -U robotpy[adi]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         pip3 install -U robotpy[adi]

.. note:: The vendor does not currently support simulation, but it will install

Setup (RoboRIO)
---------------

Even if you have robotpy-adi installed locally, you **must** install it on your
robot **separately**. Use the RobotPy installer and run the following on your computer
while connected to the internet:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download robotpy[adi]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         python3 -m robotpy_installer download robotpy[adi]

Then, when connected to the roborio's network, run:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer install robotpy[adi]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         python3 -m robotpy_installer install robotpy[adi]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
