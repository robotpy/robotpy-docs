
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
events. For this reason, if you use pynetworktables on the field, we strongly
encourage teams to `ensure every device has a static IP address`.

* Static IPs are ``10.XX.XX.2``
* mDNS Hostnames are ``roborio-XXXX-frc.local`` (don't use these!)

For example, if your team number was 1234, then the static IP to connect to
would be  ``10.12.34.2``.

For information on configuring your RoboRIO and other devices to use static IPs, see the
`WPILib screensteps documentation <https://wpilib.screenstepslive.com/s/4485/m/24193/l/319135-ip-networking-at-the-event>`_.

Server initialization (Robot)
-----------------------------

WPILib automatically starts NetworkTables for you, and no additional
configuration should need to be done.

Client initialization (Driver Station/Coprocessor)
--------------------------------------------------

.. note:: For install instructions, see
          :ref:`pynetworktables installation instructions <install_pynetworktables>`

As of 2017, this is very easy to do. Here's the code::

    from networktables import NetworkTables

    NetworkTables.initialize(server='10.xx.xx.2')

The key is specifying the correct server hostname. See the above section on
robot configuration for this.

.. warning:: NetworkTables does not connect instantly! If you write a script
             that calls ``initialize`` and immediately tries to read from
             NetworkTables, you will almost certainly not be able to read
             any data.
             
             To write a script that waits for the connection, you can use the
             following code::
                 
                import threading
                from networktables import NetworkTables

                cond = threading.Condition()
                notified = [False]

                def connectionListener(connected, info):
                    print(info, '; Connected=%s' % connected)
                    with cond:
                        notified[0] = True
                        cond.notify()

                NetworkTables.initialize(server='10.xx.xx.2')
                NetworkTables.addConnectionListener(connectionListener, immediateNotify=True)

                with cond:
                    print("Waiting")
                    if not notified[0]:
                        cond.wait()
                
                # Insert your processing code here
                print("Connected!")

Theory of operation
-------------------

In its most abstract form, NetworkTables can be thought of as a dictionary that
is shared across multiple computers across the network. As you change the value
of a table on one computer, the value is transmitted to the other side and can
be retrieved there.

The keys of this dictionary **must** be strings, but the values can be numbers,
strings, booleans, various array types, or raw binary. Strictly speaking, the
keys can be any string value, but they are typically path-like values such as
``/SmartDashboard/foo``.

When you call :meth:`NetworkTables.getTable <networktables.NetworkTables.getTable>`,
this retrieves a :class:`NetworkTable <networktables.NetworkTable>` instance
that allows you to perform operations on a specified path::

    table = NetworkTables.getTable('SmartDashboard')
    
    # This retrieves a boolean at /SmartDashboard/foo
    foo = table.getBoolean('foo', True)
    
There is also an concept of subtables::
    
    subtable = table.getSubTable('bar')
    
    # This retrieves /SmartDashboard/bar/baz
    baz = table.getNumber('baz', 1)

As you may have guessed from the above example, once you obtain a NetworkTable
instance, there are very simple functions you can call to send and receive
NetworkTables data.

* To retrieve values, call ``table.getXXX(name, default)``
* To send values, call ``table.putXXX(name, value)``

NetworkTables can also be told to call a function when a particular key in the
table is updated.

Code Samples
~~~~~~~~~~~~

There are many code samples showing various aspects of NetworkTables operation.
See the :ref:`pynetworktables examples <pynetworktables_examples>` page.

.. seealso:: :ref:`NetworkTables API Reference <pynetworktables_api>`

Troubleshooting
~~~~~~~~~~~~~~~

.. seealso:: :ref:`pynetworktables troubleshooting <troubleshooting_nt>`

External tools
--------------

WPILib's OutlineViewer (requires Java) is a great tool for connecting to
networktables and seeing what's being transmitted.

* `Download OutlineViewer <http://first.wpi.edu/FRC/roborio/maven/release/edu/wpi/first/wpilib/OutlineViewer/>`_

WPILib's Shuffleboard (requires Java) is the new (as of 2018) tool to replace
SmartDashboard for creating custom NetworkTables-enabled dashboards.

* `Download Shuffleboard <https://github.com/wpilibsuite/shuffleboard/releases/>`_

WPILib's SmartDashboard (requires Java) is an older tool used by teams to connect
to NetworkTables and used as a dashboard.

* `Download SmartDashboard <http://first.wpi.edu/FRC/roborio/maven/release/edu/wpi/first/wpilib/SmartDashboard/>`_
