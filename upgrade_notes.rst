
.. _upgrade_notes:

Upgrade Notes
=============

Here are major things that have changed for the 2023 season:

* See below for NT4 related changes
* cscore is much easier to build now, and we distribute wheels for Windows/Linux/macOS
* robotpy-commands is no longer supported, only commands v2
* There are two new packages: robotpy-apriltag and robotpy-wpinet

NetworkTables 4 (NT4)
---------------------

pynetworktables will not be upgraded to support NT4. We will still fix bugs in
its NT3 support, but we recommend all users to switch to pyntcore.

pyntcore now provides a similar API to networktables, but it lives in the
:py:mod:`ntcore` package now.


Linux specific notes
--------------------

Linux requires Ubuntu 22.04 or a distribution with an equivalent (or newer)
glibc installation. See :ref:`linux installation page <install_linux>` for
more information.
