The transaction stream
======================

Openchain server exposes a websocket endpoint (``/stream``) called the transaction stream. The transaction stream provides a live stream of transactions as they get committed into the ledger.

Validator nodes
---------------

The Openchain Server node can function in two different modes: **validator mode** and **observer mode**.

In validator mode, the node accepts transactions and validates them. Rules that make a transaction valid or invalid are customizable. They can be defined by the administrator of the validator node, and are a combination of :ref:`implicit rules <ledger-rules>`, and explicit permissions.

When a transaction is deemed valid, it gets committed into the ledger.

.. _observer-nodes:

Observer nodes
--------------

Observer nodes are nodes connecting to an upstream node, and downloading all transactions in real time using the transaction stream. The validator node is always the most upstream node. When it verifies a transaction, the transaction trickles down to its observers. All the observers should have an exact copy of the state held by the verifying node.

It is not possible to submit a transaction for validation to an observer node, as it only has a read-only view of the ledger.

Observer nodes have the ability to verify the integrity of their copy of the ledger through :ref:`anchors <anchoring>`.

Configuration
~~~~~~~~~~~~~

To configure a node to be in observer mode, the ``observer_mode`` section needs to exist in the :ref:`configuration file <master-observer-configuration>`, and the ``upstream_url`` must be set to the root URL of the upstream node.