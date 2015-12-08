.. _nginx-deployment:

Deploying Openchain in a production environment
===============================================

In production, it is recommended to proxy the Openchain server behind a reverse proxy server such as Nginx. This architecture enables a number of possibilities:

- Expose Openchain through SSL/TLS
- Host multiple Openchain server instances on the same port
- Change the URL path under which the Openchain server is being exposed
- Route requests to different Openchain instances depending on the host name used

This document explain the few steps necessary to expose Openchain through Nginx.

Install Docker
--------------

Refer to the :ref:`base Docker deployment documentation <docker-deployment>` to find out how to install Docker and Docker Compose.

Pull the Docker images through Docker Compose
---------------------------------------------

Clone the openchain/docker repository from GitHub, and copy the configuration files from the templates provided.

.. code-block:: bash

    git clone https://github.com/openchain/docker.git openchain
    cd openchain
    cp templates/docker-compose-proxy.yml docker-compose.yml
    cp templates/nginx.conf nginx/nginx.conf
    mkdir data
    cp templates/config.json data/config.json

Edit the configuration file (``data/config.json``) as described in the :ref:`base Docker deployment documentation <docker-deployment>`.

You can now start the server:

.. code-block:: bash
    
    docker-compose up -d

.. note:: By default, Nginx will run on port 80.
