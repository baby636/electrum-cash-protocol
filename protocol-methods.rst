==================
 Protocol Methods
==================

blockchain.address.get_balance
==============================

Return the confirmed and unconfirmed balances of a Bitcoin Cash address.

**Signature**

  .. function:: blockchain.address.get_balance(address)
  .. versionadded:: 1.4.3

  * *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  See :func:`blockchain.scripthash.get_balance`.

blockchain.address.get_history
==============================

Return the confirmed and unconfirmed history of a Bitcoin Cash address.

**Signature**

  .. function:: blockchain.address.get_history(address)
  .. versionadded:: 1.4.3

  * *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  As for :func:`blockchain.scripthash.get_history`.

blockchain.address.get_mempool
==============================

Return the unconfirmed transactions of a Bitcoin Cash address.

**Signature**

  .. function:: blockchain.address.get_mempool(address)
  .. versionadded:: 1.4.3

  * *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  As for :func:`blockchain.scripthash.get_mempool`.

blockchain.address.get_scripthash
=================================

Translate a Bitcoin Cash address to a :ref:`script hash <script hashes>`. This
method is potentially useful for clients preferring to work with :ref:`script
hashes <script hashes>` but lacking the local libraries necessary to generate
them.

**Signature**

  .. function:: blockchain.address.get_scripthash(address)
  .. versionadded:: 1.4.3

  * *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  The unique 32-byte hex-encoded :ref:`script hash <script hashes>` that
  corresponds to the decoded address.

blockchain.address.listunspent
==============================

Return an ordered list of UTXOs sent to a Bitcoin Cash address.

**Signature**

  .. function:: blockchain.address.listunspent(address)
  .. versionadded:: 1.4.3

  * *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  As for :func:`blockchain.scripthash.listunspent`.

blockchain.address.subscribe
============================

Subscribe to a Bitcoin Cash address.

**Signature**

  .. function:: blockchain.address.subscribe(address)
  .. versionadded:: 1.4.3

  *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  The :ref:`status <status>` of the address.

**Notifications**

  As this is a subcription, the client will receive a notification
  when the :ref:`status <status>` of the address changes.  Its
  signature is

  .. function:: blockchain.address.subscribe(address, status)
     :noindex:

  .. note:: The address returned back to the client when notifying of status
            changes will be in the same encoding and syle as was provided when
            subscribing. In effect, a whitespace-stripped version of the address
            string that the client provided will be sent back to the client when
            notifying, in order to make it easier for clients to track the
            notification.

            It is unspecified what happens if a client subscribes to the same
            address using multiple encodings or styles, but it is RECOMMENDED
            that servers simply update their internal subscription tables on
            subsequent subscriptions to the same destination such that they
            honor the latest subscription only, and not subscribe clients
            multiple times to the same logical destination. For example, Fulcrum
            server will simply update its table for how to refer to the
            subscription and send clients subsequent notifications using the
            latest encoding style of that particular address that the client
            last provided.

            Similarly, if a client mixes `blockchain.address.*` and
            `blockchain.scripthash.*` calls to the server, it is RECOMMENDED
            that the server treat all addresses as equivalent to their
            scripthashes internally such that it is possible to subscribe by
            address and later unsubscribe by scripthash, for example.

blockchain.address.unsubscribe
=================================

Unsubscribe from a Bitcoin Cash address, preventing future notifications if
its :ref:`status <status>` changes.

**Signature**

  .. function:: blockchain.address.unsubscribe(address)
  .. versionadded:: 1.4.3

  *address*

    The address as a Cash Address string (with or without prefix, case
    insensitive). Some server implementations do not support Legacy (base58)
    addresses and are not required to do so by this specification. However,
    Fulcrum does support both Legacy and Cash Address encodings.

**Result**

  Returns :const:`true` if the address was previously subscribed-to, otherwise
  :const:`false`. Note that :const:`false` might be returned even for something
  subscribed-to earlier, because the server can drop subscriptions in rare
  circumstances.

blockchain.block.header
=======================

Return the block header at the given height.

**Signature**

  .. function:: blockchain.block.header(height, cp_height=0)
  .. versionadded:: 1.3
  .. versionchanged:: 1.4
     *cp_height* parameter added
  .. versionchanged:: 1.4.1

  *height*

    The height of the block, a non-negative integer.

  *cp_height*

    Checkpoint height, a non-negative integer.  Ignored if zero,
    otherwise the following must hold:

      *height* <= *cp_height*

**Result**

  If *cp_height* is zero, the raw block header as a hexadecimal
  string.

  Otherwise a dictionary with the following keys.  This provides a
  proof that the given header is present in the blockchain; presumably
  the client has the merkle root hard-coded as a checkpoint.

  * *branch*

    The merkle branch of *header* up to *root*, deepest pairing first.

  * *header*

    The raw block header as a hexadecimal string.

  * *root*

    The merkle root of all blockchain headers up to and including
    *cp_height*.


**Example Result**

With *height* 5 and *cp_height* 0 on the Bitcoin Cash chain:

