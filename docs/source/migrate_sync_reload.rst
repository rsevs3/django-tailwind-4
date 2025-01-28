====================================================
Migrating from browser-sync to django-browser-reload
====================================================

.. note::
   This documentation is here in the unlikely event that someone is migrating from a ``django-tailwind`` installation using ``browser-sync``, and would like to migrate to ``django-tailwind-4``. At the forking of the original project it was at version 3.8.0 and it has been over 3 years since ``django-browser-refresh`` was the page refresh option, so it is unlikely that anyone will need to do this.

Starting with version ``3.1.0``, Django-Tailwind is dropping support for Node.JS-based ``browser-sync`` and switching to Django-based ``django-browser-reload`` as the primary solution for seamless page and asset updates during development.

If you generated Tailwind CSS before ``3.1.0``, I strongly suggest you to upgrade your project.

If you don’t want to upgrade and are happy with ``browser-sync``, make sure that you have ``TAILWIND_DEV_MODE=DEBUG`` set in your ``settings.py`` file.

Please follow the following migration steps.

#. Upgrade Django-Tailwind to its latest version:

   .. code-block:: bash

      pip install django-tailwind --upgrade

#. Add ``django_browser_reload`` to ``INSTALLED_APPS`` in ``settings.py``:

   .. code-block:: python

      INSTALLED_APPS = [
        # other Django apps
        'tailwind',
        'theme',
        'django_browser_reload'
      ]

   Staying in ``settings.py``, add the middleware:

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

#. Now, ``cd`` to your ``TAILWIND_APP_NAME static_src`` directory. In my case, ``TAILWIND_APP_NAME`` is ``theme``, thus I ``cd`` to ``theme/static_src``:

   .. code-block:: bash

      cd theme/static_src

#. Remove ``bs.config.js`` file from the ``theme/static_src`` directory.

#. Open ``package.json`` file in ``theme/static_src`` directory, and remove the following commands under ``scripts:sync``, ``dev:sync``, ``dev``.

#. Staying in ``package.json``, rename the ``dev:tailwind`` command under ``scripts`` to ``dev``. That’s the only command we need for development from now on.

#. Staying in the same directory, uninstall dependencies we don’t need anymore, run the following command:

   .. code-block:: bash

      npm uninstall browser-sync nodemon npm-run-all

#. Run ``python manage.py tailwind start`` to make sure everything runs properly.
