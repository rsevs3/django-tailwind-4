============
Installation
============

Step-by-step instructions
=========================

#. Install the ``django-tailwind-4`` package via ``pip``:

   .. code-block:: shell

      pip install django-tailwind-4

   If you want to use automatic page reloads during development (see steps 10-12 below) use the ``[reload]`` extras, which installs the ``django-browser-reload`` package in addition:

   .. code-block:: shell

      python -m pip install 'django-tailwind-4[reload]'

   Alternatively, you can install the latest development version via:

   .. code-block:: shell

      python -m pip install git+https://github.com/rsevs3/django-tailwind-4.git

#. Add ``'tailwind'`` to ``INSTALLED_APPS`` in ``settings.py``:

   .. code-block:: python

      INSTALLED_APPS = [
        # other Django apps
        'tailwind',
      ]

#. Create a Tailwind CSS compatible Django app, I like to call it ``theme``:

   .. code-block:: shell

      python manage.py tailwind init

#. Add your newly created ``'theme'`` app to ``INSTALLED_APPS`` in ``settings.py``:

   .. code-block:: python

      INSTALLED_APPS = [
        # other Django apps
        'tailwind',
        'theme'
      ]

#. Register the generated ``'theme'`` app by adding the following line to ``settings.py`` file:

   .. code-block:: python

      TAILWIND_APP_NAME = 'theme'

#. Install Tailwind CSS dependencies, by running the following command:

   .. code-block:: shell

      python manage.py tailwind install

#. The Django Tailwind comes with a simple ``base.html`` template located at ``your_tailwind_app_name/templates/base.html``. 

   You can always extend or delete it if you already have a layout.
   
#. If you are not using the ``base.html`` template that comes with Django Tailwind, add ``{% tailwind_css %}`` to the base.html template:

   .. code-block:: html+jinja

      {% load static tailwind_tags %}
      ...
      <head>
         ...
         {% tailwind_css %}
         ...
      </head>

   The ``{% tailwind_css %}`` tag includes Tailwind’s stylesheet.

#. Let’s also add and configure ``django_browser_reload``, which takes care of automatic page and css refreshes in the development mode. Add it to ``INSTALLED_APPS`` in ``settings.py``:

   .. code-block:: python

      INSTALLED_APPS = [
        # other Django apps
        'tailwind',
        'theme',
        'django_browser_reload'
      ]

#. Staying in ``settings.py``, add the middleware:

   .. code-block:: python

      MIDDLEWARE = [
        # ...
        "django_browser_reload.middleware.BrowserReloadMiddleware",
        # ...
      ]

   The middleware should be listed after any that encode the response, such as Django’s ``GZipMiddleware``. The middleware automatically inserts the required script tag on HTML responses before ``</body>`` when ``DEBUG`` is ``True``.

#. Include ``django_browser_reload`` URL in your root ``url.py``:

   .. code-block:: python

      from django.urls import include, path
      urlpatterns = [
          ...,
          path("__reload__/", include("django_browser_reload.urls")),
      ]

#. Finally, you should be able to use Tailwind CSS classes in HTML. Start the development server by running the following command in your terminal:

   .. code-block:: shell

      python manage.py tailwind start

   Check out the :doc:`Usage </usage>` section for information about the production mode.

Optional configurations
=======================

Content (formerly Purge) rules configuration
____________________________________________

Previously with Tailwind v3, you had to specify all of the files to wach for Tailwind classes. This is no longer the case and Tailwind v4 will scan all of the files and folders in the path that is specified. You do not need to specify scanning ``*.html``, ``*.js`` and ``*.py`` files individually. ``django-tailwind-4`` is set to scan all of the folders in you root Django project folder.

If you would like more granual control of the folders that are being watched, you can change the configuration in ``{my-tailwind-app-name}/static_src/src/styles.css`` and modify like this example:

.. code-block:: css

   @import "tailwindcss" source(none);

   @source "../../../templates";

Note that you may need to adjust those paths to suit your specific project layout.

For more information about configuring Tailwind

Configuration of the path to the npm executable
_______________________________________________

Tailwind CSS requires Node.js to be installed on your machine. Node.js is a JavaScript runtime that allows you to run JavaScript code outside the browser. Most (if not all) of the current frontend tools depend on Node.js.

If you don’t have Node.js installed on your machine, please follow installation instructions from the `official Node.js page <https://nodejs.org/>`_.

Sometimes (especially on Windows), the Python executable cannot find the ``npm`` binary installed on your system. In this case, you need to set the path to the ``npm`` executable in settings.py file manually (Linux/Mac):

.. code-block:: python

   NPM_BIN_PATH = '/usr/local/bin/npm'

On Windows, you may have ``npm`` on ``$PATH`` but it’s ``npm.cmd`` rather than ``npm``. (You can call it from the terminal because ``$PATHEXT`` contains ``.cmd``.) If so, please override the default ``NPM_BIN_PATH = 'npm'``:

.. code-block:: python

   NPM_BIN_PATH = 'npm.cmd'

Alternatively (and for maximum reliability), you can use a fully qualified path. It might look like this:

.. code-block:: python

   NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"

Please note that the path to the ``npm`` executable may be different for your system. To get the ``npm`` path, try running the command ``which npm`` in your terminal. (On Windows, please try ``where npm`` or ``Get-Command npm``)

If you share codes with others, you can search ``$PATH`` (and ``$PATHEXT`` on Windows) dynamically in settings.py:

.. code-block:: python

   from shutil import which
   NPM_BIN_PATH = which("npm")