::

   "0100000085144a84488ea88d221c8bd6c059da090e88f8a2c99690ee55dbba4e00000000e11c48fecdd9e72510ca84f023370c9a38bf91ac5cae88019bee94d24528526344c36649ffff001d1d03e477"

.. _cp_height example:

With *cp_height* 8::

  {
    "branch": [
       "000000004ebadb55ee9096c9a2f8880e09da59c0d68b1c228da88e48844a1485",
       "96cbbc84783888e4cc971ae8acf86dd3c1a419370336bb3c634c97695a8c5ac9",
       "965ac94082cebbcffe458075651e9cc33ce703ab0115c72d9e8b1a9906b2b636",
       "89e5daa6950b895190716dd26054432b564ccdc2868188ba1da76de8e1dc7591"
       ],
    "header": "0100000085144a84488ea88d221c8bd6c059da090e88f8a2c99690ee55dbba4e00000000e11c48fecdd9e72510ca84f023370c9a38bf91ac5cae88019bee94d24528526344c36649ffff001d1d03e477",
    "root": "e347b1c43fd9b5415bf0d92708db8284b78daf4d0e24f9c3405f45feb85e25db"
  }

blockchain.block.headers
========================

Return a concatenated chunk of block headers from the main chain.

**Signature**

  .. function:: blockchain.block.headers(start_height, count, cp_height=0)
  .. versionadded:: 1.2
  .. versionchanged:: 1.4
     *cp_height* parameter added
  .. versionchanged:: 1.4.1

  *start_height*

    The height of the first header requested, a non-negative integer.

  *count*

    The number of headers requested, a non-negative integer.

  *cp_height*

    Checkpoint height, a non-negative integer.  Ignored if zero,
    otherwise the following must hold:

      *start_height* + (*count* - 1) <= *cp_height*

**Result**

  A dictionary with the following members:

  * *count*

    The number of headers returned, between zero and the number
    requested.  If the chain has not extended sufficiently far, only
    the available headers will be returned.  If more headers than
    *max* were requested at most *max* will be returned.

  * *hex*

    The binary block headers concatenated together in-order as a
    hexadecimal string.  Starting with version 1.4.1, AuxPoW data (if present
    in the original header) is truncated if *cp_height* is nonzero.

  * *max*

    The maximum number of headers the server will return in a single
    request.

  The dictionary additionally has the following keys if *count* and
  *cp_height* are not zero.  This provides a proof that all the given
  headers are present in the blockchain; presumably the client has the
  merkle root hard-coded as a checkpoint.

  * *root*

    The merkle root of all blockchain headers up to and including
    *cp_height*.

  * *branch*

    The merkle branch of the last returned header up to *root*,
    deepest pairing first.


**Example Response**

See :ref:`here <cp_height example>` for an example of *root* and
*branch* keys.

::

  {
    "count": 2,
    "hex": "0100000000000000000000000000000000000000000000000000000000000000000000003ba3edfd7a7b12b27ac72c3e67768f617fc81bc3888a51323a9fb8aa4b1e5e4a29ab5f49ffff001d1dac2b7c010000006fe28c0ab6f1b372c1a6a246ae63f74f931e8365e15a089c68d6190000000000982051fd1e4ba744bbbe680e1fee14677ba1a3c3540bf7b1cdb606e857233e0e61bc6649ffff001d01e36299"
    "max": 2016
  }

blockchain.estimatefee
======================

Return the estimated transaction fee per kilobyte for a transaction to
be confirmed within a certain number of blocks.

**Signature**

  .. function:: blockchain.estimatefee(number)

  *number*

    The number of blocks to target for confirmation.

**Result**

  The estimated transaction fee in coin units per kilobyte, as a
  floating point number.  If the daemon does not have enough
  information to make an estimate, the integer ``-1`` is returned.

**Example Result**

::

  0.00101079


blockchain.headers.subscribe
============================

Subscribe to receive block headers when a new block is found.

**Signature**

  .. function:: blockchain.headers.subscribe()

**Result**

  The header of the current block chain tip.  The result is a dictionary with two members:

  * *hex*

    The binary header as a hexadecimal string.

  * *height*

    The height of the header, an integer.

**Example Result**

::

   {
     "height": 520481,
     "hex": "00000020890208a0ae3a3892aa047c5468725846577cfcd9b512b50000000000000000005dc2b02f2d297a9064ee103036c14d678f9afc7e3d9409cf53fd58b82e938e8ecbeca05a2d2103188ce804c4"
   }

**Notifications**

  As this is a subcription, the client will receive a notification
  when a new block is found.  The notification's signature is:

    .. function:: blockchain.headers.subscribe(header)
       :noindex:

    * *header*

      See **Result** above.

.. note:: Should a new block arrive quickly, perhaps while the server
  is still processing prior blocks, the server may only notify of the
  most recent chain tip.  The protocol does not guarantee notification
  of all intermediate block headers.

  In a similar way the client must be prepared to handle chain
  reorganisations.  Should a re-org happen the new chain tip will not
  sit directly on top of the prior chain tip.  The client must be able
  to figure out the common ancestor block and request any missing
  block headers to acquire a consistent view of the chain state.


