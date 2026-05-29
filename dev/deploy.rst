.. _internal_deploy:

Deploy process details
======================

This page documents the current RobotPy deploy implementation for **SystemCore**.
It is intended for developers who need to understand or manually debug deploy
failures.

The ``robotpy`` command itself is provided by
`robotpy-cli <https://github.com/robotpy/robotpy-cli>`_, but the actual deploy
implementation lives in
`robotpy-installer <https://github.com/robotpy/robotpy-installer>`_. Useful
entry points when reading the code are:

* `robotpy/main.py <https://github.com/robotpy/robotpy-cli/blob/main/robotpy/main.py>`_
* `robotpy_installer/cli_deploy.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/cli_deploy.py>`_
* `robotpy_installer/installer.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/installer.py>`_
* `robotpy_installer/sshcontroller.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/sshcontroller.py>`_
* `robotpy_installer/robot_utils.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/robot_utils.py>`_
* `robotpy_installer/wpilib_preferences.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/wpilib_preferences.py>`_
* `robotpy_installer/cli_deploy_info.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/cli_deploy_info.py>`_
* `robotpy_installer/_pipstub.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/_pipstub.py>`_

.. contents:: :local:

Overview
--------

At a high level, deploy does the following:

#. ``python -m robotpy`` discovers the ``deploy`` subcommand from the
   ``robotpy_cli.YEAR`` entry point group.
#. ``robotpy-installer`` optionally runs local tests.
#. It optionally validates the project's ``pyproject.toml`` requirements against
   the local environment.
#. It resolves the robot address and opens SSH as ``systemcore``.
#. It ensures the robot has the expected Python runtime and the required Python
   packages.
#. It writes ``/home/systemcore/robotCommand``.
#. It uploads code to ``/home/systemcore/py_new`` via SFTP.
#. It atomically swaps ``py_new`` into ``/home/systemcore/py``.
#. It byte-compiles the deployed tree and restarts the ``robot`` systemd
   service.

Deploy pipeline
---------------

Command entry
~~~~~~~~~~~~~

``python -m robotpy`` (or ``robotpy``) loads subcommands from entry points.
If a subcommand requests ``main_file`` or ``project_path``, ``robotpy-cli``
passes the selected robot file and its parent directory into the command.
Deploy therefore operates relative to the directory containing the selected
robot main file.

If you need to create your own ``robotpy`` subcommands, see the
`robotpy-cli README <https://github.com/robotpy/robotpy-cli/blob/main/README.md>`_.
It documents how subcommands are discovered from the ``robotpy_cli.YEAR`` entry
point group and the class structure they must implement.

Deploy also refuses to run if the project path is the user's home directory.
This prevents accidental uploads of an entire home directory.

Local test execution
~~~~~~~~~~~~~~~~~~~~

Unless ``--skip-tests`` is specified, deploy runs:

.. code-block:: shell

   python -m robotpy --main <robot file> test

If tests fail, deploy aborts unless the user explicitly confirms that it should
continue anyway. This means that a deploy failure can happen before any robot
connection or file transfer occurs.

Project requirement checks
~~~~~~~~~~~~~~~~~~~~~~~~~~

Unless ``--no-install`` is used, deploy loads ``pyproject.toml`` and reads the
``[tool.robotpy]`` section. It logs the required package set and, unless
``--no-verify`` is used, verifies that the local environment satisfies those
requirements.

If local packages do not match the project requirements, deploy aborts before
connecting to the robot. The usual recovery is to run:

.. code-block:: shell

   python -m robotpy sync

Robot resolution and SSH
~~~~~~~~~~~~~~~~~~~~~~~~

Deploy connects as:

* username: ``systemcore``
* password: ``systemcore``

Connection settings are loaded from ``.wpilib/wpilib_preferences.json`` in the
project directory. The file is managed by
`wpilib_preferences.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/wpilib_preferences.py>`_
and stores ``teamNumber`` and/or ``robotHostname``.

The address source is, in order:

* ``--robot`` or ``--team`` if specified
* the saved values in ``.wpilib/wpilib_preferences.json``
* an interactive prompt if neither exists

If deploy resolves by team number, ``RobotFinder`` races a small set of likely
addresses and uses the first one that accepts a TCP connection on port 22. The
current probes are:

* ``10.TE.AM.2``
* ``robot.local``
* ``172.28.0.1``
* ``172.30.0.1``

If a hostname matches an SSH alias in ``~/.ssh/config``, or if ``--no-resolve``
is used, deploy skips its own DNS resolution and lets SSH handle the hostname.

