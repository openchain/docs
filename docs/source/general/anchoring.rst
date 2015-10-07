.. _anchoring:

Anchoring and ledger integrity
==============================

Openchain is capable of immutability by committing a hash of the entire ledger (the **cumulative hash**) onto a non-reversible Blockchain such as Bitcoin.

.. note:: In the current version, the only anchoring mode available is using the Bitcoin blockchain. Different anchoring modes will be available in the future, such as anchoring in a central repository.

With the Bitcoin anchoring mode, one transaction is committed in every Bitcoin block, and contains the cumulative hash at the current time.

By doing this, even if Openchain is processing thousands of transactions per second, only one transaction gets sent to the Bitcoin blockchain every 10 minutes. There are multiple benefits to this approach:

- The irreversibility of the Openchain ledger is ensured by Bitcoin miners, and Openchain enjoys the same level of irreversibility as any other Bitcoin transaction.
- At the maximum resolution (one anchor per block), no more than 4,320 transactions per months (in average) will be committed into the blockchain, which will cost about $10 per month (as of October 2015).
- The resolution can be tuned to further reduce that cost.
- Openchain can process thousands of transactions per second while remaining cost-efficient.

:ref:`"Observer nodes" <observer-nodes>` replicating all the verified transactions locally have the ability to compute their own version of the cumulative hash and compare it to the anchor in the Bitcoin blockchain.

Calculating the cumulative hash
-------------------------------

.. include:: /common/stub.txt

Blockchain anchor format
------------------------

.. include:: /common/stub.txt