blockchain.relayfee
===================

Return the minimum fee a low-priority transaction must pay in order to
be accepted to the daemon's memory pool.

**Signature**

  .. function:: blockchain.relayfee()

**Result**

  The fee in whole coin units (BTC, not satoshis for Bitcoin) as a
  floating point number.

**Example Results**

::

   1e-05

::

   0.0

blockchain.scripthash.get_balance
=================================

Return the confirmed and unconfirmed balances of a :ref:`script hash
<script hashes>`.

**Signature**

  .. function:: blockchain.scripthash.get_balance(scripthash)
  .. versionadded:: 1.1

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  A dictionary with keys `confirmed` and `unconfirmed`.  The value of
  each is the appropriate balance in satoshis.

**Result Example**

::

  {
    "confirmed": 103873966,
    "unconfirmed": 236844
  }

blockchain.scripthash.get_history
=================================

Return the confirmed and unconfirmed history of a :ref:`script hash
<script hashes>`.

**Signature**

  .. function:: blockchain.scripthash.get_history(scripthash)
  .. versionadded:: 1.1

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  A list of confirmed transactions in blockchain order, with the
  output of :func:`blockchain.scripthash.get_mempool` appended to the
  list.  Each confirmed transaction is a dictionary with the following
  keys:

  * *height*

    The integer height of the block the transaction was confirmed in.

  * *tx_hash*

    The transaction hash in hexadecimal.

  See :func:`blockchain.scripthash.get_mempool` for how mempool
  transactions are returned.

**Result Examples**

::

  [
    {
      "height": 200004,
      "tx_hash": "acc3758bd2a26f869fcc67d48ff30b96464d476bca82c1cd6656e7d506816412"
    },
    {
      "height": 215008,
      "tx_hash": "f3e1bf48975b8d6060a9de8884296abb80be618dc00ae3cb2f6cee3085e09403"
    }
  ]

::

  [
    {
      "fee": 20000,
      "height": 0,
      "tx_hash": "9fbed79a1e970343fcd39f4a2d830a6bde6de0754ed2da70f489d0303ed558ec"
    }
  ]

blockchain.scripthash.get_mempool
=================================

Return the unconfirmed transactions of a :ref:`script hash <script
hashes>`.

**Signature**

  .. function:: blockchain.scripthash.get_mempool(scripthash)
  .. versionadded:: 1.1

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  A list of mempool transactions in arbitrary order.  Each mempool
  transaction is a dictionary with the following keys:

  * *height*

    ``0`` if all inputs are confirmed, and ``-1`` otherwise.

  * *tx_hash*

    The transaction hash in hexadecimal.

  * *fee*

    The transaction fee in minimum coin units (satoshis).

**Result Example**

::

  [
    {
      "tx_hash": "45381031132c57b2ff1cbe8d8d3920cf9ed25efd9a0beb764bdb2f24c7d1c7e3",
      "height": 0,
      "fee": 24310
    }
  ]


blockchain.scripthash.listunspent
=================================

Return an ordered list of UTXOs sent to a script hash.

**Signature**

  .. function:: blockchain.scripthash.listunspent(scripthash)
  .. versionadded:: 1.1

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  A list of unspent outputs in blockchain order.  This function takes
  the mempool into account.  Mempool transactions paying to the
  address are included at the end of the list in an undefined order.
  Any output that is spent in the mempool does not appear.  Each
  output is a dictionary with the following keys:

  * *height*

    The integer height of the block the transaction was confirmed in.
    ``0`` if the transaction is in the mempool.

  * *tx_pos*

    The zero-based index of the output in the transaction's list of
    outputs.

  * *tx_hash*

    The output's transaction hash as a hexadecimal string.

  * *value*

    The output's value in minimum coin units (satoshis).

**Result Example**

::

  [
    {
      "tx_pos": 0,
      "value": 45318048,
      "tx_hash": "9f2c45a12db0144909b5db269415f7319179105982ac70ed80d76ea79d923ebf",
      "height": 437146
    },
    {
      "tx_pos": 0,
      "value": 919195,
      "tx_hash": "3d2290c93436a3e964cfc2f0950174d8847b1fbe3946432c4784e168da0f019f",
      "height": 441696
    }
  ]

.. _subscribed:

blockchain.scripthash.subscribe
===============================

Subscribe to a script hash.

**Signature**

  .. function:: blockchain.scripthash.subscribe(scripthash)
  .. versionadded:: 1.1

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  The :ref:`status <status>` of the script hash.

**Notifications**

  The client will receive a notification when the :ref:`status <status>` of the script
  hash changes.  Its signature is

    .. function:: blockchain.scripthash.subscribe(scripthash, status)
       :noindex:

blockchain.scripthash.unsubscribe
=================================

Unsubscribe from a script hash, preventing future notifications if its :ref:`status
<status>` changes.

