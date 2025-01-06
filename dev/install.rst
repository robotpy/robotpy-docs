
Developer Installation
======================

This is a discussion of practical tips for developing RobotPy code that I personally use so I can get things done quicker. Some of these apply to writing robot code too, but that is not what this page is directed towards.

Tools
-----

It's best to use the latest version of Python available, but sometimes I lag behind a version because I'm lazy. I also have virtualenvs that use the oldest version of Python that RobotPy supports just in case I need to diagnose things there.

I use Microsoft Visual Studio Code with the pylance extension, and I always have `basic type checking enabled <https://github.com/microsoft/pyright/discussions/1975>`_ in the pyright settings.

I always set up a virtualenv for robotpy development (using `virtualenvwrapper <https://virtualenvwrapper.readthedocs.io/en/latest/>`_ to manage my environments), and typically I create a new one per year, and have separate environments that contain only locally built RobotPy projects and ones that only contain artifacts from PyPI. In particular, on Linux I find that I can't compile/link against packages that were built on PyPI because of compiler differences. YMMV.

If you're doing C++ development, make sure to install ``ccache``. I have 64GB of RAM and a fast SSD, more cores are better.

Pure Python
-----------

For all the pure python pieces of robotpy, you can follow standard python package development practices. I find installing packages in editable mode makes life a lot easier because you can just edit and test in place:

.. code:: shell

    cd project
    pip install --no-build-isolation -e .

Not much else to say here.

C++
---

For the C++ portions of RobotPy (which is all the WPILib and vendor code), I do all of my development on macOS and Linux. You can do it on Windows too, but I can't help you much there.

All of our C++ code uses robotpy-build to automatically generate wrappers around various robot code. It's worth learning about how it works (`read the docs here <https://robotpy-build.readthedocs.io/en/stable/>`_) if you're going to try to change anything -- but the basic idea is that ``pyproject.toml`` has a list of C++ header files that are being wrapped, ``gen/*.yml`` has configuration files that modify the code generation for the header files, and sometimes there are C++ files sprinkled in that do various things too.

robotpy-build wraps everything in pybind11. If you're going to do anything major, you'll probably want to read the pybind11 documentation (it's great!).

When compiling, I have these environment variables set to speed things up:

.. code:: shell

    export RPYBUILD_PARALLEL=1      # tell robotpy-build to compile in parallel
    export CC="ccache gcc"          # tell setuptools to use ccache
    export CXX="ccache g++"
    export GCC_COLORS=1
    export RPYBUILD_DEBUG=1         # tell robotpy-build to add debug symbols
    export RPYBUILD_PP_GCC=1        # tell robotpy-build

You're not supposed to do this anymore, but when I want to install my robotpy-build based projects in editable mode, I `always` run ``python setup.py develop -N`` instead of using pip or uv. It's shorter, and setuptools and pip keep changing how things work -- and also, this always uses the same build directory (instead of a random one) so this allows ``ccache`` to work correctly too. In 2026 we should be moving away from setuptools and then we can use modern python practices instead.

When doing development on mostrobotpy, there's an ``rdev.sh`` script that can be used to do most common build related things, see ``--help`` for details.
