Troubleshooting
===============

.. _invalid-namespace-error:

Error "The namespace used in the transaction is invalid"
-------------------------------------------------------------------------------------------------

You might receive this error message when submitting a transaction. You will get this error if the ``root_url`` set in the configuration doesn't match the namespace set by the client in the transaction. Clients will always use the URL they are connected to as the namespace.

This ensures that a transaction is only valid for one specific instance of Openchain, and that it is not possible to reuse a signed transaction on multiple ledgers.

Solution
~~~~~~~~

To solve this, make sure the URL :ref:`set in your configuration file <master-observer-configuration>` (``validator_mode:root_url``) matches the URL that clients use to connect to your Openchain instance. All the components of the URL must match:

* The scheme, e.g.: ``http://endpoint.com/`` vs ``https://endpoint.com/``
* The hostname, e.g.: ``http://127.0.0.1/`` vs ``http://localhost/``
* The port, e.g.: ``http://endpoint.com:80/`` vs ``http://endpoint.com/``
* The path, e.g.: ``http://endpoint.com/path/`` vs ``http://endpoint.com/``

.. important:: Make sure you don't forget the trailing slash, as clients will always include it in the namespace. E.g.: ``https://endpoint.com/`` instead of ``https://endpoint.com``. 