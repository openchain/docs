Openchain Server Docker deployment
==================================

The supported deployment method for Openchain server is through Docker.

Prerequisites
-------------

Openchain Server is cross platform, and will run on Windows, OS X and Linux. These installation instructions should work on any system able to run Docker, however they have only been tested on Ubuntu 14.04.

Install Docker
--------------------------

First, install Docker if you don't have it:

.. code-block:: bash

    wget -qO- https://get.docker.com/ | sh

Then install Docker Compose:

.. code-block:: bash

    sudo apt-get install python-pip
    sudo pip install -U docker-compose
    
Install Openchain Server
------------------------

Clone the openchain/docker repository from GitHub:

.. code-block:: bash

    git clone https://github.com/openchain/docker.git openchain-docker
    cd openchain-docker
    cp templates/server.yml docker-compose.yml
    mkdir data
    cp templates/config.json data/config.json

Now, edit the configuration file (``data/config.json``). Edit ``root_url`` to reflect the URL at which the server is hosted. You can use ``http://localhost:8080`` if you are just testing locally.

.. code-block:: json
   :emphasize-lines: 11, 12
   
    {
      "enable_transaction_stream": true,

      "storage": {
        "type": "SQLite",
        "path": "ledger.db"
      },

      // Define transaction validation parameters
      "master_mode": {
        // Required: The root URL where this instance is hosted
        "root_url": "http://localhost:8080",
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
    
    docker-compose up

This will run the server in the foreground. You should see log messages confirming the server is running.

.. code-block:: bash

    openchain_1 | info    : [General] Transaction validation mode enabled (Master mode)
    openchain_1 | info    : [General] Stream subscriber disabled
    openchain_1 | Application started. Press Ctrl+C to shut down.

You can now stop the execution and run it in the background:

.. code-block:: bash
    
    docker-compose up -d
    
Now that you have a server running, you can connect to the server with a client.

Configure admin keys
--------------------

Use the client to generate a seed, and derive it into an address. Once you have an address, you can use it as an admin address on your server instance. To do so, update ``data/config.json`` and add it to the `admin_addresses` list:

.. code-block:: json
    
    // [...]
    "admin_addresses": [
      "<your_address_here>"
    ],
    // [...]

Controlling the server
----------------------

To restart the server, use::

    docker-compose restart
    
To stop it, use::

    docker-compose stop