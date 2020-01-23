.. _install_navx:

robotpy-navx install
====================

Setup (tests/simulator)
-----------------------

If you intend to use robotpy-navx in your *robot tests* or via the *pyfrc
simulator*, you must install this package locally::

    pip3 install -U robotpy-navx

Or on Windows::
    
    py -3 -m pip install -U robotpy-navx

Setup (RoboRIO)
---------------

Even if you have robotpy-navx installed locally, you **must** install it on your
robot **separately**.

Use the RobotPy installation script (comes with the RobotPy download),
and run the following on your computer while connected to the internet::

  py -3 -m robotpy_installer download-opkg robotpy-navx

Then, when connected to the roborio's network, run::

  py -3 -m robotpy_installer install-opkg robotpy-navx

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.
