
.. _install_pynetworktables:

pynetworktables install
=======================

pynetworktables is a python package that allows FRC teams to use Python to
communicate with their robots via NetworkTables. It should work without issues
on your Driver Station, on a coprocessor such as a Raspberry Pi, or anywhere
else that you might install Python.

pynetworktables requires Python 2.7 or 3.3 or greater to be installed on the
system that you'll be using it on.

.. note:: You only need to install pynetworktables separately if you're using
          it on a system that doesn't already have pyfrc or RobotPy installed
          on it (such as a coprocessor)
          
Install via pip on Windows
--------------------------

The latest versions of Python on Windows come with pip, but you may need to
install it by hand if you're using an older version. Once pip is installed,
run the following command from the command line:

.. code-block:: sh

    Python 2.7: py -2 -m pip install pynetworktables
    Python 3.x: py -3 -m pip install pynetworktables
    
To upgrade, you can run this:

.. code-block:: sh

    Python 2.7: py -2 -m pip install --upgrade pynetworktables
    Python 3.x: py -3 -m pip install --upgrade pynetworktables

If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper-win <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    Python 2.7: py -2 -m pip install --user pynetworktables
    Python 3.x: py -3 -m pip install --user pynetworktables
    
Install via pip on macOS/Linux
------------------------------

On a Linux or macOS system that has pip installed, just run the following command
from the Terminal application (may require admin rights):

.. code-block:: sh

    Python 2.7: pip install pynetworktables
    Python 3.x: pip3 install pynetworktables
    
To upgrade, you can run this:

.. code-block:: sh

    Python 2.7: pip install --upgrade pynetworktables
    Python 3.x: pip3 install --upgrade pynetworktables
    
If you don't have administrative rights on your computer, either use
`virtualenv/virtualenvwrapper-win <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_, or
or you can install to the user site-packages directory:

.. code-block:: sh

    Python 2.7: pip -m pip install --user pynetworktables
    Python 3.x: pip3 -m pip install --user pynetworktables
    
Manual install (without pip)
----------------------------

.. note:: It is highly recommended to use pip for installation when possible

You can download the source code, extract it, and run this::
    
    python setup.py install

If you are using Python 2.7, you will need to also install the
`monotonic package from pypi <https://pypi.python.org/pypi/monotonic>`_

Getting Started
---------------

See the :ref:`NetworkTables guide <networktables_guide>` to learn more about
using pynetworktables to communicate with your robot.
