========
Settings
========

Django Tailwind comes with preconfigured settings. You can override them in the ``settings.py`` of your Django project.

``TAILWIND_APP_NAME``
=====================

This defines the Tailwind theme Django app containing your Tailwind CSS styles. I prefer to name such an app ``'theme'``. You should generate the app during the installation phase by running the following command:

.. code-block:: bash

    python manage.py tailwind init

Please refer to the :doc:`Installation </installation>` section for more information on the installation process.

``TAILWIND_DEV_MODE`` (Depreciated)
===================================

Determines whether the ``browser-sync`` snippet is added to the page via the ``{% tailwind_css %}`` tag. It is set to ``False`` by default, so if you use legacy pre-``3.1.0`` configuration and rely on ``browser-sync``, add ``TAILWIND_DEV_MODE=True`` to your ``settings.py``.

``NPM_BIN_PATH``
================

This defines a path to the ``npm`` executable in your system.

.. note::

    Tailwind CSS requires you to have Node.js installed on your machine. Node.js is a JavaScript runtime that allows running JavaScript code outside a browser. Most of the current frontend tools depend on Node.js.

    If you don’t have Node.js installed on your machine, please follow installation instructions from the `official Node.js page <https://nodejs.org/>`_.

The default value is:

.. code-block:: python

   NPM_BIN_PATH = 'npm'

Please note, that on Windows the path might look completely different (pay attention to “backslashes” in the path):

.. code-block:: python

   NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"

``TAILWIND_CSS_PATH``
=====================

This defines a path to the generated Tailwind CSS stylesheet. If you have created a theme app via the ``python manage.py tailwind init`` command, chances are you don’t need to change this value.

However, if you integrated Tailwind CSS in another way or want to use a CDN version of the bundle, you might want to change the path.

The default value is:

.. code-block:: python

   TAILWIND_CSS_PATH = 'css/dist/styles.css'