Preflight behavior on connect
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``RobotpyInstaller.connect_to_robot`` calls ``ensure_image_version`` and logs
robot disk and memory usage before and after deploy.

.. note::

   ``ensure_image_version`` currently returns immediately, so the image-version
   check is effectively a no-op even though the CLI still exposes
   ``--ignore-image-version``.

Requirement preparation on the robot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before copying user code, deploy may modify the robot runtime environment:

* It removes Java/C++ user programs if present.
* It checks for ``/home/systemcore/.python/bin/python3``.
* It verifies that the robot Python major/minor version matches the installer's
  expected version.
* It checks whether the robot already has the packages required by the project.

If Python is missing or has the wrong version, deploy installs or replaces the
runtime under ``/home/systemcore/.python``. The Python archive is **not**
downloaded on the robot. It must already exist in the local installer cache.

If project packages are missing, deploy creates a venv at
``/home/systemcore/venv`` and installs packages into it from the local RobotPy
cache. The robot does not use the public internet for this step; the installer
starts a temporary local cache server and points pip on the robot at that
server.

The package install path uses a pip stub to force SystemCore-compatible platform
markers such as ``linux-systemcore`` and ``platform.machine() == 'systemcore'``.
That behavior lives in
`_pipstub.py <https://github.com/robotpy/robotpy-installer/blob/main/robotpy_installer/_pipstub.py>`_.

File staging and upload
~~~~~~~~~~~~~~~~~~~~~~~

The deploy command kills the running robot program before starting the copy.
It then writes a new command line to ``/home/systemcore/robotCommand``.

Normal deploy writes a command equivalent to:

.. code-block:: shell

   /home/systemcore/venv/bin/python3 -u -O -m robotpy --main /home/systemcore/py/robot.py run

Debug deploy writes a command equivalent to:

.. code-block:: shell

   /home/systemcore/venv/bin/python3 -u -m robotpy --main /home/systemcore/py/robot.py -v run

The exact main file name depends on the file passed via ``--main``.

User code is not copied directly into ``/home/systemcore/py``. Instead deploy:

#. removes any stale ``/home/systemcore/py_new``
#. copies the project into a temporary local directory
#. writes ``deploy.json`` into that temporary tree
#. uploads the temporary tree as ``/home/systemcore/py_new`` via SFTP
#. replaces ``/home/systemcore/py`` with ``py_new`` on the robot

This ensures if a deploy is interrupted, the previous robot code is still available on the robot.

The upload step intentionally skips some files and directories:

* directories starting with ``.``
* ``__pycache__``
* ``ctre_sim``
* ``venv``
* files ending in ``.pyc``, ``.whl``, ``.ipk``, ``.zip``, ``.gz``, or
  ``.wpilog``
* files whose filename starts with ``.``

Large files are also checked before upload. By default deploy warns about files
larger than 250000 bytes unless ``--large`` is specified.

Activation
~~~~~~~~~~

After the swap, deploy runs:

* ``python -m compileall`` against ``/home/systemcore/py``
* ``sudo sync``
* ``sudo systemctl enable robot``
* ``sudo systemctl start robot``

Stopping the robot uses:

.. code-block:: shell

   sudo systemctl stop robot

That command is shared with other maintenance commands such as undeploy.

Important files and directories
-------------------------------

Local development machine
~~~~~~~~~~~~~~~~~~~~~~~~~

These paths are the most useful when debugging package/download issues:

* ``.wpilib/wpilib_preferences.json``
* ``~/wpilib/YEAR/robotpy``
* ``~/wpilib/YEAR/robotpy/pip_cache``
* ``~/wpilib/YEAR/robotpy/pkg_cache``

``pip_cache`` contains downloaded wheels used for robot package installation.
``pkg_cache`` contains the cached Python runtime archive and related artifacts.

Robot
~~~~~

These are the important robot-side paths:

