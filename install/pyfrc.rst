
.. _install_pyfrc:

pyfrc install
=============

Installing pyfrc will install all of the packages needed to help you write and 
test Python-based Robot code on your development computer. These tools include
WPILib, pynetworktables, unit testing support, and the
:ref:`robot simulator <simulator>`.

pyfrc requires Python 3.6 or greater to be installed on your computer. On
Windows, the `Visual Studio 2019 redistributable <https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads>`_
package is required to be installed.

* `Python for Windows <https://www.python.org/downloads/windows/>`_
* `Python for macOS <https://www.python.org/downloads/mac-osx/>`_

Install via pip on Windows
--------------------------

.. note:: pip typically requires internet access

The easiest installation is by using pip. pip is installed by default with 
Python 3.5+. Run the following command from the command line to install pyfrc
via the RobotPy meta package:

.. code-block:: sh

    py -3 -m pip install robotpy

To upgrade, you can run this:

.. code-block:: sh

    py -3 -m pip install --upgrade robotpy

If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper-win <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    py -3 -m pip install --user robotpy

Install via pip on macOS
------------------------

.. note:: pip typically requires internet access

Due to WPILib requirements, RobotPy only supports OSX 10.14+

The easiest installation is by using pip. pip is installed by default with
Python 3.6+. On a macOS system that has pip installed, just run the
following command from the Terminal application (may require admin rights):

.. code-block:: sh

    pip3 install robotpy

To upgrade, you can run this:

.. code-block:: sh

    pip3 install --upgrade robotpy

If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    pip3 install --user robotpy

.. _install_linux:

Install via pip on Linux
------------------------

.. note:: pip typically requires internet access

Unfortunately, RobotPy does not distribute manylinux compatible wheels 
for Linux, so you will most likely need a C++17 compiler installed for
instaling RobotPy components. WPILib is compiled on Ubuntu 18.04, and it
is likely that on Linux RobotPy can only be used on a system that has an
equivalent version of glibc or newer.

The following Linux distributions are known to work, but this list is not
necessarily comprehensive:

* Ubuntu 18.04+
* Fedora 31
* Arch Linux

binary install
~~~~~~~~~~~~~~

If you have Ubuntu 18.04 or a Linux distribution with a compatible glibc,
you can try using our `precompiled wheels to install RobotPy <https://www.tortall.net/~robotpy/wheels/2020/linux_x86_64/>`_.

.. code-block:: sh

    pip3 install --find-links https://www.tortall.net/~robotpy/wheels/2020/linux_x86_64/ pyfrc

To upgrade, you can run this:

.. code-block:: sh

    pip3 install --find-links https://www.tortall.net/~robotpy/wheels/2020/linux_x86_64/ --upgrade pyfrc

If you get the following error or something similar, your system is
most likely not compatible with RobotPy.

    OSError: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.22' not found (required by /usr/local/lib/python3.7/dist-packages/wpiutil/lib/libwpiutil.so)`

source install
~~~~~~~~~~~~~~

Alternatively, if you have a compatible C++ compiler installed, you can use
pip to install RobotPy from source.

.. warning:: It may take a very long time to install!

.. code-block:: sh

    pip3 install pyfrc

To upgrade, you can run this:

.. code-block:: sh

    pip3 install --upgrade pyfrc

If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    pip3 install --user pyfrc

Manual install (without pip)
----------------------------

While this is possible to do, due to the large number of dependencies this is 
not recommended nor is it supported.
	
code coverage support
---------------------

If you wish to run code coverage testing, then you must install the `coverage <https://pypi.python.org/pypi/coverage>`_
package. It requires a compiler to install from source. However, if you are using
a supported version of Python and a modern version of pip, it may install a
binary wheel instead, which removes the need for a compiler.

.. code-block:: sh

    Windows:   py -3 -m pip install coverage

    Linux/macOS: pip3 install coverage
    
If you run into compile errors, then you will need to install a compiler on your
system.

* On Windows you can download the Visual Studio compilers for Python (be sure to
  download the one for your version of Python).
* On macOS it requires XCode to be installed
* On Linux you will need to have python3-dev/python3-devel or a similar package
  installed