**Signature**

  .. function:: blockchain.scripthash.unsubscribe(scripthash)
  .. versionadded:: 1.4.2

  *scripthash*

    The script hash as a hexadecimal string.

**Result**

  Returns :const:`true` if the scripthash was previously subscribed-to, otherwise
  :const:`false`. Note that :const:`false` might be returned even for something
  subscribed-to earlier, because the server can drop subscriptions in rare
  circumstances.

blockchain.transaction.broadcast
================================

Broadcast a transaction to the network.

**Signature**

  .. function:: blockchain.transaction.broadcast(raw_tx)
  .. versionchanged:: 1.1
     errors returned as JSON RPC errors rather than as a result.

  *raw_tx*

    The raw transaction as a hexadecimal string.

**Result**

  The transaction hash as a hexadecimal string.

  **Note** protocol version 1.0 (only) does not respond according to
  the JSON RPC specification if an error occurs.  If the daemon
  rejects the transaction, the result is the error message string from
  the daemon, as if the call were successful.  The client needs to
  determine if an error occurred by comparing the result to the
  expected transaction hash.

**Result Examples**

::

   "a76242fce5753b4212f903ff33ac6fe66f2780f34bdb4b33b175a7815a11a98e"

Protocol version 1.0 returning an error as the result:

::

  "258: txn-mempool-conflict"

blockchain.transaction.dsproof.get
==================================

Returns information on a :ref:`double-spend proof <dsproofs>`. The query can be
by either `tx_hash` or `dspid`.

**Signature**

  .. function:: blockchain.transaction.dsproof.get(hash)
  .. versionadded:: 1.4.5

  *hash*

    The transaction hash (or `dspid`) as a hexadecimal string.


**Result**

    If the transaction in question has an associated :ref:`dsproof <dsproofs>`,
    then a JSON object. Otherwise :const:`null`.

**Example Results**::

   {
     "dspid": "587d18bf8a64ede9c7450fdaeab27b9b3c46cfa8948f4c145f889601153c56b0",
     "txid": "5b59ce35093fbd13549cd6f203d4b5b01762d70e75b8e9733dfc463e0ff8cc13",
     "hex": "410c56078977120e828e4aacdd813a818d17c47d94183aa176d62c805d47697dddddf46c2ab68ee1e46a3e17aa7da548c38ec43416422d433b1782eb3298356df441",
     "outpoint": {
       "txid": "f6e2a16ba665d5402dad147fe35872961bc6961da62345a2171ee001cfcf7600",
       "vout": 0
     },
     "descendants": [
       "36fbb099e6de59d23477727e3199c65caae35ded957660f56fc681a6d81d5570",
       "5b59ce35093fbd13549cd6f203d4b5b01762d70e75b8e9733dfc463e0ff8cc13"
     ]
   }

blockchain.transaction.dsproof.list
===================================

List all of the transactions that currently have double-spend proofs associated
with them.

**Signature**

  .. function:: blockchain.transaction.dsproof.list()
  .. versionadded:: 1.4.5

**Result**

  A JSON array of hexadecimal strings. May be empty.  Each string is a
  transaction hash of an in-mempool transaction that has a double-spend proof
  associated with it. Each of the hashes appearing in the list may be given as
  an argument to :func:`blockchain.transaction.dsproof.get` in order to obtain
  the associated double-spend proof for that transaction.

**Example Results**::

   [
     "e67cc122f3c28a4243c3a1b14b38a9474c22ba928af9a194ca2b85426f0fd1bb",
     "077f0cc2439f2e48567c72eeeba5a447f8649c00c3d18ab6516eccfd4119726f",
     "ccc2f0d90b7067a83566024d4df842f0b6cb8180e18d642fcc85cae8acadbd58"
   ]

blockchain.transaction.dsproof.subscribe
========================================

Subscribe for :ref:`dsproof <dsproofs>` notifications for a transaction.

**Signature**

  .. function:: blockchain.transaction.dsproof.subscribe(tx_hash)
  .. versionadded:: 1.4.5

  *tx_hash*

    The transaction hash as a hexadecimal string.

**Result**

  A result identical to what one would get from :func:`blockchain.transaction.dsproof.get`.

**Notifications**

  The client will receive a notification when the :ref:`dsproof <dsproofs>`  status
  of the transaction changes.  Its signature is

    .. function:: blockchain.transction.dsproof.subscribe(tx_hash, dsproof)
       :noindex:

       With *dsproof* being identical to what one would get from invoking
       :func:`blockchain.transaction.dsproof.get` for that particular *tx_hash*.

blockchain.transaction.dsproof.unsubscribe
==========================================

Unsubscribe from receiving any further :ref:`dsproof <dsproofs>` notifications
for a transaction.

**Signature**

  .. function:: blockchain.transaction.dsproof.unsubscribe(tx_hash)
  .. versionadded:: 1.4.5

  *tx_hash*

    The transaction hash as a hexadecimal string.

