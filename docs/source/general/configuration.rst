Openchain Server Configuration
==============================

The configuration of Openchain server is handled through a JSON file named `config.json`. The file is stored under the ``wwwroot/App_Data`` folder.

It is possible to override a configuration value through environment variables. The name of the variable should be the concatenation of all the components of the path, separated by the character ``:``. For example: ``validator_mode:validator:allow_third_party_assets``.

config.json
-----------

Here is the default file:

.. code-block:: json
   
    {
      "enable_transaction_stream": true,

      "storage": {
        "type": "Sqlite",
        "path": "ledger.db"
      },

      // Define transaction validation parameters
      "validator_mode": {
        // Required: The root URL where this instance is hosted
        "root_url": "http://localhost:8080/",
        "validator": {
          "type": "PermissionBased",
          // Enable /p2pkh/<address>/ accounts
          "allow_p2pkh_accounts": true,
          // Enable /asset/p2pkh/<address>/ accounts
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

      // Uncomment this and comment the "validator_mode" section to enable observer mode
      // "observer_mode": {
      //   "upstream_url": ""
      // },

      "anchoring": {
        "type": "blockchain",
        // The key used to publish anchors in the Blockchain
        "key": "",
        "bitcoin_api_url": "https://testnet.api.coinprism.com/v1/"
      }
    }
    
Root section
------------

* ``enable_transaction_stream``: Boolean indicating whether the transaction stream websocket should be enabled on this instance.

``storage`` section
-------------------

* ``type``: Value defining which storage provider to use. Currently, the only supported value is ``Sqlite``, and uses a local Sqlite database to store data.
* ``path``: The path of the Sqlite database, relative to the ``wwwroot/App_Data`` folder. Absolute paths are also allowed, however, make sure the user under which the DNX process is running has write access to the file.

.. _master-observer-configuration:

``validator_mode`` and ``observer_mode`` sections
-------------------------------------------------

These two sections are mutually exclusive. Depending whether the instance is setup in validator mode or observer mode, either the ``validator_mode`` section or ``observer_mode`` section should be present.

In the case of validator mode:

* ``validator_mode:root_url``: The namespace to be used in transactions. If a client submits a transaction with a mismatching namespace, the transaction will be rejected. Clients use the endpoint their are connected to as the namespace, so for transactions to be properly accepted, this value should match the root URL clients are connecting to. If this value is incorrect, clients trying to submit a transaction will receive the :ref:`error "The namespace used in the transaction is invalid" <invalid-namespace-error>`.
* ``validator_mode:validator:type``: The type of validation performed by the Openchain instance when transactions are submitted. The only supported value currently is ``PermissionBased``. See :ref:`this section <ledger-rules>` for more details about the implicit rules of the ``PermissionBased`` mode.
* ``validator_mode:validator:allow_p2pkh_accounts``: Boolean indicating whether :ref:`P2PKH accounts <p2pkh-accounts>` (``/p2pkh/<address>/``) are enabled.
* ``validator_mode:validator:allow_third_party_assets``: Boolean indicating whether :ref:`thrid party issuance accounts <third-party-issuance-accounts>` (``/asset/p2pkh/<address>/``) are enabled.
* ``validator_mode:validator:admin_addresses``: List of strings representing all addresses with admin rights.
* ``validator_mode:validator:version_byte``: The version byte to use when representing a public key using its Bitcoin address representation.

In the case of observer mode:

* ``observer_mode:upstream_url``: The endpoint URL of the upstream instance to connect to. Transactions will be replicated using this endpoint.

``anchoring`` section
---------------------

This section contains configuration settings relative to publishing an anchor to preserve data integrity.

- ``type``: Value defining which anchoring mode to use. Currently, the only supported value is ``blockchain``, and publishes a cumulative hash of the database onto a Bitcoin-compatible blockchain.
- ``key``: The private key to use (in WIF format) as the signing address for the proof of publication transactions.
- ``bitcoin_api_url``: The Coinprism API endpoint to use to list unspent outputs and broadcast the signed transaction. Valid values include:

    - https://api.coinprism.com/v1/ (Bitcoin mainnet)
    - https://testnet.api.coinprism.com/v1/ (Bitcoin testnet)
