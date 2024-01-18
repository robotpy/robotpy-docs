robotpy-docs
============

As RobotPy has expanded into more and more projects and components, it has
become increasingly more difficult to introduce all of the pieces to new
users. This is the foundation for a unified documentation site for all
RobotPy projects, in hopes of reducing that confusion.

Now that Python is an officialy supported language, this site still aims to unify the various API documentation for RobotPy projects. However, much of our documentation has been moved to the [WPILib documentation](https://docs.wpilib.org).

All of our documentation is built using Sphinx, and is hosted at
http://robotpy.readthedocs.io/

You can learn more about how to write RST-formatted documentation
at http://sphinx-doc.org/rest.html#rst-primer

Build your own copy of the docs
===============================

First, you need to install the software needed to build the docs. You can
install it by running the following command in the `docs` directory:

    pip3 install -r requirements.txt

If you are on OSX/Linux, you can build the docs using this command:

    make html

On Windows, you need to run this:

    make.bat html

Once this completes, you can open `_build/html/index.html` to view the
generated documentation!