**Result**

  Returns :const:`true` if the transaction was previously subscribed-to for
  dsproof notifications, otherwise :const:`false`. Note that :const:`false`
  might be returned even for something subscribed-to earlier, because the server
  can drop subscriptions in rare circumstances.


blockchain.transaction.get
==========================

Return a raw transaction.

**Signature**

  .. function:: blockchain.transaction.get(tx_hash, verbose=false)
  .. versionchanged:: 1.1
     ignored argument *height* removed
  .. versionchanged:: 1.2
     *verbose* argument added

  *tx_hash*

    The transaction hash as a hexadecimal string.

  *verbose*

    Whether a verbose coin-specific response is required.

**Result**

    If *verbose* is :const:`false`:

       The raw transaction as a hexadecimal string.

    If *verbose* is :const:`true`:

       The result is a coin-specific dictionary -- whatever the coin
       daemon returns when asked for a verbose form of the raw
       transaction.

**Example Results**

When *verbose* is :const:`false`::

  "01000000015bb9142c960a838329694d3fe9ba08c2a6421c5158d8f7044cb7c48006c1b48"
  "4000000006a4730440220229ea5359a63c2b83a713fcc20d8c41b20d48fe639a639d2a824"
  "6a137f29d0fc02201de12de9c056912a4e581a62d12fb5f43ee6c08ed0238c32a1ee76921"
  "3ca8b8b412103bcf9a004f1f7a9a8d8acce7b51c983233d107329ff7c4fb53e44c855dbe1"
  "f6a4feffffff02c6b68200000000001976a9141041fb024bd7a1338ef1959026bbba86006"
  "4fe5f88ac50a8cf00000000001976a91445dac110239a7a3814535c15858b939211f85298"
  "88ac61ee0700"

When *verbose* is :const:`true`::

 {
   "blockhash": "0000000000000000015a4f37ece911e5e3549f988e855548ce7494a0a08b2ad6",
   "blocktime": 1520074861,
   "confirmations": 679,
   "hash": "36a3692a41a8ac60b73f7f41ee23f5c917413e5b2fad9e44b34865bd0d601a3d",
   "hex": "01000000015bb9142c960a838329694d3fe9ba08c2a6421c5158d8f7044cb7c48006c1b484000000006a4730440220229ea5359a63c2b83a713fcc20d8c41b20d48fe639a639d2a8246a137f29d0fc02201de12de9c056912a4e581a62d12fb5f43ee6c08ed0238c32a1ee769213ca8b8b412103bcf9a004f1f7a9a8d8acce7b51c983233d107329ff7c4fb53e44c855dbe1f6a4feffffff02c6b68200000000001976a9141041fb024bd7a1338ef1959026bbba860064fe5f88ac50a8cf00000000001976a91445dac110239a7a3814535c15858b939211f8529888ac61ee0700",
   "locktime": 519777,
   "size": 225,
   "time": 1520074861,
   "txid": "36a3692a41a8ac60b73f7f41ee23f5c917413e5b2fad9e44b34865bd0d601a3d",
   "version": 1,
   "vin": [ {
     "scriptSig": {
       "asm": "30440220229ea5359a63c2b83a713fcc20d8c41b20d48fe639a639d2a8246a137f29d0fc02201de12de9c056912a4e581a62d12fb5f43ee6c08ed0238c32a1ee769213ca8b8b[ALL|FORKID] 03bcf9a004f1f7a9a8d8acce7b51c983233d107329ff7c4fb53e44c855dbe1f6a4",
       "hex": "4730440220229ea5359a63c2b83a713fcc20d8c41b20d48fe639a639d2a8246a137f29d0fc02201de12de9c056912a4e581a62d12fb5f43ee6c08ed0238c32a1ee769213ca8b8b412103bcf9a004f1f7a9a8d8acce7b51c983233d107329ff7c4fb53e44c855dbe1f6a4"
     },
     "sequence": 4294967294,
     "txid": "84b4c10680c4b74c04f7d858511c42a6c208bae93f4d692983830a962c14b95b",
     "vout": 0}],
   "vout": [ { "n": 0,
              "scriptPubKey": { "addresses": [ "12UxrUZ6tyTLoR1rT1N4nuCgS9DDURTJgP"],
                                "asm": "OP_DUP OP_HASH160 1041fb024bd7a1338ef1959026bbba860064fe5f OP_EQUALVERIFY OP_CHECKSIG",
                                "hex": "76a9141041fb024bd7a1338ef1959026bbba860064fe5f88ac",
                                "reqSigs": 1,
                                "type": "pubkeyhash"},
              "value": 0.0856647},
            { "n": 1,
              "scriptPubKey": { "addresses": [ "17NMgYPrguizvpJmB1Sz62ZHeeFydBYbZJ"],
                                "asm": "OP_DUP OP_HASH160 45dac110239a7a3814535c15858b939211f85298 OP_EQUALVERIFY OP_CHECKSIG",
                                "hex": "76a91445dac110239a7a3814535c15858b939211f8529888ac",
                                "reqSigs": 1,
                                "type": "pubkeyhash"},
              "value": 0.1360904}]}

