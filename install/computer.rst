
.. _install_computer:

Computer Installation
=====================

.. note:: installation via pip typically requires internet access

RobotPy requires Python 3.6/3.7/3.8/3.9 to be installed on your computer. It
is highly recommended to install a 64-bit version of Python.

* `Python for Windows <https://www.python.org/downloads/windows/>`_
* `Python for macOS <https://www.python.org/downloads/mac-osx/>`_

Once you have installed Python, you can use pip to install RobotPy. While it is
possible to install without pip, due to the large number of dependencies this is 
not recommended nor is it supported.

.. tab:: Windows

     .. warning:: On Windows, the `Visual Studio 2019 redistributable <https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads>`_
                 package is required to be installed.


     Run the following command from cmd or Powershell to install the core RobotPy
     packages:

     .. code-block:: sh

         py -3 -m pip install robotpy

     .. seealso:: This command only installs the core RobotPy packages. See additional
                 details for installing :ref:`optional/vendor components
                 <robotpy_components>`

     To upgrade, you can run this:

     .. code-block:: sh

         py -3 -m pip install --upgrade robotpy

     If you don't have administrative rights on your computer, either use
     `virtualenv/virtualenvwrapper-win <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
     or you can install to the user site-packages directory:

     .. code-block:: sh

         py -3 -m pip install --user robotpy


.. tab:: macOS

     .. warning:: Due to WPILib dependencies, RobotPy only supports non-ARM OSX 10.14+

     On a macOS system that has pip installed, just run the following command from the
     Terminal application (may require admin rights):

     .. code-block:: sh

         pip3 install robotpy

     .. seealso:: This command only installs the core RobotPy packages. See additional
                 details for installing :ref:`optional/vendor components
                 <robotpy_components>`

     To upgrade, you can run this:

     .. code-block:: sh

         pip3 install --upgrade robotpy

     If you don't have administrative rights on your computer, either use
     `virtualenv/virtualenvwrapper <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
     or you can install to the user site-packages directory:

     .. code-block:: sh

         pip3 install --user robotpy

.. tab:: Linux

     .. _install_linux:

     Since 2021, RobotPy distributes manylinux binary wheels on PyPI. However,
     installing these requires a distro that has glibc 2.27 or newer, and
     an installer that implements :pep:`600`, such as pip 20.3 or newer.
     You can check your version of pip with the following command:

     .. code-block:: sh

         pip3 --version

     If you need to upgrade your version of pip, it is highly recommended to use a
     `virtual environment <https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/>`_.

     If you have a compatible version of pip, you can simply run:

     .. code-block:: sh

         pip3 install robotpy

     .. seealso:: This command only installs the core RobotPy packages. See additional
                 details for installing :ref:`optional/vendor components
                 <robotpy_components>`

     To upgrade, you can run this:

     .. code-block:: sh

         pip3 install --upgrade robotpy

     The following Linux distributions are known to work, but this list is not
     necessarily comprehensive:

     * Ubuntu 18.04+
     * Fedora 31+
     * Arch Linux

     If you manage to install the packages and get the following error or
     something similar, your system is most likely not compatible with RobotPy::

         OSError: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.22' not found (required by /usr/local/lib/python3.7/dist-packages/wpiutil/lib/libwpiutil.so)

     **source install**

     Alternatively, if you have a C++17 compiler installed, you may be able
     to use pip to install RobotPy from source.

     .. warning:: It may take a very long time to install!

     .. warning::

         Mixing our pre-built wheels with source installs may cause runtime errors.
         This is due to internal ABI incompatibility between compiler versions.

         Our wheels are built on Ubuntu 18.04 with GCC 7.

     If you need to build with a specific compiler version, you can specify them
     using the :envvar:`CC` and :envvar:`CXX` environment variables:

     .. code-block:: sh

         export CC=gcc-7 CXX=g++-7

