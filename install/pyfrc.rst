
.. _install_pyfrc:

pyfrc install
=============

Installing pyfrc will install all of the packages needed to help you write and 
test Python-based Robot code on your development computer. These tools include
WPILib, pynetworktables, unit testing support, and the
:ref:`robot simulator <simulator>`.

It is recommended to install using the robotpy meta package:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install robotpy

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install robotpy

code coverage support
---------------------

If you wish to run code coverage testing, then you must install the `coverage <https://pypi.python.org/pypi/coverage>`_
package. It requires a compiler to install from source. However, if you are using
a supported version of Python and a modern version of pip, it may install a
binary wheel instead, which removes the need for a compiler.

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install coverage

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install coverage

If you run into compile errors, then you will need to install a compiler on your
system.

* On Windows you can download the Visual Studio compilers for Python (be sure to
  download the one for your version of Python).
* On macOS it requires XCode to be installed
* On Linux you will need to have python3-dev/python3-devel or a similar package
  installed
