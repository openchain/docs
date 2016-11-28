.. _ledger-rules:

Default ledger rules
====================

Global rules
------------

A transaction is made of multiple :ref:`record mutations <data-structures-records>`. ``ACC`` record mutations are subject to a balancing rule. The balancing rule works as follow:

1. For every ``ACC`` record, the delta between the previous balance and the new proposed balance is calculated.
2. The sum of all deltas **per asset type** is calculated.
3. For every asset type, the sum must be equal to zero.

This ensures every asset creation and destruction is recorded through an account in the system. This means however that at least one account must be able to have a negative balance. Usually, a special account is used to do so, and the ability to create a negative balance on an account requires special permissions.

.. tip:: :ref:`Third-party asset issuance accounts <third-party-issuance-accounts>` are allowed to have negative balances.

.. _aliases:

Aliases (``/aka/<name>/``)
--------------------------

Openchain has the ability to define aliases for accounts. This simplifies the user experience as users no longer have to remember a base-58 random string of characters.

To do so, clients should understand the following syntax as a valid account path: ``@<name>``, and turn it internally into ``/aka/<name>/``.

Example
~~~~~~~

If a user wants to send funds to the following account::

    @bank
    
The client application should convert it internally into::

    /aka/bank/

.. _goto-records:

Goto records (``goto``)
-----------------------

Goto records are special :ref:`DATA records <data-record>` instructing the client application to use a different account.

Goto records must have the special name ``goto``.

When a client application sends funds to a path, it must first look for a ``DATA`` record named ``goto``. If it exists, the client application must use the path defined as the value of the record instead.

Example
~~~~~~~

If a user wants to send funds to the following account::

    /account/alpha/
    
The client must first check the existance of a record with the following key::

    /account/alpha/:DATA:goto

If the record doesn't exist, nothing happens and funds are sent to ``/account/alpha/``. If the record exists, assuming its value is::

    /account/beta/

Then funds are sent instead to ``/account/beta/``.

.. note:: It is possible and recommended for security reasons that the client application uses a :ref:`check-only record <check-only-record>` with the goto record to make sure the value of the goto record is still valid and hasn't changed when the transaction is validated.

.. _asset-metadata:

Asset definition record (``asdef``)
-----------------------------------

It is important to be able to associate information with an asset type so that users have the right expectations about it.

The asset definition record can be used to record this information. The asset definition record is a ``DATA`` record with the special name ``asdef``. In addition, it must be placed under the same path as the asset it is attached to.

Example
~~~~~~~

In order to associate information with the asset represented by path ``/asset/gold/``, the following record must be set::

    /asset/gold/:DATA:asdef
    
The value of the record is a UTF-8 string representing a JSON document with the following schema:

.. code-block:: json

    {
        name: '<string>',
        name_short: '<string>',
        icon_url: '<string>'
    }
    
The definition of these fields are the following:

* ``name``: The full name of the asset (e.g.: ``U.S. Dollar``, ``Gold Ounce``).
* ``name_short``: The short name of the asset. This is used to denominate amounts (e.g.: ``USD``, ``XAU``)
* ``icon_url``: The URL to an icon representing the asset.

.. _ledger-info-record:

Ledger info record (``info``)
-----------------------------

Each Openchain instance can store a :ref:`DATA record <data-record>` named ``info`` at the root path (``/``). In other words, the record key should be ``/:DATA:info``.

The info record exposes meta-information about the ledger itself. The value must be a JSON document with the following schema:

.. code-block:: json

    {
        name: '<string>',
        validator_url: '<string>',
        tos: '<string>',
        webpage_url: '<string>'
    }

The definition of these fields are the following:

* ``name``: The name of the Openchain instance.
* ``validator_url``: The URL of the main validator for this Openchain instance.
* ``tos``: The terms of service of the Openchain instance.
* ``webpage_url``: A link to user-readable content where users can get more information about this Openchain instance.

.. _p2pkh-accounts:

Pay-To-Pubkey-Hash accounts (``/p2pkh/<address>/``)
---------------------------------------------------

Pay-To-Pubkey-Hash accounts are special accounts with implicit permissions. Signing a transaction spending funds from this account or any sub-account requires the private key corresponding to ``<address>``.

This automatically works with any account of that format, where ``<address>`` is a valid base-58 address.

.. note:: ``<address>`` is a base-58 address constructed in the same way a Bitcoin address for the same private and public key would be.

.. _third-party-issuance-accounts:

Third-party asset issuance accounts (``/asset/p2pkh/<address>/``)
-----------------------------------------------------------------

Third-party asset issuance accounts are special accounts with implicit permissions. The owner of the private key corresponding to <address> can sign transactions spending funds from this account. Funds have to be of the asset type ``/asset/p2pkh/<address>``. Also, this address is authorized to have a negative balance. This means it is possible to use this address as the issuance source of asset type ``/asset/p2pkh/<address>``.

This automatically works with any account of that format, where ``<address>`` is a valid base-58 address.

.. note:: ``<address>`` is a base-58 address constructed in the same way a Bitcoin address for the same private and public key would be.