blockchain.transaction.get_height
=================================

Returns the block height for a confirmed transaction, or 0 for a mempool
transaction, given its hash.

**Signature**

  .. function:: blockchain.transaction.get_height(tx_hash)
  .. versionadded:: 1.4.5

  *tx_hash*

    The transaction hash as a hexadecimal string.

**Result**

  Either a numeric value or :const:`null`.

  * Numeric values :const:`> 0`

    The transaction is confirmed at this block height.

  * Numeric values :const:`== 0`

    The transaction is not confirmed but is in the mempool.

  * :const:`null`

    The transaction is unknown.

blockchain.transaction.get_merkle
=================================

Return the merkle branch to a confirmed transaction given its hash
and, optionally, its height.

**Signature**

  .. function:: blockchain.transaction.get_merkle(tx_hash, [height])
  .. versionchanged:: 1.4.5
     *height* is no longer required and is now optional

  *tx_hash*

    The transaction hash as a hexadecimal string.

  *height* (optional in 1.4.5 or above)

    The height at which it was confirmed, an integer. As of version 1.4.5 of
    the protocol, this second argument may be omitted, however if it is included
    the lookup may return the results slightly faster since an extra database
    lookup is avoided on the server-side.

**Result**

  A dictionary with the following keys:

  * *block_height*

    The height of the block the transaction was confirmed in.

  * *merkle*

    A list of transaction hashes the current hash is paired with,
    recursively, in order to trace up to obtain merkle root of the
    block, deepest pairing first.

  * *pos*

    The 0-based index of the position of the transaction in the
    ordered list of transactions in the block.

**Result Example**

::

  {
    "merkle":
    [
      "713d6c7e6ce7bbea708d61162231eaa8ecb31c4c5dd84f81c20409a90069cb24",
      "03dbaec78d4a52fbaf3c7aa5d3fccd9d8654f323940716ddf5ee2e4bda458fde",
      "e670224b23f156c27993ac3071940c0ff865b812e21e0a162fe7a005d6e57851",
      "369a1619a67c3108a8850118602e3669455c70cdcdb89248b64cc6325575b885",
      "4756688678644dcb27d62931f04013254a62aeee5dec139d1aac9f7b1f318112",
      "7b97e73abc043836fd890555bfce54757d387943a6860e5450525e8e9ab46be5",
      "61505055e8b639b7c64fd58bce6fc5c2378b92e025a02583303f69930091b1c3",
      "27a654ff1895385ac14a574a0415d3bbba9ec23a8774f22ec20d53dd0b5386ff",
      "5312ed87933075e60a9511857d23d460a085f3b6e9e5e565ad2443d223cfccdc",
      "94f60b14a9f106440a197054936e6fb92abbd69d6059b38fdf79b33fc864fca0",
      "2d64851151550e8c4d337f335ee28874401d55b358a66f1bafab2c3e9f48773d"
    ],
    "block_height": 450538,
    "pos": 710
  }

blockchain.transaction.id_from_pos
==================================

Return a transaction hash and optionally a merkle proof,
given a block height and a position in the block.

**Signature**

  .. function:: blockchain.transaction.id_from_pos(height, tx_pos, merkle=false)
  .. versionadded:: 1.4

  *height*

    The main chain block height, a non-negative integer.

  *tx_pos*

    A zero-based index of the transaction in the given block, an integer.

  *merkle*

    Whether a merkle proof should also be returned, a boolean.

**Result**

  If *merkle* is :const:`false`, the transaction hash as a hexadecimal string.
  If :const:`true`, a dictionary with the following keys:

  * *tx_hash*

    The transaction hash as a hexadecimal string.

  * *merkle*

    A list of transaction hashes the current hash is paired with,
    recursively, in order to trace up to obtain merkle root of the
    block, deepest pairing first.

**Example Results**

When *merkle* is :const:`false`::

  "fc12dfcb4723715a456c6984e298e00c479706067da81be969e8085544b0ba08"

When *merkle* is :const:`true`::

  {
    "tx_hash": "fc12dfcb4723715a456c6984e298e00c479706067da81be969e8085544b0ba08",
    "merkle":
    [
      "928c4275dfd6270349e76aa5a49b355eefeb9e31ffbe95dd75fed81d219a23f8",
      "5f35bfb3d5ef2ba19e105dcd976928e675945b9b82d98a93d71cbad0e714d04e",
      "f136bcffeeed8844d54f90fc3ce79ce827cd8f019cf1d18470f72e4680f99207",
      "6539b8ab33cedf98c31d4e5addfe40995ff96c4ea5257620dfbf86b34ce005ab",
      "7ecc598708186b0b5bd10404f5aeb8a1a35fd91d1febbb2aac2d018954885b1e",
      "a263aae6c470b9cde03b90675998ff6116f3132163911fafbeeb7843095d3b41",
      "c203983baffe527edb4da836bc46e3607b9a36fa2c6cb60c1027f0964d971b29",
      "306d89790df94c4632d652d142207f53746729a7809caa1c294b895a76ce34a9",
      "c0b4eff21eea5e7974fe93c62b5aab51ed8f8d3adad4583c7a84a98f9e428f04",
      "f0bd9d2d4c4cf00a1dd7ab3b48bbbb4218477313591284dcc2d7ca0aaa444e8d",
      "503d3349648b985c1b571f59059e4da55a57b0163b08cc50379d73be80c4c8f3"
    ]
  }

