.. _install_revcolor:

robotpy-rev-color install
==========================

Setup (tests)
--------------

If you intend to use the color sensor with your tests or with the
simulator, you'll need to install this package::

    pip3 install -U robotpy-rev-color

Or on Windows::

    py -3 -m pip install -U robotpy-rev-color

Setup (RoboRIO)
-----------------------

You **must** install this package on your robo if you intend to use it.

Python package
~~~~~~~~~~~~~~

So you don't have to compile this yourself, you can download a pre-packaged
version of this from our opkg repository. Use the :ref:`robotpy-installer <install_packages>`
and run the following when connected to the internet::

    py -3 -m robotpy_installer download-opkg robotpy-rev-color

Then, when connected to the roborio's network, run::

    py -3 -m robotpy_installer install-opkg robotpy-rev-color

For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

Usage
------

Using the color sensor is simple. The following code shows how to get the
Red, Green, Blue, and the IR (infrared) values from the sensor, as well as the proximity::

    import wpilib
    from rev.color import ColorSensorV3

    class MyRobot(wpilib.TimedRobot):
        def robotInit(self):
            self.colorSensor = ColorSensorV3(wpilib.I2C.Port.kOnboard)
        
        def robotPeriodic(self):
            # Get the sensor attributes
            color = self.colorSensor.getColor()
            ir = self.colorSensor.getIR()

            # Get the individual components of the color
            red = color.red
            blue = color.blue
            green = color.green

            # Get the approximate proximity of an object
            proximity = self.colorSensor.getProximity()
        
    if __name__ == "__main__":
        wpilib.run(MyRobot)

There's also a more in depth example found at `the example folder for the library <https://github.com/robotpy/robotpy-rev-color/blob/master/examples/read_rgb_values/robot.py>`_