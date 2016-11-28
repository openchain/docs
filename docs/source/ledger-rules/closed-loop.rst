.. _closed-loop:

How to: Configure a ledger to be closed-loop
============================================

Financial institutions and companies letting their users transfer value often have to comply with regulations that require them to "know their customers" (KYC).

It is possible to use Openchain in this configuration with little effort. This section describes the necessary steps.

The goal of this walkthrough is to configure Openchain so that:

1. Users go through an external registration process where they have their identity verified by the company administrating the ledger, and associate their identity with a public key.
2. Only public keys matching a registered user can be used to send funds.
3. Funds can only be sent to registered users.

This way, funds can only circulate amongst "known" users.

Initial configuration
---------------------

The Openchain instance must be configured with both :ref:`P2PKH accounts <p2pkh-accounts>` and :ref:`third party issuance accounts <third-party-issuance-accounts>` disabled. The settings ``validator_mode:validator:allow_p2pkh_accounts`` and ``validator_mode:validator:allow_third_party_assets`` must both be set to false to achieve this. See :ref:`this section <master-observer-configuration>` for more details.

With this configuration, by default, users have no rights, while administrators have all rights. It is not possible for any normal user to either send or receive tokens.

Onboarding process
------------------

The second step is to build an onboarding workflow for the users. For example, this could be a mobile application where the user creates a username and password, enters her email address and submits a proof of identity (photo of her passport).

As part of the process, a **private key** is generated and stored on the user's device. The matching **public key** is sent along with the other pieces of information. This part can be entirely invisible to the user.

Creating the access rights
--------------------------

Once the company has validated the identity of the user, it can create an account on Openchain for that user, and associate her username with her public key.

:ref:`Aliases <aliases>` are based on a special path (``/aka/<alias>/``). Assuming that the username of the user is ``alice``, we need to:

1. Allow other users to send funds to ``/aka/alice/`` (and subaccounts).
2. Allow Alice's public key to be used to spend funds on ``/aka/alice/`` (and subaccounts).

This can be achieved by creating an ``acl`` record under ``/aka/alice/``.

.. tip:: See the documentation about :ref:`dynamic permissions <dynamic-permissions>` for more details.

The record ``/aka/alice/:DATA:acl`` must be created and set to:

.. code-block:: json

    [
        {
            "subjects": [ { "addresses": [ ], "required": 0 } ],
            "permissions": { "account_modify": "Permit", "account_create": "Permit" }
        },
        {
            "subjects": [ { "addresses": [ "<alices-address>" ], "required": 1 } ],
            "permissions": { "account_spend": "Permit" }
        }
    ]

.. important:: Since only an administrator will have the right to modify this record, the mutation creating this record must be signed using an administrator key.

Alice's address is the base-58 representation of the hash of her public key. It is constructed the same way it would be for a Bitcoin address.

By tweaking the access control list, it is possible to:

1. Handle multiple devices (with different keys) per user.
2. Implement multisignature schemes, for joint accounts for example.

Credit the user's account
-------------------------

Now that the user has an account she can use, she will want to fund it. There are many possible configurations for this:

- A treasury is initially created by the company and credits are send from that treasury.
- Tokens are issued dynamically whenever the user purchases them through an external payment method.

Assuming the following:

- The asset path for the tokens is ``/asset/usd/`` (this can be arbitrarily chosen).
- The tokens are dynamically issued from the account ``/treasury/usd/``.

A funding transaction will simply take the form of a transaction sending X units of the asset ``/asset/usd/`` from the account ``/treasury/usd/`` to the account ``/aka/alice/``.

.. image:: /images/closedloop-1.png

The transaction should be signed by an administrator only an administrator has access to ``/treasury/usd/``. The balance on ``/treasury/usd/`` will be negative, and reflect the total amount of tokens that have been issued on the ledger. Again, the administrator is allowed to make the balance negative.

The final ledger tree should look as follow:

.. image:: /images/closedloop-2.png

.. _loss-theft:

Addressing loss and theft of the private keys
---------------------------------------------

Inevitably, some users will lose the device on which their private key is stored.

When this happens, they should report it to the company administering the Openchain instance. The company will first perform identity checks, then ask the user to generate a new key on a new device.

The administrator can then simply update the relevant ``acl`` record to change the previous address into the new address, corresponding to the new key.

Handling fraudulent transactions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If fraudulent transactions have happened in the meantime, the administrator can commit a new transaction representing the opposite transfer.

For example, if 10 units have been sent fraudulently from ``/aka/alice/`` to ``/aka/oscar/``, then the administrator can simply submit a new transaction sending 10 units from ``/aka/oscar/`` to ``/aka/alice/``, thus reverting the effects of the fraudulent transaction. The ledger being immutable, both transactions will remain visible in the ledger, with the fact that the second transaction transferring funds back from ``/aka/oscar/`` is not signed by Oscar's key, but instead signed by the administrator's key.

.. note:: It bears mentioning that in a setup where all the users have to go through an identity verification process, it is unlikely that Oscar steals funds from Alice in the first place, since the company running the ledger has all the information about Oscar, and could press charges against him.

Conclusion
----------

With this setup, users are able to send tokens to each other, however, they are not able to send funds to addresses that are not associated to a registered user.

This represents just one way to implement a closed-loop ledger, and there are many other possible configurations depending on the requirements.
