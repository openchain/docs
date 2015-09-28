Overview of Openchain
=====================

What is Openchain?
------------------

Openchain is an open source distributed ledger technology. It is suited for organizations wishing to issue and manage digital assets in a robust, secure and scalable way.

Features of Openchain include:

1. Instant confirmation of transactions.
2. No transaction fees to be paid.
3. Extremely high scalability.
4. Digital signatures for non-repudiation.
5. Ability to synchronize a local replica with another Openchain instance.
6. Transparency and auditability of transactions.
7. Full control of the participants on the network.
8. Full control of what constitutes a valid transaction.
9. :ref:`Immutability <anchoring>`: Commit an anchor in the Bitcoin Blockchain to benefit from the irreversibility of its Proof of Work.

Getting started
---------------

To familiarize yourself with Openchain, you can:

* :ref:`Try the wallet <openchain-client>` against the test endpoint
* :ref:`Deploy your own Openchain server <docker-deployment>`

Frequently Asked Questions
--------------------------

.. include:: /common/stub.txt

Is Openchain a block chain?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Openchain is not a *block chain* by itself, but a close cousin. A block chain is a data structure where a series of blocks are ordered and linked cryptographically through hashing. Usually, a block is a set of transactions. Openchain links transactions cryptographically through hashing, however it doesn't have a concept of blocks, so Openchain should be considered as a *transaction chain* rather than a *block chain*.

Is Openchain a sidechain?
~~~~~~~~~~~~~~~~~~~~~~~~~

It is possible to use a pegging module that will act as a bridge between a Blockchain (such as Bitcoin) and an Openchain instance. When Bitcoins are sent to a specific address, a proxy for those coins will be created on the Openchain instance. Later on, these proxy tokens can be redeemed to unlock the Bitcoins on the main chain. This setup creates a 2-way peg between Bitcoin and the Openchain instance. In that scenario, the Openchain instance is behaving as a sidechain.

The pegging module is optional, and an instance doesn't have to be setup as a sidechain if that is not required.