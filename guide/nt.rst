
.. _networktables_guide:

Using NetworkTables
===================

NetworkTables is a communications protocol used in FIRST Robotics. It provides a
simple to use mechanism for communicating information between several computers.
There is a single server (typically your robot) and zero or more clients. These
clients can be on the driver station, a coprocessor, or anything else on the
robot’s local control network.

.. contents:: :local:

Robot Configuration
-------------------

.. note:: These notes apply to all languages that use NetworkTables, not just Python

FIRST introduced the mDNS based addressing for the RoboRIO in 2015, and
generally teams that use additional devices have found that while it works at
home and sometimes in the pits, it tends to not work correctly on the field at
events. For this reason, if you use NetworkTables on the field, we strongly
encourage teams to `ensure every device has a static IP address`.

* Static IPs are ``10.XX.XX.2``
* mDNS Hostnames are ``roborio-XXXX-frc.local`` (don't use these!)

For example, if your team number was 1234, then the static IP to connect to
would be  ``10.12.34.2``.

For information on configuring your RoboRIO and other devices to use static IPs, see the
:doc:`WPILib documentation <frc:docs/networking/networking-introduction/ip-configurations>`.

Resources
---------

Installation:

.. seealso:: :ref:`pyntcore installation <install_pyntcore>`

API:

.. seealso:: :ref:`NTCore API Reference <ntcore_api>`

Troubleshooting:

.. seealso:: :ref:`NetworkTables troubleshooting <troubleshooting_nt>`