blockchain.transaction.subscribe
================================

Subscribe to a transaction in order to receive future notifications if its confirmation status changes.

**Signature**

  .. function:: blockchain.transaction.subscribe(tx_hash)
  .. versionadded:: 1.4.5

  *tx_hash*

    The transaction hash as a hexadecimal string.

**Result**

  A result identical to what one would get from :func:`blockchain.transaction.get_height`.

**Notifications**

  The client will receive a notification when the confirmation status
  of the transaction changes.  Its signature is

    .. function:: blockchain.transction.subscribe(tx_hash, height)
       :noindex:

       With *height* being identical to what one would get from invoking :func:`blockchain.transaction.get_height`.

blockchain.transaction.unsubscribe
==================================

Unsubscribe from a transaction, preventing future notifications if its confirmation status changes.

**Signature**

  .. function:: blockchain.transaction.unsubscribe(tx_hash)
  .. versionadded:: 1.4.5

  *tx_hash*

    The transaction hash as a hexadecimal string.

**Result**

  Returns :const:`true` if the transaction was previously subscribed-to, otherwise
  :const:`false`. Note that :const:`false` might be returned even for something
  subscribed-to earlier, because the server can drop subscriptions in rare
  circumstances.

blockchain.utxo.get_info
========================

Return information for an unspent transaction output.

**Signature**

  .. function:: blockchain.utxo.get_info(tx_hash, out_n)
  .. versionadded:: 1.4.4

  *tx_hash*

    The UTXO's transaction hash as a hexadecimal string.

  *out_n*

    The UTXO's transaction output number. This should be a number in the range
    0 <= out_n <= 65535

**Result**

  If the UTXO in question does not exist or is spent, :const:`null` is returned.
  Otherwise, a dictionary will be returned containing the following keys:

  * *confirmed_height*

    (Optional) The integer height of the block the UTXO's transaction was
    confirmed in. This key will be missing from the dictionary if the
    transaction is in the mempool and is not yet confirmed.

  * *scripthash*

    The output's destination :ref:`script hash <script hashes>` as a hexadecimal
    string.

  * *value*

    The output's value in integer minimum coin units (satoshis).

**Result Example**

::

   {
     "confirmed_height": 602123,
     "scripthash": "1c1e184c97abc87626c497b95f755df1025f48b9c27d037ea335677c57f38e5c",
     "value": 45318048
   }

mempool.get_fee_histogram
=========================

Return a histogram of the fee rates paid by transactions in the memory
pool, weighted by transaction size.

**Signature**

  .. function:: mempool.get_fee_histogram()
  .. versionadded:: 1.2

**Result**

  The histogram is an array of [*fee*, *vsize*] pairs, where |vsize_n|
  is the cumulative virtual size of mempool transactions with a fee rate
  in the interval [|fee_n1|, |fee_n|], and |fee_n1| > |fee_n|.

  .. |vsize_n| replace:: vsize\ :sub:`n`
  .. |fee_n| replace:: fee\ :sub:`n`
  .. |fee_n1| replace:: fee\ :sub:`n-1`

  Fee intervals may have variable size.  The choice of appropriate
  intervals is currently not part of the protocol.

**Example Result**

  ::

    [[12, 128812], [4, 92524], [2, 6478638], [1, 22890421]]


server.add_peer
===============

A newly-started server uses this call to get itself into other servers'
peers lists.  It sould not be used by wallet clients.

**Signature**

  .. function:: server.add_peer(features)

  .. versionadded:: 1.1

  * *features*

    The same information that a call to the sender's
    :func:`server.features` RPC call would return.

**Result**

  A boolean indicating whether the request was tentatively accepted.
  The requesting server will appear in :func:`server.peers.subscribe`
  when further sanity checks complete successfully.


server.banner
=============

Return a banner to be shown in the Electron Cash console.

**Signature**

  .. function:: server.banner()

**Result**

  A string.

**Example Result**

  ::

     "Welcome to Fulcrum!"


server.donation_address
=======================

Return a server donation address.

**Signature**

  .. function:: server.donation_address()

**Result**

  A string.

**Example Result**

  ::

     "1BWwXJH3q6PRsizBkSGm2Uw4Sz1urZ5sCj"


server.features
===============

Return a list of features and services supported by the server.

**Signature**

  .. function:: server.features()
  .. versionchanged:: 1.4.2
     *hosts* key is no longer required, but recommended.
  .. versionchanged:: 1.4.5
     *dsproof* key added (optional).