* ``/home/systemcore/.python``: installed standalone Python runtime
* ``/home/systemcore/venv``: venv used to install RobotPy and project packages
* ``/home/systemcore/robotCommand``: command consumed by the robot service
* ``/home/systemcore/py``: active deployed code
* ``/home/systemcore/py_new``: staging directory during deploy
* ``/home/systemcore/py/deploy.json``: deploy metadata
* ``/home/systemcore/*.jar``: Java user program artifacts removed during cleanup
* ``/home/systemcore/frcUserProgram``: C++ user program artifact removed during
  cleanup

Deploy metadata
---------------

Deploy writes ``deploy.json`` into the uploaded tree. It contains:

.. code-block:: json

   {
    "git-desc": "2022.1-8-gb4fc2aca-dirty",
    "git-hash": "b4fc2aca399810f1fe28faf23314cd422a6db920",
    "git-branch": "feat/working_code",
    "deploy-host": "MyLaptop",
    "deploy-user": "me",
    "deploy-date": "2018-6-10T02:40:55",
    "code-path": "/home/me/robots/MyRobotCode"
   }

Git fields are included only if deploy is running inside a git repository and
``git`` is available locally.

To fetch this file without SSHing manually, use:

.. code-block:: shell

   python -m robotpy deploy-info

That command simply connects over SSH and prints the contents of
``/home/systemcore/py/deploy.json`` if it exists.

Example code:

.. code-block:: python

   #!/usr/bin/env python3

   import os
   import json
   import wpilib.deployinfo


   class MyRobot(wpilib.TimedRobot):
      def __init__(self):
         data = wpilib.deployinfo.getDeployData()

         print(data)

Manual debugging cookbook
-------------------------

SSH access
~~~~~~~~~~

Start by SSHing to the robot as ``systemcore``:

.. code-block:: shell

   ssh systemcore@<robot-hostname-or-ip>

If name resolution is suspicious, connect to the raw address that deploy found.

Is the service running?
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

   sudo systemctl status robot
   sudo journalctl -u robot -n 200 --no-pager

Use these first when the driver station reports ``No Robot Code`` or when deploy
appears to succeed but nothing starts.

What command will the robot service run?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

   cat /home/systemcore/robotCommand

Deploy updates this file before the SFTP upload. If the main filename or Python
path is wrong, this file is the first thing to inspect.

What code actually got deployed?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

   ls -al /home/systemcore
   ls -al /home/systemcore/py
   find /home/systemcore/py -maxdepth 2 -type f | sort
   cat /home/systemcore/py/deploy.json

If a deploy was interrupted, also inspect:

.. code-block:: shell

   ls -al /home/systemcore/py_new

If ``py_new`` exists but ``py`` was not replaced, the failure happened after the
upload but before activation completed.

Is Python installed?
~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

   /home/systemcore/.python/bin/python3 --version
   /home/systemcore/.python/bin/python3 -c "import sys; print(sys.executable)"

If this path is missing, deploy could not install the standalone Python runtime,
or the runtime was manually removed.

Does the venv exist?
~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

   /home/systemcore/venv/bin/python3 --version
   /home/systemcore/venv/bin/python3 -c "import sys; print(sys.prefix)"

If the runtime exists but the venv does not, package installation probably never
ran or the venv was cleared during recovery.

What packages are installed in the venv?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This matches the metadata-based query used by ``robotpy-installer``:

.. code-block:: shell

   /home/systemcore/venv/bin/python3 -c "from importlib.metadata import distributions; import json, sys; json.dump({dist.name: dist.version for dist in distributions()}, sys.stdout, indent=2, sort_keys=True)"

For a quick package list, you can also run:

.. code-block:: shell

   /home/systemcore/venv/bin/python3 -m pip list

Can I run the code manually?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes, but stop the service first so two copies are not fighting each other:

.. code-block:: shell

   sudo systemctl stop robot
   cat /home/systemcore/robotCommand

Then copy the printed command and run it manually. For interactive debugging,
using the debug-style command is often more useful:

.. code-block:: shell

   /home/systemcore/venv/bin/python3 -u -m robotpy --main /home/systemcore/py/robot.py -v run

If your robot entry file is not ``robot.py``, substitute the actual file named in
``robotCommand``.

Useful local-side commands
~~~~~~~~~~~~~~~~~~~~~~~~~~

These commands are often enough to explain a failed deploy without touching the
robot filesystem directly:

.. code-block:: shell

   python -m robotpy deploy-info
   python -m robotpy installer cache location
   python -m robotpy installer cache list
   python -m robotpy sync

If Python itself is missing on the robot and you want to repair that separately
from a full deploy, use:

.. code-block:: shell

   python -m robotpy installer install-python

Notes
-----

A few details that are easy to forget when debugging deploy internals:

* The robot-side package install uses cached wheels and a temporary local HTTP
  server, not a direct pip install from the public internet.
* Direct URL or VCS requirements may need cached wheels produced by
  ``robotpy sync`` before deploy can install them.
* ``deploy-info`` reads the deployed metadata file over SSH; it does not
  inspect local state.
* The current implementation writes only ``robotCommand``. Older references to
  ``robotDebugCommand`` are obsolete.
