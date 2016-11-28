.. _docker-deployment:

Openchain Server Docker deployment
==================================

Openchain Server is cross platform and can be deployed as a `DNX application <https://dotnet.readthedocs.org/en/latest/dnx/overview.html>`_ on Windows, OS X and Linux. However, to simplify dependency management and homogenize deployment of Openchain, we are shipping it as a Docker image.

This document explains the few steps necessary to have the Openchain server running. Refer to the `next section <https://docs.openchain.org/en/latest/general/running-openchain.html>`_ to deploy Openchain directly.

Install Docker
--------------

.. note:: This assumes you are running Linux. Use `these instructions <http://docs.docker.com/installation/windows/>`_ if you are running Windows, and `these instructions <http://docs.docker.com/installation/mac/>`_ if you are running OS X.

First, install Docker if you don't have it:

.. code-block:: bash

    wget -qO- https://get.docker.com/ | sh

Then install Docker Compose:

.. code-block:: bash

    apt-get install python-pip
    pip install -U docker-compose

Install Openchain Server
------------------------

Clone the openchain/docker repository from GitHub, and copy the configuration files from the templates provided.

.. code-block:: bash

    git clone https://github.com/openchain/docker.git openchain
    cd openchain
    cp templates/docker-compose-direct.yml docker-compose.yml
    mkdir data
    cp templates/config.json data/config.json

Now, edit the configuration file (``data/config.json``):

.. code-block:: bash

    nano data/config.json

Set the ``instance_seed`` setting to a random (non-empty) string.

.. code-block:: json
   :emphasize-lines: 5
   
    [...]
      // Define transaction validation parameters
      "validator_mode": {
        // Required: A random string used to generate the chain namespace
        "instance_seed": "",
        "validator": {
    [...]
    
.. note:: By default, the Openchain server will run on port 8080. You can edit ``docker-compose.yml`` if you want to run on a non-default port.

You can now start the server:

.. code-block:: bash
    
    docker-compose up -d

This will start the Openchain server in the background. To check that the server is running properly, check the docker logs:

.. code-block:: bash

    docker logs openchain-server

You should not see any error:

.. code-block:: bash

    info: General[0]
        [2016-07-10 18:20:10Z] Starting Openchain v0.7.0
    info: General[0]
        [2016-07-10 18:20:11Z]
    info: General[0]
        [2016-07-10 18:20:13Z] Stream subscriber disabled
    info: General[0]
        [2016-07-10 18:20:13Z] Anchoring disabled
    Hosting environment: Production
    Content root path: /openchain
    Now listening on: http://0.0.0.0:8080
    Application started. Press Ctrl+C to shut down.

.. tip:: You can also run the Openchain Docker container in the foreground by running ``docker-compose up`` and omitting the ``-d`` switch.

Now that you have a server running, you can connect to the server with a :ref:`client <openchain-client>`.

Configuring admin keys
----------------------

Use the :ref:`client <openchain-client>` to generate a seed, and derive it into an address. Once you have an address, you can use it as an admin address on your server instance. To do so, update ``data/config.json`` and add it to the ``admin_addresses`` list:

.. code-block:: json
   :emphasize-lines: 3
   
    // ...
    "admin_addresses": [
      "<your_address_here>"
    ],
    // ...

.. tip:: Follow :ref:`these steps <create-info-record>` to configure the ``info`` record on your new instance. The ``info`` record is used by clients connecting to the instance to receive additional information about the instance they are connecting to.

Controlling the server
----------------------

To restart the server, use::

    docker-compose restart
    
To stop it, use::

    docker-compose stop
