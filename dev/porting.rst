.. _porting:

How to update RobotPy
=====================

Every year, the WPILib team and vendors make changes to their libraries, so
RobotPy needs to be updated to maintain compatibility.

While this is largely a manual process, we
now use a tool called `git-source-track <https://github.com/virtuald/git-source-track>`_
to assist with this process.

.. note:: git-source-track only works on Linux/macOS at this time. If you're
          interested in helping with the porting process and you use Windows,
          file a github issue and we'll try to help you out.

Wrapping C++: robotpy-build
---------------------------

TODO, but see the `robotpy-build <https://robotpy-build.readthedocs.io/en/stable/>`_
documentation for more information.

Pure python porting: using git-source-track
-------------------------------------------

First, you need to checkout the source git repo (usually `allwpilib <https://github.com/wpilibsuite/allwpilib>`_)
and the destination repo next to each other in the same directory like so::

    allwpilib/
    robotpy-commands-v2/

The way git-source-track works is it looks for a comment in the header of each
tracked file that looks like this::

    # validated: 2015-12-24 DS 6d854af athena/java/edu/wpi/first/wpilibj/Compressor.java

This stores when the file was validated to match the original source, initials
of the person that did the validation, what commit it was validated against, and
the path to the original source file.

Finding differences
~~~~~~~~~~~~~~~~~~~

From the destination directory, you can run ``git source-track`` and it
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
the file. If no Python-relevant changes have been made, then you can answer
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

Pure Python Syntax/Style Guide
------------------------------

RobotPy projects use the `black <https://github.com/psf/black>`_
code autoformatter. Black generates pretty good looking code, and it makes it
easier to review incoming pull requests. Before making pull requests, please
install black and run it on the repo you're changing.

Except where it makes sense, developers should try to retain the structure and
naming conventions that the Java implementation of WPILib follows. There are
a few guidelines that can be helpful when translating Java to Python:

* Member variables such as ``m_foo`` should be converted to ``self.foo``
* Private/protected functions (but NOT variables) should start with an underscore
* Always retain original Javadoc documentation, and convert it to the
  appropriate standard Python docstring (see below)

Type hints
~~~~~~~~~~

It is our goal to make all of our code usable with a type checker. Adding type hints
to our code makes it easier for teams to use type checking in their robot code also,
which helps to find bugs earlier. We are not super strict on these however, but
here are some guidelines:

* All function parameters should have type hints
* Attributes should have hints where it's helpful
* Property setters and getters should have hints

Converting javadocs to docstrings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Converting documentation to docstrings is a time consuming and tedious
task, so we automated this to save time. There is a project called
`sphinxify <https://github.com/auscompgeek/sphinxify>`_, which you can
install by running::

    pip install sphinxify

Once installed, it has a tool that you can run and access from your browser.
Just run::

    python3 -m sphinxify server

This will pop up your browser and show a page that can be used for docstring
conversion. The way it works is you copy a Java docstring in the top box
(you can also paste in a function prototype too) and it will output a Python
docstring in
the bottom box.

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
WPILib does not actually utilize most of the interfaces. It may make sense
to implement them as a ``typing.Protocol`` for the purposes of type checking.

Testing
~~~~~~~

We use `pytest <https://docs.pytest.org/>`_ for unit testing.

We don't strictly require unit tests for all new contributions, but they are
useful and highly recommended for all but the simplest changes.

Final thoughts
~~~~~~~~~~~~~~

Before translating WPILib Java code to RobotPy's WPILib, first take some time
and read through the existing RobotPy code to get a feel for the style of the
code. Try to keep it Pythonic and yet true to the original spirit of the code.
Style *does* matter, as students will be reading through this code and it will
potentially influence their decisions in the future.

Remember, all contributions are welcome, no matter how big or small!
