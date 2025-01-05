
.. _upgrade_notes:

Upgrade Notes
=============

Nothing particularly major has changed for the 2025 season.

Here are major things that have changed for the 2024 season:

* Python is now an officially supported language 
* robotpy-commands-v2 is now a pure python implementation of the Command framework
* Instead of executing ``robot.py`` directly, we now require that you execute the ``robotpy`` module
* Robot projects now can contain a ``pyproject.toml`` which lists the pip requirements for a particular robot project. The packages can be downloaded via the ``sync`` command, and are now installed at deploy time.
