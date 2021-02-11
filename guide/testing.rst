.. _unit_tests:

Unit testing robot code
=======================

.. warning:: Testing is currently still broken for RobotPy 2020

pyfrc comes with robot.py extensions that support testing robot code using the
py.test python testing tool. To run the unit tests for your robot, just run your
robot.py with the following arguments:

.. tab:: Windows

   .. code-block:: sh

      py -3 robot.py test

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 robot.py test

Your tests must be in a directory called 'tests' either next to robot.py, or in
the directory above where robot.py resides. See 'samples/simple' for an example
test program that starts the robot code and runs it through autonomous mode and
operator mode.

Builtin unit tests
------------------

pyfrc comes with testing functions that can be used to test basic
functionality of just about any robot, including running through a 
simulated practice match. As of pyfrc 2016.1.1, to add these standardized
tests to your robot code, you can run the following:

.. tab:: Windows

   .. code-block:: sh

      py -3 robot.py add-tests

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 robot.py add-tests

Running this command creates a directory called 'tests' if it doesn't already
exist, and then creates a file in your tests directory called pyfrc_test.py,
and put the following contents in the file::

    from pyfrc.tests import *
    
Unlike previous years, all tests work on all types of robots now.

As of pyfrc 2015.0.2, the ``--builtin`` option allows you to run the builtin
tests without needing to create a tests directory.

Writing your own test functions
-------------------------------

Often it's useful to create custom tests to test specific things that the
generic tests aren't able to test. When running a test, py.test will look for
functions in your test modules that start with 'test\_'. Each of these functions
will be ran, and if any errors  occur the tests will fails. A simple test
function might look like this::

    def two_plus(arg):
        return 2 + arg

    def test_addition():
        assert two_plus(2) == 4

The `assert` keyword can be used to test whether something is True or False,
and if the condition is False, the test will fail.

Pytest supports something called a 'fixture', which allows you to add an
argument to your test function and it will call the fixture and pass the
result to your test function as that argument. pyfrc has a custom pytest
plugin that it uses to provide this special functionality to your tests.

For more information:

* `RobotPy example code <https://github.com/robotpy/examples>`_
* `py.test documentation <http://pytest.org/latest/example/index.html>`_

code coverage for tests
-----------------------

pyfrc supports measuring code coverage using the `coverage.py <http://nedbatchelder.com/code/coverage/>`_
module. This feature can be used with any robot.py commands and provide coverage
information.

For example, to run the 'test' command to run unit tests:

.. tab:: Windows

   .. code-block:: sh

      py -3 robot.py coverage test

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 robot.py coverage test

Or to run coverage over the simulator:

.. tab:: Windows

   .. code-block:: sh

      py -3 robot.py coverage sim

.. tab:: Linux/macOS

   .. code-block:: sh

      python3 robot.py coverage sim

Running code coverage while the simulator is running is nice, because you
don't have to write unit tests to make sure that you've completely covered
your code. Of course, you *should* write unit tests anyways... but this is
good for developing code that needs to be run on the robot quickly and you
need to make sure that you tested everything first.

When using the code coverage feature, what actually happens is robot.py gets
executed *again*, except this time it is executed using the coverage module.
This allows coverage.py to completely track code coverage, otherwise any
modules that are imported by robot.py (and much of robot.py itself) would not
be reported as covered. 

.. note:: There is a py.test module called pytest-cov that is supposed to allow
   you to run code coverage tests. However, I've found that it doesn't work
   particularly well for me, and doesn't appear to be maintained anymore.

.. note:: For some reason, when running the simulation under the code coverage
   tool, the output is buffered until the process exits. This does not happen
   under py.test, however. It's not clear why this occurs. 

Next Steps
----------

Learn more about some :ref:`best_practices` when creating robot code. 
