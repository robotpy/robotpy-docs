
.. _install_pynetworktables:
.. _install_pyntcore:

pyntcore install
================

pyntcore is a python package that allows FRC teams to use Python to communicate
with their robots via NetworkTables. It can be used on your Driver Station, on a
coprocessor such as a Raspberry Pi, or any platform supported by the WPILib team.

.. note:: If you're looking for a pure python implementation of NetworkTables,
          check out the `pynetworktables <https://github.com/robotpy/pynetworktables>`_
          project. It only supports NT2/NT3.

RoboRIO installation
--------------------

``pyntcore`` is installed as part of the core RobotPy installation.

.. tab:: Windows

   .. code-block:: sh

      # While connected to the internet
      py -3 -m robotpy_installer download robotpy

      # While connected to the network with a RoboRIO on it
      py -3 -m robotpy_installer install robotpy

.. tab:: Linux/macOS

   .. code-block:: sh
   
      # While connected to the internet
      robotpy-installer download robotpy

      # While connected to the network with a RoboRIO on it
      robotpy-installer install robotpy[cscore]
    
For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

Non-roboRIO installation
------------------------

Pre-built wheels of ``pyntcore`` can be installed by installing the ``robotpy``
package, or installed separately via pip (shown below):

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install -U pyntcore

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install -U pyntcore

.. tab:: ARM Coprocessor

   .. code-block:: sh

      pip3 install -U --find-links=https://tortall.net/~robotpy/wheels/2023/raspbian/ pyntcore

Getting Started
---------------

See the :ref:`NetworkTables guide <networktables_guide>` to learn more about
using NetworkTables to communicate with your robot.
