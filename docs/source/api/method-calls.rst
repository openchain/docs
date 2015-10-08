.. _http-api:

Method calls
============

The Openchain server exposes an HTTP API that can be used to interact with the data. The URL of an operation is constructed from the base URL of the endpoint, and concatenating it with the relative path of the operation being called.

For example, if the base URL is ``https://www.openchain.org/endpoint/``, for calling the ``/value`` operation (query a record), the full URL should be ``https://www.openchain.org/endpoint/value``.

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

.. include:: /common/stub.txt

Query a transaction (``/query/transaction``)
--------------------------------------------

.. include:: /common/stub.txt

Query record in account and subaccounts (``/query/subaccounts``)
----------------------------------------------------------------

.. include:: /common/stub.txt
