.. _create-info-record:

Setting the instance info on a new instance
===========================================

The :ref:`ledger info record<ledger-info-record>` exposes meta-information about the ledger itself. It is used by clients that connect to the instance to retrieve informations such as the name of the instance, and the associated terms of service.

After you have deployed a new instance, it is a good idea to create the info record. This can be done from the web interface.

1. First, follow :ref:`these steps <openchain-client>` to connect to the instance and log in. Make sure you log in with a seed that has admin access on this instance as the ``info`` record can only be modified by an administrator.
2. Go to the **Advanced** tab and click **Edit Ledger Info** on the left. The screen will show you a form that will let you edit the ledger name and other fields stored in the ``info`` record.

.. important:: Make sure that the **Validator Root URL** is set to the same value as the ``root_url`` setting in the configuration file.
