.. _dynamic-permissions:

Dynamic permissions
===================

Openchain supports an implicit permission layout through :ref:`P2PKH accounts <p2pkh-accounts>` (``/p2pkh/<address>/``) and :ref:`thrid party issuance accounts <third-party-issuance-accounts>` (``/asset/p2pkh/<address>/``). It is also possible to dynamically define permissions by submitting transactions modifying a special record: the ``acl`` record.

Access Control Lists
--------------------

Permissions are applied to a specific path. To apply an access control list to a path, set the ``acl`` record under that path. It must be a ``DATA`` record. The value is a JSON file.

For example, when trying to set the permissions to the path ``/users/alice/``, the following record must be set: ``/users/alice/:DATA:acl``.

Schema
------

The schema of the JSON file that the record contains is the following:

.. code-block:: json

    [
        {
            "subjects": [
                {
                    "addresses": [ "<string>" ],
                    "required": <integer>
                }
            ],
            "recursive": <boolean>,
            "record_name": "<string>",
            "record_name_matching": "<record-matching-type>",
            "permissions": {
                "account_negative": "<permission>",
                "account_spend": "<permission>",
                "account_modify": "<permission>",
                "account_create": "<permission>",
                "data_modify": "<permission>"
            }
        }
    ]

The contents is an array containing all the applicable permissions. When the ``acl`` record does not exist, this is equivalent to having an empty array.

The meaning of the fields within a permission object are the following:

- ``subjects``: An array of subjects for which this permission object applies.

  - ``addresses``: An array of strings representing the addresses for which signatures are expected.
  - ``required``: The number of required signatures from the ``addresses`` array. If the ``addresses`` array contains 3 addresses, and ``required`` is set to 2, that means that for the permission to apply, at least 2 signatures from the 3 addresses specified must be present. This is known as a n-of-m multi-signature scheme.
  
- ``recursive``: (Default: true) A boolean indicating whether the permission applies recursively to the sub accounts.

  .. note:: With recursion, lower level permissions overrule higher level permissions.

- ``record_name``: (Default: empty string) The pattern to use for record name matching.
- ``record_name_matching``: (Default: ``Prefix``) The type of record name matching to use. There are two possible values:

  - ``Exact`` means that the record name must be exactly equal to the value of the ``record_name`` field for the permission to apply.
  - ``Prefix`` means that the record name must start with the value of the ``record_name`` field for the permission to apply. Using ``Prefix`` with an empty ``record_name`` means that the permission applies to all records.
  
  .. hint:: The record name of an ``ACC`` record is the asset path.
  
- ``permissions``: Contains the permissions being applied if this permission object is a match. The meaning of the various permissions is explained in the next section. The value must be set to ``Permit`` for the permission to be granted, or ``Deny`` for the permission to be denied. If it is unset, the inherited value is used.

Permissions
-----------

``account_negative``
~~~~~~~~~~~~~~~~~~~~

This permission indicates the right to affect the balance of ``ACC`` records, both to increase it (receive funds) and decrease it (send funds) with no restriction on the final balance. If this permission is granted, the ``ACC`` record balance can be made negative.

This permission is typically granted to the users allowed to issue an asset.

``account_spend``
~~~~~~~~~~~~~~~~~

This permission indicates the right to affect the balance of ``ACC`` records, both to increase it (receive funds) and decrease it (send funds) with the restriction that the final balance must remain positive or zero.

``account_modify``
~~~~~~~~~~~~~~~~~~

This permission is required to affect the balance of ``ACC`` records that have already been modified before (the record version is non-empty).

``account_create``
~~~~~~~~~~~~~~~~~~

This permission is required to affect the balance of ``ACC`` records that have never been modified before (the record version is empty).

.. note::
    A user can only send funds from an account if she has the ``account_negative`` or ``account_spend`` rights plus the ``account_modify`` or ``account_create`` rights. Sending to an account requires ``account_modify`` or ``account_create`` on the destination account.

    A closed loop ledger can be created by denying ``account_modify`` and ``account_create`` by default, and selectively granting these for some accounts. By doing this, only approved accounts can receive funds.

``data_modify``
~~~~~~~~~~~~~~~

This permission is required to modify a ``DATA`` record.