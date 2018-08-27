.. _porting:

Updating RobotPy source code to match WPILib
============================================

Every year, the WPILib team makes improvements to WPILib, so RobotPy needs to be
updated to maintain compatibility. While this is largely a manual process, we
now use a tool called `git-source-track <https://github.com/virtuald/git-source-track>`_
to assist with this process.

.. note:: git-source-track only works on Linux/OSX at this time. If you're
          interested in helping with the porting process and you use Windows,
          file a github issue and we'll try to help you out.

Using git-source-track
----------------------

First, you need to checkout the git repo for `allwpilib <https://github.com/wpilibsuite/allwpilib>`_
and the RobotPy WPILib next to each other in the same directory like so::
    
    allwpilib/
    robotpy-wpilib/

The way git-source-track works is it looks for a comment in the header of each
tracked file that looks like this::
    
    # validated: 2015-12-24 DS 6d854af athena/java/edu/wpi/first/wpilibj/Compressor.java
    
This stores when the file was validated to match the original source, initials
of the person that did the validation, what commit it was validated against, and
the path to the original source file.

Finding differences
~~~~~~~~~~~~~~~~~~~

From the `robotpy-wpilib` directory, you can run ``git source-track`` and it
will output all of the configured files and their status. The status codes
include:

* ``OK``: File is up to date, no changes required
* ``OLD``: The tracked file has been updated, ```git source-track diff FILENAME`` can
  be used to show all of the git log messages and associated diffs.
* ``??``: The tracked file has moved or has been deleted
* ``IGN``: The file has explicitly been marked as do not track
* ``--``: The file is not currently being tracked

Sometimes, commits are added to WPILib which only change comments, formatting,
or mass file renames -- these don't change the semantic content of the file,
so we can ignore those commits. When identified, those commits should be added
to ``devtools/exclude_commits``.

Looking at differences
~~~~~~~~~~~~~~~~~~~~~~

Once you've identified a file that needs to be updated, then you can run::
    
    git source-track diff FILENAME
    
This will output a verbose git log command that will show associated commit
messages and the diff output associated with that commit for that specific file.
Note that it will only show the change for that specific file, it will
not show changes for other files (use ``git log -p COMMITHASH`` in the 
original source directory if you want to see other changes).

After running ``git source-track diff`` it will ask you if you want to validate
the file. If no Python-significant changes have been made, then you can answer
'y' and the validation header will be updated.

Adding new files
~~~~~~~~~~~~~~~~

Unfortunately, git-source-track doesn't currently have a mechanism that allows
it to identify new files that need to be ported. We need to do that manually.

Dealing with RobotPy-specific files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We don't need to track those files; ``git source-track set-notrack FILENAME``
takes care of it.

After you finish porting the changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you've finished making the appropriate changes to the Python code, then
you should update the validation header in the source file. Thankfully,
there's a command to do this::
    
    git source-track set-valid FILENAME
    
It will store the current date and the tracked git commit.

Additionally, if you answer 'y' after running ``git source-track diff FILENAME``,
then it will update the validation header in the file.

HAL Changes
-----------

RobotPy uses the WPILib HAL API to talk to the hardware. This API is not
guaranteed to be stable, so every year we have to update it. There are several
pieces to this that need to be updated.

Each WPILib build publishes new header files and library files to their website,
and in ``hal-roborio/hal_impl/distutils.py`` there is code to download and
extract the package. The version number of the HAL release we want to use
needs to be updated there.

Once that's updated, you can run the unit tests to see if there are any breaking
HAL changes. If they fail and there are changes to the HAL, there are two places
that HAL code updates need to be made aside from the code in WPILib that uses
the HAL:

* ``hal-base/hal/functions.py`` - contains ctypes signatures for each HAL
  function, which must match the functions in the HAL headers. Running the
  robotpy-wpilib unit tests will fail if the functions do not match.
* ``hal-sim/hal_impl/functions.py`` - contains simulated HAL definitions,
  which need to have the same parameter names as defined in hal-base

Additionally, ``devtools/hal_fix.sh`` is a script that can be used to detect
errors in the HAL and print out the correct HAL definitions for a given function
or generate empty python stubs (via the ``--stubs`` argument). Use ``--help``
for more information on the capabilities of this tool.

Syntax/Style Guide
------------------

Except where it makes sense, developers should try to retain the structure and
naming conventions that the Java implementation of WPILib follows. There are
a few guidelines that can be helpful when translating Java to Python:

* Member variables such as ``m_foo`` should be converted to ``self.foo``
* Private/protected functions (but NOT variables) should start with an underscore
* Always retain original Javadoc documentation, and convert it to the
  appropriate standard Python docstring (see below)

Converting javadocs to docstrings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There is an HTML page in devtools called ``convert_javadoc.html`` that you can
use. The way it works is you copy a Java docstring in the top box (you can also
paste in a function prototype too) and it will output a Python docstring in
the bottom box. When adding new APIs that have documentation, this tool is
invaluable and will save you a ton of time -- but feel free to improve it!

This tool has also been converted to a command line application called
`sphinxify <https://github.com/auscompgeek/sphinxify>`_,
which you can install by running::

    pip install sphinxify

Enums
~~~~~

Python 3.4 and up have an enum module. In the past, we did not use it to
implement the enums found in the Java WPILib, however, we are slowly moving
towards its use, starting with moving existing enums to IntEnum.
New enums should preferably use a plain Enum (although this may be up for discussion).
See `robotpy-wpilib issue #78 <https://github.com/robotpy/robotpy-wpilib/issues/78>`_ for details.
For example::
    
    class SomeObject:
    
        class MyEnum(enum.IntEnum):
            VALUE1 = 1
            VALUE2 = 2

Many WPILib classes define various enums, see existing code for example
translations.

Synchronized
~~~~~~~~~~~~

The Python language has no equivalent to the Java ``synchronized`` keyword.
Instead, create a ``threading.RLock`` instance object called ``self.lock``, and
surround the internal function body with a ``with self.lock:`` block::
  
    def someSynchronizedFunction(self):
        with self.lock:
            # do something here...

Interfaces
~~~~~~~~~~

While we define the various interfaces for documentation's sake, the Python
WPILib does not actually utilize most of the interfaces.

Final thoughts
~~~~~~~~~~~~~~

Before translating WPILib Java code to RobotPy's WPILib, first take some time
and read through the existing RobotPy code to get a feel for the style of the
code. Try to keep it Pythonic and yet true to the original spirit of the code.
Style *does* matter, as students will be reading through this code and it will
potentially influence their decisions in the future.

Remember, all contributions are welcome, no matter how big or small!
