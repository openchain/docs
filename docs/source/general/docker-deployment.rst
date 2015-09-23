Openchain Server Docker deployment
==================================

Openchain Server is cross platform and can be deployed as a `DNX application <https://dotnet.readthedocs.org/en/latest/dnx/overview.html>`_ on Windows, OS X and Linux. However, to simplify dependency management and homogenize deployment of Openchain, we are shipping it as a Docker image.

This document explain the few steps necessary to have the Openchain server running.

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

    git clone https://github.com/openchain/docker.git openchain-docker
    cd openchain-docker
    cp templates/server.yml docker-compose.yml
    mkdir data
    cp templates/config.json data/config.json

Now, edit the configuration file (``data/config.json``):

.. code-block:: bash

    nano data/config.json

Edit ``root_url`` to reflect the URL at which the server is hosted. You can use ``http://localhost:8080/`` if you are just testing locally.

.. code-block:: json
   :emphasize-lines: 12
   
    {
      "enable_transaction_stream": true,

      "storage": {
        "type": "Sqlite",
        "path": "ledger.db"
      },

      // Define transaction validation parameters
      "master_mode": {
        // Required: The root URL where this instance is hosted
        "root_url": "http://localhost:8080/",
        "validator": {
          "type": "PermissionBased",
          "allow_third_party_assets": true,
          // Base-58 addresses that must have admin rights
          "admin_addresses": [
          ],
          // Special issuer, in the following format:
          //
          // {
          //   "path": "",
          //   "addresses": [
          //     ""
          //   ]
          // }
          "issuers": [
          ],
          "version_byte": 111
        }
      },

      // Uncomment this and comment the "master_mode" section to enable observer mode
      // "observer_mode": {
      //   "master_url": ""
      // },

      "anchoring": {
        "type": "blockchain",
        // The key used to publish anchors in the Blockchain
        "key": "",
        "bitcoin_api_url": "https://testnet.api.coinprism.com/v1/"
      }
    }
    
.. note:: By default, the Openchain server will run on port 8080. You can edit ``docker-compose.yml`` if you want to run on a non-default port.

You can now start the server:

.. code-block:: bash
    
    docker-compose up -d

To check that the server is running properly, check the docker logs:

.. code-block:: bash

    docker logs openchain-server

You should not see any error:

.. code-block:: bash

    info    : [General] Transaction validation mode enabled (Master mode)
    info    : [General] Stream subscriber disabled
    Application started. Press Ctrl+C to shut down.
    
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

Controlling the server
----------------------

To restart the server, use::

    docker-compose restart
    
To stop it, use::

    docker-compose stop