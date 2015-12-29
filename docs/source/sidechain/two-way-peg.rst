Using the 2-Way Peg
===================

Prerequisite
------------

Define a gateway path and asset path for the proxy-token. It is recommended but not required for these two values to be the same. For example, for a Bitcoin pegged asset, you can use ``/asset/btc/``.

Configuring the permissions
---------------------------

Create a DATA record called ``acl`` under the gateway path (e.g. ``/asset/btc/``):

.. code-block:: json

    [{
        "subjects": [
            { "addresses": [ "<gateway-address>" ], "required": 1 }
        ],
        "permissions": {
            "account_negative": "Permit",
            "account_spend": "Permit",
            "account_modify": "Permit",
            "account_create": "Permit",
            "data_modify": "Permit"
        }
    }]

Create a DATA record called ``acl`` in the ``out`` folder under the gateway path (e.g. ``/asset/btc/out/``)

.. code-block:: json

    [{
        "subjects": [
            { "addresses": [ ], "required": 0 }
        ],
        "record_name": "<asset-path>",
        "record_name_matching": "Exact",
        "permissions": {
            "account_create": "Permit"
        }
    }]

Configuring a gateway alias
---------------------------

Optionally, configure an alias for the gateway. For example, if the gateway path is ``/asset/btc/`` and the alias should be called ``@redeem``, create a DATA record called ``goto`` under ``/aka/redeem/`` containing the value ``/asset/btc/out``.