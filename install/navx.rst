.. _install_navx:

robotpy-navx install
====================

Setup (tests/simulator)
-----------------------

If you intend to use robotpy-navx in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally.  It is recommended to
install using the robotpy meta package:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m pip install -U robotpy[navx]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         pip3 install -U robotpy[navx]

Setup (RoboRIO)
---------------

Even if you have robotpy-navx installed locally, you **must** install it on your
robot **separately**. Use the RobotPy installer and run the following on your 
computer while connected to the internet:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer download -U robotpy[navx]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer download -U robotpy[navx]

Then, when connected to the roborio's network, run:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: sh

         py -3 -m robotpy_installer install robotpy[navx]

   .. group-tab:: Linux/macOS

      .. code-block:: sh

         robotpy-installer install robotpy[navx]

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