**Result**

  A dictionary of keys and values.  Each key represents a feature or
  service of the server, and the value gives additional information.

  The following features MUST be reported by the server.  Additional
  key-value pairs may be returned.

  * *genesis_hash*

    The hash of the genesis block.  This is used to detect if a peer
    is connected to one serving a different network.

  * *hash_function*

    The hash function the server uses for :ref:`script hashing
    <script hashes>`.  The client must use this function to hash
    pay-to-scripts to produce script hashes to send to the server.
    The default is "sha256".  "sha256" is currently the only
    acceptable value.

  * *server_version*

    A string that identifies the server software.  Should be the same
    as the result to the :func:`server.version` RPC call.

  * *protocol_max*
  * *protocol_min*

    Strings that are the minimum and maximum Electrum Cash protocol
    versions this server speaks.  Example: "1.1".

  * *pruning*

    An integer, the pruning limit.  Omit or set to :const:`null` if
    there is no pruning limit.  Should be the same as what would
    suffix the letter ``p`` in the IRC real name.

  The following features are RECOMMENDED that be reported by the servers.

  * *hosts*

    A dictionary, keyed by host name, that this server can be reached
    at.  If this dictionary is missing, then this is a way to signal to
    other servers that while this host is reachable, it does not wish to
    peer with other servers.  A server SHOULD stop peering with a peer
    if it sees the *hosts* dictionary for its peer is empty and/or no
    longer contains the expected route (e.g. hostname).  Normally this
    dictionary will only contain a single entry; other entries can be
    used in case there are other connection routes (e.g. Tor).

    The value for a host is itself a dictionary, with the following
    optional keys:

    * *ssl_port*

      An integer.  Omit or set to :const:`null` if SSL connectivity
      is not provided.

    * *tcp_port*

      An integer.  Omit or set to :const:`null` if TCP connectivity is
      not provided.

    * *ws_port*

      An integer.  Omit or set to :const:`null` if Web Socket (ws://)
      connectivity is not provided.

    * *wss_port*

      An integer.  Omit or set to :const:`null` if Web Socket Secure (wss://)
      connectivity is not provided.

    A server should ignore information provided about any host other
    than the one it connected to.

  * *dsproof*

    A boolean value. If present and set to :const:`true`, then the server
    has :ref:`double-spend proof <dsproofs>` support, and it supports the
    :const:`blockchain.transaction.dsproof.*` set of RPC methods. If this key
    is missing or :const:`false`, then the server does not support :ref:`dsproofs <dsproofs>`.


**Example Result**

::

  {
      "genesis_hash": "000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943",
      "hosts": {"14.3.140.101": {"tcp_port": 51001, "ssl_port": 51002}},
      "protocol_max": "1.4",
      "protocol_min": "1.4.3",
      "pruning": null,
      "server_version": "Fulcrum 1.0.5",
      "hash_function": "sha256",
      "dsproof": true
  }


server.peers.subscribe
======================

Return a list of peer servers.  Despite the name this is not a
subscription and the server must send no notifications.

**Signature**

  .. function:: server.peers.subscribe()

**Result**

  An array of peer servers, each returned as a 3-element array.  For
  example::

    ["107.150.45.210",
     "e.anonyhost.org",
     ["v1.0", "p10000", "t", "s995"]]

  The first element is the IP address, the second is the host name
  (which might also be an IP address), and the third is a list of
  server features.  Each feature and starts with a letter.  'v'
  indicates the server maximum protocol version, 'p' its pruning limit
  and is omitted if it does not prune, 't' is the TCP port number, and
  's' is the SSL port number.  If a port is not given for 's' or 't'
  the default port for the coin network is implied.  If 's' or 't' is
  missing then the server does not support that transport.

server.ping
===========

Ping the server to ensure it is responding, and to keep the session
alive.  The server may disconnect clients that have sent no requests
for roughly 10 minutes.

**Signature**

  .. function:: server.ping()
  .. versionadded:: 1.2

**Result**

  Returns :const:`null`.

server.version
==============

Identify the client to the server and negotiate the protocol version.
Only the first :func:`server.version` message is accepted.

**Signature**

  .. function:: server.version(client_name="", protocol_version="1.4")

  * *client_name*

    A string identifying the connecting client software.

  * *protocol_version*

    An array ``[protocol_min, protocol_max]``, each of which is a
    string.  If ``protocol_min`` and ``protocol_max`` are the same,
    they can be passed as a single string rather than as an array of
    two strings, as for the default value.

  The server should use the highest protocol version both support::

    version = min(client.protocol_max, server.protocol_max)

  If this is below the value::

    max(client.protocol_min, server.protocol_min)

  then there is no protocol version in common and the server must
  close the connection.  Otherwise it should send a response
  appropriate for that protocol version.

**Result**

  An array of 2 strings:

     ``[server_software_version, protocol_version]``

  identifying the server and the protocol version that will be used
  for future communication.

**Example**::

  server.version("Electron Cash 3.3.6", ["1.2", "1.4"])

**Example Result**::

  ["Fulcrum 1.0.5", "1.4"]
