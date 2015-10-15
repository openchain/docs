Overview of Openchain
=====================

What is Openchain?
------------------

Openchain is an open source distributed ledger technology. It is suited for organizations wishing to issue and manage digital assets in a robust, secure and scalable way.

Features of Openchain include:

1. Instant confirmation of transactions.
2. No mining fees.
3. Extremely high scalability.
4. Secured through digital signatures.
5. :ref:`Immutability <anchoring>`: Commit an anchor in the Bitcoin Blockchain to benefit from the irreversibility of its Proof of Work.
6. :ref:`Assign aliases <aliases>` to users instead of using base-58 addresses.
7. Multiple levels of control:

  - Fully open ledger that can be joined anonymously.
  - :ref:`Closed-loop ledger <closed-loop>` where participants must be approved by the administrator.
  - A mix of the above where approved users enjoy more rights than anonymous users.

8. :ref:`Hierarchical account system <account-hierarchy>` allowing to set permissions at any level.
9. Transparency and auditability of transactions.
10. :ref:`Handle loss or theft <loss-theft>` of private keys without any loss to the end users.
11. Ability to have multiple Openchain instances :ref:`replicating from each other <transaction-stream>`.

Getting started
---------------

To familiarize yourself with Openchain, you can:

* :ref:`Try the wallet <openchain-client>` against the test endpoint
* :ref:`Deploy your own Openchain server <docker-deployment>`

Frequently Asked Questions
--------------------------

Is Openchain a block chain?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Openchain falls under the umbrella of Blockchain technology. However, if we take the term "block chain" literally, Openchain is not a "block chain", but a close cousin. A block chain is a data structure that orders blocks of transactions and links them cryptographically through hashing.

Openchain doesn't use the concept of blocks. Transactions are directly chained with one another, and they are no longer grouped in blocks. Having to group transactions in blocks introduces a delay. Even if some systems manage to reduce the block time to just a few seconds, a few seconds is still a long time for latency-sensitive applications, such as trading. In Openchain, transactions are linked to the chain as soon as they are submitted to the network. As a result, Openchain is able to offer real-time confirmations.

This means that a more appropriate term for Openchain is a "transaction chain" rather than a "block chain".

Is Openchain a sidechain?
~~~~~~~~~~~~~~~~~~~~~~~~~

It is possible to use a pegging module that will act as a bridge between a Blockchain (such as Bitcoin) and an Openchain instance. When Bitcoins are sent to a specific address, a proxy for those coins will be created on the Openchain instance. Later on, these proxy tokens can be redeemed to unlock the Bitcoins on the main chain. This setup creates a 2-way peg between Bitcoin and the Openchain instance. In that scenario, the Openchain instance is behaving as a sidechain.

The pegging module is optional, and an instance doesn't have to be setup as a sidechain if that is not required.

Does Openchain support multi-signature?
---------------------------------------

Multi-signature is supported. Permissions are expressed using a list of public keys, and a number of require signatures. If you provide 3 public keys, and require 2 signatures, you have a 2-of-3 multi-signature account. Read about :ref:`dynamic permissions <dynamic-permissions>` to learn more about it.
