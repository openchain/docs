Upgrading Openchain server
==========================

To upgrade an Openchain deployment done :ref:`through Docker <docker-deployment>`, run the following commands:

.. code-block:: bash

    git reset --hard
    git pull
    cp templates/docker-compose-direct.yml docker-compose.yml
    docker-compose build
    docker-compose restart
    
.. note:: If the new version you are upgrading to includes a configuration file schema change, don't forget to update the configuration file before restarting Openchain.