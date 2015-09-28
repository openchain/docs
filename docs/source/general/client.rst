.. _openchain-client:

Getting started with the wallet
===============================

Openchain Server exposes a :ref:`public HTTP API <http-api>`, which can be called by any program capable of making HTTP calls.

To wrap all those operations in a user-friendly user interface, we also provide a client: the Openchain Wallet.

The Openchain Wallet is an open source web based interface, available at `wallet.openchain.org <https://wallet.openchain.org>`_.

Connecting to a server
----------------------

The wallet is a client side application running in the browser, and capable of connecting to any Openchain endpoint. It can connect to multiple endpoints at the same time, and pull information and submit transactions to multiple instances of Openchain, however the first time you use it, you need to connect to at least one endpoint.

.. image:: /images/wallet-1.png
   :width: 75%
   :align: center

The first page invites you to connect to an endpoint. Click the link to use the test endpoint provided by Coinprism, then click "Check endpoint". The wallet will then try to connect to the Openchain instance and retrieve the instance information.

.. image:: /images/wallet-2.png
   :width: 75%
   :align: center

Confirm to connect to this endpoint.

Logging in
----------

The wallet will now ask you to provide a mnemonic seed used to derive your private key and address.

.. image:: /images/wallet-3.png
   :width: 75%
   :align: center

Click "Create a new wallet" if you want to generate a new mnemonic, and reuse one you have already generated. Click "Sign in" to confirm.

After the key has been derived from your seed, you should see your home screen:

.. image:: /images/wallet-4.png
   :width: 75%
   :align: center

You are now able to receive payments on the Openchain instance by giving your account path to the payer (``/p2pkh/n2yYKCsho8gDrr53SUtzgtKCBypD3JMUxo/`` in the example above).

Issue an asset
--------------

The test endpoint provided by Coinprism has third party asset issuance enabled, so we can now issue an asset.

To do this, click the "Assets" tab.

.. image:: /images/wallet-5.png
   :width: 75%
   :align: center

Select the endpoint and the first slot, and click "Confirm".

.. image:: /images/wallet-6.png
   :width: 75%
   :align: center
   
Click "Issue Asset" and type an amount to issue (10000 for example). Press "Issue".

You should then see a confirmation of the transaction.

.. image:: /images/wallet-7.png
   :width: 75%
   :align: center
   
Your account should have been updated with the newly issued asset.

.. image:: /images/wallet-8.png
   :width: 75%
   :align: center

Send a payment
--------------

Now that we have funds, we can send them.

Click the newly issued asset to be taken to the "Send" page.

.. image:: /images/wallet-9.png
   :width: 75%
   :align: center
   
Type a valid destination, such as ``/p2pkh/mfiCwNxuFYMtb5ytCacgzDAineD2GNCnYo/``, and a valid amount.

Press "Send" to confirm. If the transaction went through successfully, you should see the transaction confirmation screen.

.. image:: /images/wallet-7.png
   :width: 75%
   :align: center