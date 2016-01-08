Running Openchain
=================

Deploying Openchain server can be done :ref:`through Docker <docker-deployment>`.

This document explains how to deploy Openchain directly on a machine without using docker.

Prerequisites
-------------

Install the `.NET Command Line Interface <https://dotnet.github.io/docs/getting-started/installing/index.html>`_ . This is cross-platform and runs on Windows, Linux and OS X.

Download the project files
--------------------------

Download the ``project.json`` file and ``config.json`` files from GitHub, then restore the NuGet dependencies. On Linux:

.. code-block:: bash

    $ wget https://raw.githubusercontent.com/openchain/openchain/master/src/Openchain/project.json
    $ wget https://raw.githubusercontent.com/openchain/openchain/master/src/Openchain/Webroot/App_Data/config.json -P Webroot/App_Data
    $ dnu restore

.. note:: On Windows, simply download the files manually using your browser, then run ``dnu restore``.

Run Openchain Server
--------------------

Run openchain server using the following command:

.. code-block:: bash

    $ dnx start

Configuration
-------------

The dependencies section of the ``project.json`` file references the external providers pulled from NuGet:

.. code-block:: json
   :emphasize-lines: 4-6

    "dependencies": {
      "Openchain.Server": "0.5.0-rc1",

      "Openchain.Sqlite": "0.5.0-rc1",
      "Openchain.Validation.PermissionBased": "0.5.0-rc1",
      "Openchain.Anchoring.Blockchain": "0.5.0-rc1"
    },

By defaut, this imports the Sqlite storage engine (``Openchain.Sqlite``), the permission-based validation module (``Openchain.Validation.PermissionBased``), and the Blockchain anchoring module (``Openchain.Anchoring.Blockchain``). Update this list with the modules (and versions) you want to import.

You can then edit the ``Webroot/App_Data/config.json`` file to reference the :ref:`providers you want to use <configuration>`.

.. tip:: For example, if you want to use the ``SQLite`` provider as a storage engine, you will need to make sure the ``Openchain.Sqlite`` module is listed in the dependencies.

Make sure you run ``dnu restore`` again after modifying project.json.

.. note:: The Openchain.Server dependency is the only one that is always required. The version of the ``Openchain.Server`` package is the version of Openchain you will be running.

Updating the target platform
----------------------------

The frameworks section of the project.json file lists the available target frameworks:

.. code-block:: json

    "frameworks": {
      "dnxcore50": {},
      "dnx451": {}
    }

By default .NET Core (cross-platform) and the .NET Framework (Windows only) are both targeted. Some providers run only on a subset of frameworks. In that case, remove the unsupported frameworks from the list to ensure the project runs.
