.. _anchoring:

Anchoring and ledger integrity
==============================

Openchain is capable of immutability by committing a hash of the entire ledger (the **cumulative hash**) onto a non-reversible Blockchain such as Bitcoin.

.. note:: In the current version, the only anchoring mode available is the ``blockchain`` mode, based on the Bitcoin blockchain. Different anchoring modes will be available in the future, such as anchoring in a central repository.

With the Bitcoin anchoring mode, one transaction is committed in every Bitcoin block, and contains the cumulative hash at the current time.

By doing this, even if Openchain is processing thousands of transactions per second, only one transaction gets sent to the Bitcoin blockchain every 10 minutes. There are multiple benefits to this approach:

- The irreversibility of the Openchain ledger is ensured by the Bitcoin miners, therefore Openchain enjoys the same level of irreversibility as Bitcoin itself.
- At the maximum resolution (one anchor per block), no more than 4,320 transactions per month (in average) will be committed into the blockchain, which will cost about $10 per month (as of October 2015), regardless of the number of transactions processed.
- The resolution can be tuned to further reduce that cost.
- Openchain can process thousands of transactions per second while remaining very cost-efficient.

:ref:`"Observer nodes" <observer-nodes>` replicating all the verified transactions locally have the ability to compute their own version of the cumulative hash and compare it to the anchor in the Bitcoin blockchain.

Calculating the cumulative hash
-------------------------------

The cumulative hash is updated every time a new transaction is added to the ledger.

The cumulative hash at a given height is calculated using the previous cumulative hash and the hash of the new transaction being added to the ledger:

.. code-block:: text
    
    cumulative_hash = SHA256( SHA256( previous_cumulative_hash + new_transaction_hash ) )
    
- ``previous_cumulative_hash`` (32 bytes) is the cumulative hash at the previous height. At height 0 (when the ledger has no transaction), a 32 bytes buffer filled with zeroes is used.
- ``new_transaction_hash`` (32 bytes) is the double SHA-256 hash of the :ref:`raw transaction <data-structures-transaction>` being added to the ledger.

Both values are concatenated to form a 64 bytes array, then hashed using double SHA-256.

Blockchain anchor format
------------------------

The Blockchain anchor is stored in the blockchain using an OP_RETURN operator, followed by a pushdata containing the anchor.

.. code-block:: text
    
    OP_RETURN <anchor (42 bytes)>

The anchor is constructed in the following way:

.. code-block:: text

    0x4f 0x43 <transaction count (8 bytes)> <cumulative hash (32 bytes)>

- The first two bytes indicates that the output represents an Openchain anchor.
- The transaction count is the number of transactions being represented by the cumulative hash (the height). It's an unsigned 64 bits integer, encoded in big endian.
- The cumulative hash is full cumulative hash (256 bits) as calculated in the previous section.
