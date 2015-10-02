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

Query a record (``/value``)
---------------------------

Query the value and version of a record given its key.

**Method**: GET

Inputs
~~~~~~

Inputs are passed through the query string as URL encoded parameters.

==============  ==============
Parameter name  Description
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

Transaction stream (``/stream``)
--------------------------------

.. include:: /common/stub.txt

Query an account (``/query/account``)
-------------------------------------

.. include:: /common/stub.txt

Query a transaction (``/query/transaction``)
--------------------------------------------

.. include:: /common/stub.txt

Query record in account and subaccounts (``/query/subaccounts``)
----------------------------------------------------------------

.. include:: /common/stub.txt
