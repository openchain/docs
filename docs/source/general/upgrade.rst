Upgrading Openchain server
==========================

To upgrade an Openchain deployment done :ref:`through Docker <docker-deployment>`, run the following commands:

.. code-block:: bash

    git pull
    docker-compose restart
    
.. note:: If the new version you are upgrading to include a configuration file schema change, don't forget to update the configuration file before restarting Openchain.