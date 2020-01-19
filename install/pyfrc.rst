
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

The easiest installation is by using pip. pip is installed by default with Python
3.5+. Run the following command from the command line:

.. code-block:: sh

    py -3 -m pip install pyfrc

To upgrade, you can run this:

.. code-block:: sh

    py -3 -m pip install --upgrade pyfrc

If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper-win <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    py -3 -m pip install --user pyfrc

Install via pip on macOS/Linux
------------------------------

.. note:: pip typically requires internet access

The easiest installation is by using pip. pip is installed by default with
Python 3.5 (though on Linux it may not be installed). On a Linux or macOS system
that has pip installed, just run the following command from the Terminal
applciation (may require admin rights):

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
