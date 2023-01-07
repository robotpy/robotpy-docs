
.. _install_cscore:

robotpy-cscore install
======================

.. note:: cscore is not installed when you ``pip install robotpy``

RoboRIO installation
--------------------

If you have robotpy-installer on your computer, then installing ``robotpy-cscore``
is very simple:

.. tab:: Windows

   .. code-block:: sh

      # While connected to the internet
      py -3 -m robotpy_installer download robotpy-cscore

      # While connected to the network with a RoboRIO on it
      py -3 -m robotpy_installer install robotpy-cscore

.. tab:: Linux/macOS

   .. code-block:: sh
   
      # While connected to the internet
      robotpy-installer download robotpy-cscore

      # While connected to the network with a RoboRIO on it
      robotpy-installer install robotpy-cscore
    
For additional details about running robotpy-installer on your computer, see
the :ref:`robotpy-installer documentation <install_packages>`.

Non-roboRIO installation
------------------------

We now distribute wheels for CSCore on pypi, so you can just use the robotpy-meta
package to install it:

.. tab:: Windows

   .. code-block:: sh

      py -3 -m pip install -U robotpy[cscore]

.. tab:: Linux/macOS

   .. code-block:: sh

      pip3 install -U robotpy[cscore]


Next steps
----------

See our :ref:`cscore documentation <vision>` for examples and deployment thoughts.
