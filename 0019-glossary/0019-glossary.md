# Glossary

## Account
An administrative item that acts as a bucket of assets on a ledger.

## Application layer
The mechanisms on top of the [transport layer](#transport-layer) which embed it into an application, such as the
[Simple Payment Setup Protocol](#spsp) for simple WebFinger-based discovery of PSK parameters, which is much easier
to integrate into an end-user software application than the underlying PSK transport layer itself.

## Arbitrage
The practice of doing [cyclic transactions](#cyclic-transaction) which start and end on the same [ledger](ledger),
or groups of simultaneous transactions which add up to one cyclic transaction.
Taking advantage of quirks and fluctuations in exchange rates,
the goal is to achieve a [destination amount](#destination-amount) which is higher than the [source amount](#source-amount).

## Asset
Something you can own. Can be a physical and unique item, a quantity of a physical substance, a specific physical collection of coins and bank notes,
a non-physical quantity of currency, a non-physical claim to a resource, a reputation for keeping one's word, someone else's intention to obey the
terms of a contract, etc. Some types of assets can be more easily traded on a ledger than others. Also, for some asset pairs it is easier to determine
a fair exchange rate than for others.

## Backward action
For a receiver, when notified that an incoming transfer has reached the 'prepared' state, the backward action consists in submitting the fulfillment to the ledger.
When a connector is notified that an outgoing transfer has reached the 'executed' state, its backward action is to obtain the fulfillment from its
outgoing ledger, and submit that same fulfillment to its incoming ledger.
For a sender, there is no backward action.

## Balance
The amount of a ledger's assets held by a given account. When part of an account's balance is not readily usable, we distinguish
the cleared balance and the uncleared balance. Uncleared balance can be further split into outgoing transfers which have not yet
completed, incoming transfers which have not yet completed, and disputed balance, in the case a ledger allows [dispute](#dispute).
When an account has both cleared and uncleared balance, its "balance", when not specified further,
is only the uncleared balance. So the most pessimistic view of the balance is used by default.

## Canceled
One of the final states an interledger transactions can end up in.

## Centralized ledger
A ledger which is operated by a single entity.

## Conditional transfer
Each local transfer is first *prepared* and then either *executed* or *rejected*. When a transfer is prepared, the ledger puts the funds of the source account on hold with a *cryptographic condition* and *timeout*. If the condition is fulfilled before the timeout, the transfer is executed and the funds are transferred. If the timeout is reached, the transfer expires and the ledger returns the funds to the source account automatically.
Inspired by the [Lightning Network](http://lightning.network), Interledger uses the digest of the [SHA-256](https://en.wikipedia.org/wiki/SHA-2) hash function as the condition for transfers. The fulfillment is a valid 32-byte preimage for the hash specified when the transfer was prepared. Ledgers are responsible for validating fulfillments. [Transport Layer](#transport-layer) protocols are used by the sender and receiver to generate the condition for a particular payment.
To be fully Interledger-compatible, ledgers MUST support conditional transfers, though it is possible to send Interledger payments over a ledger that does not natively support the recommended features. See [IL-RFC 17](../0017-ledger-requirements/0017-ledger-requirements.md) for the full description and tiers of ledger requirements.

## Connector
A party who chains one transfer to the next in an interledger transaction. A connector has two tasks: its [forward action](#forward-action) and its [backward action](#backward-action).
A connector receives a local transfer on one ledger in exchange for making another local transfer on a different ledger. A single interledger payment may include multiple connectors and may traverse any number of ledgers.
Connectors take some risk, but this risk can be managed and is primarily based upon the connector's chosen ledgers and direct peers.

## Cyclic transaction
A transaction where the destination account is the same account, on the same ledger, as the source account.

# Destination address
The address of the receiver, included in the
[interledger packet](#interledger-packet), and indicating
to the ledger and account of the receiver.
Note that anyone can claim to have a certain [Interledger address](#interledger-address), address ownership is not enforced.

# Destination amount
The amount to be received by the receiver.

# Destination ledger
The ledger pointed to by the destination address.

## Exchange rate
If the adjacent ledgers of a connector represent different assets, the connector sets the exchange rate between the transfers.
Connectors may generate revenue from the difference in value between incoming and outgoing transfers.
Senders may request quotes from multiple connectors to determine the best price before sending a payment.
The exchange rate between source and destination is determined by the product of exchange rates at each hop.
The application layer may provide guidance to sender and receiver in determining whether an exchange rate offered by a connector
is reasonable or not.

## Fulfillment
See [conditional transfer](#conditional-transfer).

## Destination (transfer, ledger, amount)
The transfer/ledger/amount directly adjacent to the receiver.

## Dispute
Some ledgers, when administered by more than one authority, allow dispute over the ledger's state to occur naturally. This can be
useful when the cost of resolving every small difference in accounting is much higher than the value of the differences.
A transfer whose outcome is disputed among the ledger's administering authorities, can be resolved as 'disputed', in which case
the sender's balance is decreased, and instead of increasing the receiver's balance, the sender's "disputed balance" is increased.
Disputed balance can disappear over time when disputed transfers happen in the other direction, or when settled by out-of-band
communication between the authorities who administer the ledger.
Note that although individual ledgers can support disputed transfers, an interledger transaction as a whole can never be disputed;
its final state can only be either 'executed' or 'canceled'.

## Distributed ledger
A ledger which is operated by a group of entities.

## Executed
One of the final states an interledger transactions can end up in.

## Final state
The state an interledger transaction ends up in. Has to be either
[canceled](#canceled) or [executed](#executed).

## Forward action
For a sender, the forward action consists in creating an outgoing transfer.
When a connector is notified that an incoming transfer has reached the 'prepared' state, its forward action is to propose a transfer on the next ledger,
with the same hashlock condition, and the same ILP packet attached.
For a receiver, there is no forward action.

## Hop
One connector-facilitated connection between one ledger and the next.
The number of hops is always one less than the number of transfers, so a one-transfer ("local") transaction is a zero-hop transaction.

## IL-RFC
Interledger request for comments. A document describing a part of the protocol stack, or a topic related to it, such as this glossary. The official list of IL-RFCs is maintained in
[gh:interledger/rfcs](https://github.com/interledgers/rfcs).

## Incoming (transfer, ledger, amount)
Relative to a connector, the transfer/ledger/amount that is located inbetween the connector and the sender, directly adjacent to the connector.
For a receiver, the adjacent transfer/ledger/amount is always incoming.
A sender does not have an incoming transfer/ledger/amount.

## Interledger (in "Let's use Interledger for that!")
The [Interledger protocol stack](#interledger-protocol-stack).

## interledger (in "An interledger")
Any network of two or more ledgers connected via the Interledger protocol stack. See also [Interledger (The)](#interledger-the)

## Interledger (in "The Interledger")
The main public network of ledgers connected via the Interledger protocol stack. See [ConnectorLand](https://connector.land) for statistics about it.

## Interledger Addresses
Interledger addresses provide a ledger-agnostic way to address ledgers and accounts. Interledger addresses are dot-separated strings that contain prefixes to group ledgers. An example address might look like `g.us.acmebank.acmecorp.sales.199` or `g.crypto.bitcoin.1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2`.
When initiating an Interledger payment, the sender attaches an ILP packet to the local transfer to the connector. The packet is a binary message that includes the destination account, destination amount, and additional data for the receiver. The packet is relayed by connectors and attached to each transfer that comprises the payment. In some cases, ledger protocols may define alternative ways to communicate the packet.
Note that anyone can claim to have a certain Interledger address, there is no registry of them.
Whether or not a transaction ends up at the intended receiver is ultimately safe-guarded by the hashlock condition,
not by enforcement of address ownership.
See [IL-RFC 15](../0015-ilp-addresses/0015-ilp-addresses.md)

## Interledger Architecture
The [Interledger protocol stack](interledger-protocol-stack), and the design principles behind it.

## Interledger layer
The lowest layer of the Interledger protocol stack, consisting of
the [ILP](#interledger-protocol-ilp) Interledger protocol, and the [ILQP](#ilqp) quoting protocol,

## Interledger module
The part of a software application that processes ILP transactions.
Analoguous to the network card of an internet-connected computer.

## Interledger packet
The binary data packet that is forwarded along all hops from sender to receiver, and which provides the receiver
with metadata which the receiver may need to be willing and able to fulfill the transaction.
It also contains the destination address and destination amount, on which connectors will base their routing decisions -
both in choosing the onward path, and in deciding whether their incoming amount was big enough, in comparison to the destination
amount mentioned in the ILP packet, to even merit an [forward action](#forward-action) at all. See also [routing table](#routing-table)

## Interledger Payment Request (IPR)
In the Interledger Payment Request (IPR) protocol, the receiver generates the payment details and condition. The receiver does not share the secret used to generate the condition and fulfillment with the sender or anyone else, but the sender must ask the recipient to generate and share a condition before sending each payment. IPR is primarily useful for building non-repudiable application layer protocols, in which the sender's posession of the fulfillment proves to third parties that the sender has paid the receiver for a specific obligation.
See [IL-RFC 11](../0011-interledger-payment-request/0011-interledger-payment-request.md).

# Interledger Protocol (ILP)
The core of the Interledger protocol suite, described in [IL-RFC 3](../0003-interledger-protocol/0003-interledger-protocol.md).
Colloquially the whole Interledger stack is sometimes referred to as "ILP". Technically, however, the Interledger Protocol is only one layer in the
[Interledger protocol stack](#interledger-protocol-stack).

## Interledger protocol stack
The stack consisting of the [Interledger layer](#interledger-layer),
a choice of [transport layers](#transport-layer), and an [application layer](#application-layer).

## Interledger Quoting Protocol (ILQP)
The protocol that defines how senders request quotes from connectors to determine the source or destination amount for an Interledger payment. Quoting is optional and senders MAY cache quotes and send repeated payments through the same connector.
See [IL-RFC 8](../0008-interledger-quoting-protocol/0008-interledger-quoting-protocol.md).

## Intermediate (transfer, ledger, amount)
Any transfer/ledger/amount that is neither directly adjacent to the sender, nor to the receiver.

## Ledger
Stateful systems that track the ownership of [assets](#asset). Ledgers contain buckets of assets known as [accounts](#account) and record [transfers](#transfer) between them.
Each account has a [balance](#balance), which is the amount of the ledger's assets the account holds. Account balances may be positive or negative, representing assets or liabilities.

## Liquidity
Maximum amount which can be moved back and forth over a route.

## Local transaction
A transaction that contains just one [transfer](#transfer) and zero [hops](#hop), thus staying within the same ledger.

## Outgoing (transfer, ledger, amount)
Relative to a connector, the transfer/ledger/amount that is located inbetween the connector and the receiver, directly adjacent to the connector.
For a sender, the adjacent transfer/ledger/amount is always outgoing.
A receiver does not have an outgoing transfer/ledger/amount.

## Payment
Roughly synomymous to [transaction](#transaction), but with emphasis on the context of the negotiation between the sender and receiver. For instance,
a transaction may be paying a debt, or a purchase, it may be a pull payment or a push payment. All these aspects which put a transaction in a wider
context and link it to real-world interactions, contracts, and motivations between parties make a transaction into a payment. A
[cyclic transaction](#cyclic-transaction) is generally not considered a payment, because the sending party of such a transaction is also the receiving party.

## Payment channel
A payment channel is similar to a trustline, but each party parks an asset on an "underlying ledger", equal to the other party's maximum balance.
Apart from disputes canceling each other out, and out-of-band settlement, disputes can be settled by each party claiming the other party's parked
asset. This way, two strangers can run a ledger with properties similar to a trustline, but without fully trusting each other.

## Peer balance
See [trustline](#trustline).

## Peering
Connectors "peer" with one another to exchange information used to determine the best route for a payment, and to route it (connect its chain of
transfers through [forward](#forward-action) and [backward](#backward-action) action.
A peering relationship is usually implemented using either a [trustline](#trustline) or a [payment channel](#payment-channel)..

## Prepared
The second state of an interledger transaction, after [proposed](#proposed), and
before its [final state](#final-state).

## Pre-Shared Key (PSK)
The recommended transport layer for most use cases.
In Pre-Shared Key (PSK) protocol, the sender and receiver use a shared secret to generate the payment condition, authenticate the ILP packet, and encrypt application data. Using PSK, the sender is guaranteed that fulfillment of their transfer indicates the receiver got the payment, provided that no one aside from the sender and receiver have the secret and the sender did not submit the fulfillment. See [IL-RFC 16](../0016-pre-shared-key/0016-pre-shared-key.md).

## Proposed
The initial state of an interledger transaction, before [prepared](#prepared).

## Receiver
The party who is the beneficiary (payee) of a transfer or a transaction (payment).

## Route
A list of [transfers](transfer) and the [hops](#hop) between them.

## Routing table
A lookup table, used by a connector to decide who their outgoing receiver should be (the next
hop, which may be another connector, or the destination receiver), and what the lowest outgoing
amount is which they can safely offer for the outgoing transfer (equal to the destination amount in case
the outgoing is the destination transfer).

## Sender
The party who is the initiator (payer) of a transfer or a transaction (payment).

## Simple Payment Setup Protocol (SPSP)
SPSP uses Webfinger ([RFC 7033](https://tools.ietf.org/html/rfc7033)) and an HTTPS-based protocol for communicating account, amount, and Pre-Shared Key details from the receiver to the sender.
See [IL-RFC 9](../0009-simple-payment-setup-protocol/0009-simple-payment-setup-protocol.md).

## Source (transfer, ledger, amount)
The transfer/ledger/amount directly adjacent to the sender.

## Transaction
The movement of assets from one party to another. A transaction consists of one or more [ledger transfers](#transfer). Roughly synonymous with
[payment](#payment).

# Transfer
The movement of assets on one single ledger. Multiple transfers can be chained together into one multi-hop [transaction](#transaction).
Also sometimes called 'ledger transfer'.

## Transport layer
The middle layer of the Interledger protocol stack, which can be either
either [Pre-Shared Key](#psk) or [Interledger Payment Request](#ipr). It is called the transport layer because
it provides the end-to-end hashlock condition which ultimately connects the hops that chain the individual
ledger transfers together, into one Interledger transaction.

## Trustline
A trustline between two participating parties (participants) is a ledger with two accounts, where each participant keeps track of their
own account balance, but the parties do agree on an [asset](#asset) (unit of value) for expressing balance and transfer amounts.
At any time, the party that has a negative balance may unilaterally close the trustline, leaving the other party at a loss. This is why
participating parties set their own maximum balance, thus limiting their own exposure to risk.
A transfer on a trustline must have one of the participants as the sender, the other participant as the receiver, and an amount such that
its execution would not make the receiver's account go above their maximum balance.
When the two parties disagree over a given transfer - whether it was executed or canceled,
what the amount was, which participant played the sender role, or even whether it happened at all -
each party updates their own balance as they see fit, meaning the two balances will no longer add up to zero.
The participants can ask each other to report their own balance. The other participant's balance, reported this way, is called the 'peer balance'.
Suppose Alice has a balance of 10 (maximum: 15), and Bob reports a peer balance to her of -8 (maximum: 15).
She should then consider her total balance to consist of a cleared balance of 8, and a disputed balance of 2.
In this situation, Bob should consider his total balance to be -10, and his disputed balance to be 2.
The sum of the balances has now decreased to -10+8 = -2, and the trustline's total liquidity has now decreased from (15+15) to (15+15-2).
The disputed balance can decrease again when disputes cancel each other
out, be settled out-of-band by the parties, or left to grow as the parties periodically increment the other account's minimum balance.
