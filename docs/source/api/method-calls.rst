.. _http-api:

Method calls
============

The Openchain server exposes an HTTP API that can be used to interact with the data. The URL of an operation is constructed from the base URL of the endpoint, and concatenating it with the relative path of the operation being called.

For example, if the base URL is ``https://www.openchain.org/endpoint/``, for calling the ``/record`` operation (query a record), the full URL should be ``https://www.openchain.org/endpoint/record``.

Submit a transaction (``/submit``)
----------------------------------

Submits a transaction for validation.

**Method**: POST

Inputs
~~~~~~

The input is a JSON document passed as part of the body of the request.

The format of the JSON document is the following:

.. code-block:: json

    {
        "mutation": "<string>",
        "signatures": [
            {
                "pub_key": "<string>",
                "signature": "<string>"
            }
        ]
    }

Description of the payload:

- ``mutation``: The hex-encoded mutation. The mutation is serialized using the :ref:`Mutation Protocol Buffers schema <data-structures-mutation>`.
- ``signatures``: An array of documents with two properties, ``pub_key`` and ``signature``.

    - ``pub_key``: The hex-encoded public key used to sign.
    - ``signature``: The hex-encoded signature of the hash of the mutation.

Signing Process
^^^^^^^^^^^^^^^

For producing the signatures:

1. Serialize the mutation using the :ref:`Mutation Protocol Buffers schema <data-structures-mutation>`.
2. Hash the mutation byte string using double SHA256.
3. Sign it with the relevant private key using Secp256k1. The matching public key must be submitted along with the signature.

.. important:: You must submit the exact byte string as obtained after step 1. If it modified, the hash won't match and the signature will then be invalid.

Outputs
~~~~~~~

The output is a JSON document passed as part of the body of the response.

.. code-block:: json

    {
        "transaction_hash": "<string>"
    }
    
The ``transaction_hash`` field contains the hex-encoded hash of the full transaction.

Query a record (``/record``)
----------------------------

Query the value and version of a record given its key.

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

==============  ==============
``key``         The hex-encoded key of the record being queried.
==============  ==============

Output
~~~~~~

The output is a JSON document passed as part of the body of the response.

The format of the JSON document is the following:

.. code-block:: json

    {
        "key": "<string>",
        "value": "<string>",
        "version": "<string>"
    }

The fields are the following:

* ``key``: The hex-encoded key of the record.
* ``value``: The hex-encoded value of the record.
* ``version``: The hex-encoded version of the record.

.. _stream-api-endpoint:

Transaction stream (``/stream``)
--------------------------------

**Method**: GET

This endpoint is a WebSocket endpoint. It can be used to receive all the newly confirmed transaction in real-time.

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

==============  ==============
``from``        **(optional)** The hex-encoded hash of the last transaction to resume from. If omitted, it will start from the first transaction.
==============  ==============

Output
~~~~~~

The output is a WebSocket binary stream.

Each message in the stream is the :ref:`serialized transaction <data-structures-transaction>`.

Query an account (``/query/account``)
-------------------------------------

Query all the ACC records at a given path (non-recursively).

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

=================  ==============
``account``        The path to query for.
=================  ==============

Output
~~~~~~

The output is a JSON array passed as part of the body of the response.

The format of the JSON array is the following:

.. code-block:: json

    [
        {
            "account": "<string>",
            "asset": "<string>",
            "balance": "<string>",
            "version": "<string>"
        }
    ]

The fields of each item of the array are the following:

* ``account``: The path of the record.
* ``value``: The asset ID of the record (the record name).
* ``balance``: The balance for that asset ID at that path.
* ``version``: The hex-encoded version of the record.

Query a transaction (``/query/transaction``)
--------------------------------------------

Retrieve a transaction given the hash of the mutation.

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

=================  ==============
``mutation_hash``  The hex-encoded hash of the mutation represented by the transaction.
``format``         The output format (``raw`` or ``json``).
=================  ==============

Output
~~~~~~

The output is a JSON document passed as part of the body of the response.

The format of the JSON document depends on the ``format`` argument:

1. ``raw`` output format (default):

.. code-block:: json

    {
        "raw": "<string>"
    }

The ``raw`` property contains the serialized transaction.
    
2. ``json`` output format

.. code-block:: json

    {
        "transaction_hash": "<string>",
        "mutation_hash": "<string>",
        "mutation": {
            "namespace": "<string>",
            "records": [
                {
                    "key": "<string>",
                    "value": "<string>",
                    "version": "<string>"
                }
            ]
        },
        "timestamp": "<string>",
        "transaction_metadata": "<string>"
    }

Query a specific version of a record (``/query/recordversion``)
---------------------------------------------------------------

Retrieve a specific version of a record.

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

=================  ==============
``key``            The hex-encoded record key.
=================  ==============

Output
~~~~~~

The output is a JSON document passed as part of the body of the response.

The format of the JSON document is the following:

.. code-block:: json

    {
        "key": "<string>",
        "value": "<string>",
        "version": "<string>"
    }

The fields are the following:

* ``key``: The hex-encoded key of the record.
* ``value``: The hex-encoded value of the record.
* ``version``: The hex-encoded version of the record.

If the record version doesn't exist, HTTP code 404 will be returned by the server.

Query all mutations that have affected a record (``/query/recordmutations``)
----------------------------------------------------------------------------

Retrieve all the mutations that have affected a given record.

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

=================  ==============
``key``            The key of the record of which mutations are being retrieved.
=================  ==============

Output
~~~~~~

The output is a JSON document passed as part of the body of the response.

The format of the JSON document is the following:

.. code-block:: json

    [
        {
            "mutation_hash": "<string>"
        }
    ]

The output is a list representing all the mutation hashes of the mutations that have affected the key represented by the ``key`` argument.

Query records in an account and its subaccounts (``/query/subaccounts``)
------------------------------------------------------------------------

.. include:: /common/stub.txt
