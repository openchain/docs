.. _modules:

Openchain modules
=================

Openchain uses an extensible architecture where modules can be swapped in and out depending on the functionality needed. Modules are selected by:

- Referencing the appropriate package in the ``project.json`` file. Packages are then pull automatically from NuGet.
- Referencing the module in ``config.json``.

This document lists the available modules, and relevant packages.

Storage engines
---------------

Storage engines are core components responsible for storing the transaction chain and records.

=================  ===========================================================================   ================================================================  ====================================================================
Provider           Module                                                                        Description                                                       Maintainer
=================  ===========================================================================   ================================================================  ====================================================================
``SQLite``         `Openchain.Sqlite <https://www.nuget.org/packages/Openchain.Sqlite>`_         Stores the chain in a local Sqlite database.                      `Coinprism <https://github.com/openchain/openchain>`_
``MsSQL``          `Openchain.SqlServer <https://www.nuget.org/packages/Openchain.SqlServer>`_   Stores the chain in a SQL Server database.                        `Coinprism <https://github.com/openchain/openchain>`_
``MongoDB``        Openchain.MongoDb                                                             Stores the chain in a MongoDB database.                           `@fluce <https://github.com/openchain/mongodb-storage>`_
=================  ===========================================================================   ================================================================  ====================================================================

Validation engines
------------------

Anchoring media
---------------
