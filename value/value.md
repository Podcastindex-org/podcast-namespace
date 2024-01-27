# The "podcast:value" Specification

<small>Version 1.4 by [Dave Jones](https://github.com/daveajones) - with [Gigi](https://github.com/dergigi)
, [Evan Feenstra](https://github.com/evanfeenstra) & [Paul Itoi](https://github.com/pitoi)</small><br>
<small>September 1st, 2021</small>

<br>

## Purpose

Here we describe an additional "block" of XML that gives podcasters (and other media creators) a way to receive direct
payments from their audience in response to the action of viewing or listening to a created work. Utilizing so called "Layer-2"
cryptocurrency networks like Lightning, and possibly other digital currency mechanisms, near real-time micropayments can
be directly sent from listener or viewer to the creator without the need for a payment processor or other "middle men".

The value block designates single or multiple destinations for these micro-payments. The idea is that the creator of the
media
describes (within the feed) where and how they would like payments to be sent back to them during consumption
of that media. The format is designed to be flexible enough to handle many scenarios of direct payment streaming. Even
the use of fiat currency, if there is an API that is capable of interfacing as a receiver within this format.

<br>

## Play to Pay

This system can be thought of as Play-to-pay, rather than the traditional Pay-to-play paywall approach. When a
media listener (such as within a podcast app) presses the play button on an episode whose feed contains a value
block, a compatible app will be expected to begin streaming micro-payments to the designated recipients on a time
interval that makes sense for the app. The amount of each payment can be any amount the listener chooses, including
zero. Alternatively, the listener can send one-time payments (referred to as "boosts" in this document).
If the listener chooses not to pay to listen to this media, then the app can ignore the value block of that feed.

<br>

## Payment Intervals

The "time interval" for calculating streaming payments is **always 1 minute**. However, the actual interval between when payments
are sent can be longer. The interval should be chosen with a few factors in mind such as connectivity (is the app
currently online?), transaction fees (batch payments together to reduce fee percentage), cryptocurrency network load
(can the given crypto network or API support this payment rate?).

No matter what the chosen time interval for the actual transaction, the calculation should be done on a once-per-minute
basis. So, if the micro-payment is sent every 15 minutes, it should be calculated as 15 payments batched together
in a single transaction. Likewise, some apps with limited connectivity may need to only send payments once per
hour. In that scenario, there would be 60 payments added up into a single, larger payment. Batching transactions
like this also helps to minimize the impact of transaction fees on the underlying cryptocurrency network.

Note that playback speed is not a factor in this calculation. The "one minute payment interval" refers to the minutes
that make up the total runtime of the content, thus all payment calculations are independent of playback speed.

<div class="page"/>

<br><br>

## Elements

There are two elements that make up what we refer to as the "value block". They are a parent element (`<podcast:value>`) that specifies the
currency to use, and one or more child elements (`<podcast:valueRecipient>`) that specify who to pay using the currency and protocol described by the
parent.

<br>

### Value Element

The `<podcast:value>` tag designates the cryptocurrency or payment layer that will be used, the transport method for
transacting
the payments, and a suggested amount denominated in the given cryptocurrency.

This element can exist at either the `<channel>` or `<item>` level.

<br>

#### Structure:

```xml

<podcast:value
        type="[cryptocurrency or layer(string)]"
        method="[payment transport(string)]"
        suggested="[payment amount(float)]"
>
    [one or more "podcast:valueRecipient" elements]
</podcast:value>
```

<br>

#### Attributes:

- `type` (required) This is the service slug of the cryptocurrency or protocol layer.
- `method` (required) This is the transport mechanism that will be used.
- `suggested` (optional) This is an optional suggestion on how much cryptocurrency to send with each payment.

<br>

#### Explanation:

Using Lightning as an example, the `type` would be `"lightning"`. Various possible `type` values will be kept
in a slug list [here](valueslugs.txt). This is the only type currently in active use. Others are under development
and will be added to the list as they see some measure of adoption, or at least a working example to prove viability.

The `method` attribute is used to indicate a sub-protocol to use within the given `type`. Again, returning to
Lightning as an example, the `method` would be `"keysend"`. Normally, a Lightning payment requires an invoice
to be generated by the payee in order to fulfill a transaction. The `"keysend"` protocol of Lightning allows payments
to be streamed to what is, essentially, an open invoice. Other cryptocurrencies may have a similar protocol that
would be used here. If not, a value of `"default"` should be given.

The `"suggested"` amount is just that. It's a suggestion, and must be changeable by the user to another value, or
to zero. The suggested amount depends on the payment protocol being used. For instance, with Lightning on the Bitcoin
network, the amount can be as low as one millisatoshi, expressed as `0.00000000001` BTC.

A single value tag can contain many `<podcast:valueRecipient>` tags as children. All of these given recipients are
sent individual payments when the payment interval triggers.

The value tag, when it exists at the `<channel>` level, designates the payment scheme for the entire podcast. When it
exists at the `<item>` level, it is intended to override the channel level tag for that episode only.

#### Example:

The following example uses the `keysend` method of the `lightning` network and
sets the suggested value to 5 sats. A sat, short for satoshi, is a
one-hundred-millionth of a single bitcoin (0.00000001 BTC). The smallest unit on
the Lightning Network is a millisat, which is a thousandth of a sat.

```xml

<podcast:value
        type="lightning"
        method="keysend"
        suggested="0.00000005000"
></podcast:value>
```

<br><br>

<div class="page"/>

### Value Recipient Element

The `valueRecipient` tag designates various destinations for payments to be sent to during consumption of the enclosed
media. Each recipient is considered to receive a "split" of the total payment according to the number of shares given
in the `split` attribute.

This element may only exist within a parent `<podcast:value>` element.

There is no limit on how many `valueRecipient` elements can be present in a given `<podcast:value>` element.

<br>

#### Structure:

```xml

<podcast:valueRecipient
        name="[name of recipient(string)]"
        type="[address type(string)]"
        address="[the receiving address(string)]"
        customKey="[optional key to pass(mixed)]"
        customValue="[optional value to pass(mixed)]"
        split="[share count(int)]"
        fee=[true|false]
        />
```

<br>

#### Attributes:

- `name` (recommended) A free-form string that designates who or what this recipient is.
- `customKey` (optional) The name of a custom record key to send along with the payment.
- `customValue` (optional) A custom value to pass along with the payment. This is considered the value that belongs to
  the `customKey`.
- `type` (required) A slug that represents the type of receiving address that will receive the payment.
- `address` (required) This denotes the receiving address of the payee.
- `split` (required) If `fee=false`, the number of shares for the recipient; if `fee=true`, the percentage of the payment that the recipient will receive.
- `fee` (optional) Indicates whether the recipient should instead receive a percentage amount off the top of each payment. If this attribute is not specified, it is assumed to be false.

<br>

#### Explanation:

The `name` is just a human readable description of who or what this payment destination is. This could be something
simple like
"Podcaster", "Co-host" or "Producer". It could also be more descriptive like "Ronald McDonald House Charity", if a
podcaster
chooses to donate a percentage of their incoming funds to a charity.

The `type` denotes what type of receiving entity this is. For instance, with lightning this would typically be `"node"`.
This would
indicate that the `address` attribute for this recipient is a Lightning node that is capable of directly receiving
incoming keysend payments. Valid values for
the `type` attribute are kept in the accompanying file [here](valueslugs.txt). Another option is given in examples
below.

Payments must be sent to a valid destination which is given in the `address` attribute. This address format will vary
depending on
the underlying currency being used.

The meaning of `split` depends on the value of `fee`:
- When `fee=true`, `split` denotes the percent value that should be taken off each transaction for the recipient.
- When `fee=false`, `split` denotes the number of "shares" for the recipient—splits among non-fee recipients represent the ratios in which the remaining amount (*after* the `fee=true` recipients receive their payments) must be distributed.

`fee=true` is the preferred way for service providers such as apps, hosting companies, APIs, and third-party value-add providers to add their fee to a value block.
Because when `fee=true`, `split` represents *percentage* value, the sum of splits for `fee=true` recipients cannot exceed 100.

#### Custom Key/Value Pairs

The `customKey` and `customValue` pair can be used (especially for the Lighning Network) to help a receiving application
route or process payments that have all arrived at a single node.

The idea is that Podcast Index will parse and store and all client apps will always send a `customKey:customValue` pair
if these are found in the Value Block.

For example, the `customKey`'s of `818818`, `112111100` are used to route payments to Hive accounts or specific wallets
on LNPay respectively. These fields are
documented [in the list maintained by Satoshis Stream](https://github.com/satoshisstream/satoshis.stream/blob/main/TLV_registry.md)
.

If your specific application would benefit from your own `customKey:customValue` pair which will be passed along from
the player to your app, and for which nothing already exists, add your own.

<br><br>

<div class="page"/>

### Payment calculation

When all combined, together the recipients will receive
```
(Batch payout) = (Listener payment) * (Interval count)
```

`Listener payment` and `Interval count` refer to one of the following scenarios:
- either the amount of a one-time payment made by the listener (with `Interval count` usually being 1)
- or the amount being streamed per-minute by the listener (with `Interval count` referring to the interval in minutes at which payments are sent by their client)

How `Batch payout` is divided between the recipients depends on their `split` and `fee` values.

For `fee=true` recipients $X_1, X_2, \ldots, X_m$ with splits $x_1, x_2, \ldots, x_m$ and `fee=false` recipients $Y_1, Y_2, \ldots, Y_n$ with splits $y_1, y_2, \ldots, y_n$:

1. Firstly, `fee=true` recipients $X_1, X_2, \ldots, X_n$ will receive $x_1\\%$, $x_2\\%$, $\ldots$, $x_n\\%$ of the total batch payout.
2. Then, the remaining amount will be distributed among `fee=false` recipients $Y_1, Y_2, \ldots, Y_m$ in the ratio $y_1 : y_2 : \ldots : y_m$.

#### Example 1: no fees, splits add up to 100

```xml

<podcast:value type="lightning" method="keysend" suggested="0.00000015000">
    <podcast:valueRecipient
            name="A"
            type="node"
            address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
            split="50"
    />
    <podcast:valueRecipient
            name="B"
            type="node"
            address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
            split="40"
    />
    <podcast:valueRecipient
            name="C"
            type="node"
            address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
            split="10"
    />
</podcast:value>
```

This block designates three payment recipients.
On each timed payment interval, the total payment will be split into 3 smaller payments according to the `split` of each recipient.
Because there are no `fee=true` recipients, step 1 of the calculation can be skipped—the splits represent the ratios in which the payment should be divided.
I.e., for recipients `A`, `B`, and `C`, the payment should be divided in the ratio $50:40:10$:
- `A` will receive $\dfrac{50}{50 + 40 + 10} = 0.5 = 50\\%$ of the payment
- `B` will receive $\dfrac{40}{50 + 40 + 10} = 0.4 = 40\\%$ of the payment
- `C` will receive $\dfrac{10}{50 + 40 + 10} = 0.1 = 10\\%$ of the payment

In this case, because splits add up to 100 (that is, $50 + 40 + 10 = 100$), the splits happen to correspond to the percentage values that each recipient will receive.
However, in general, it should never be assumed or expected that splits add up to 100.

Suppose that the listener chose to pay 300 sats per minute with the app sending out the payments each minute. Then:

Batch payout: **300 sats**
  
- Recipient `A` gets a payment of 150 sats (calculated using $300 \cdot \dfrac{50}{50 + 40 + 10} = 150$)
- Recipient `B` gets a payment of 120 sats (calculated using $300 \cdot \dfrac{40}{50 + 40 + 10} = 120$)
- Recipient `C` gets a payment of 30 sats (calculated using $300 \cdot \dfrac{10}{50 + 40 + 10} = 30$)

If an app chooses to only make a payout once every 5 minutes of listening/watching, the calculation would be the same after multiplying the per-minute payment by 5:

Batch payout: **1500 sats** (calculated using $5 \cdot 300 = 1500$)

- Recipient `A` gets a payment of 750 sats (calculated using $1500 \cdot \dfrac{50}{50 + 40 + 10} = 750$)
- Recipient `B` gets a payment of 600 sats (calculated using $1500 \cdot \dfrac{40}{50 + 40 + 10} = 600$)
- Recipient `C` gets a payment of 150 sats (calculated using $1500 \cdot \dfrac{10}{50 + 40 + 10} = 150$)

As shown above, the once per minute calculation does not have to actually be sent every minute.
A longer payout period can be chosen. But, the once-per-minute nature of the payout still remains in order for listeners and creators to have an easy way to measure and calculate how much they will spend and charge.

#### Example 2: no fees, splits don't add up to 100

```xml

<podcast:value type="lightning" method="keysend" suggested="0.00000015000">
    <podcast:valueRecipient
            name="A"
            type="node"
            address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
            split="100"
    />
    <podcast:valueRecipient
            name="B"
            type="node"
            address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
            split="99"
    />
    <podcast:valueRecipient
            name="C"
            type="node"
            address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
            split="1"
    />
</podcast:value>
```

Again, there are no `fee=true` recipients, so step 1 of the calculation can be skipped.
The splits represent the ratios in which the payment should be divided.
I.e., for recipients `A`, `B`, and `C`, the payment should be divided in the ratio $100:99:1$:
- `A` will receive $\dfrac{100}{100 + 99 + 1} = 0.5 = 50\\%$ of the payment
- `B` will receive $\dfrac{99}{100 + 99 + 1} = 0.495 = 49.5\\%$ of the payment
- `C` will receive $\dfrac{1}{100 + 99 + 10} = 0.005 = 0.5\\%$ of the payment

This example demonstrates why splits should not be required to add up to 100—it allows for recipients to receive payments that are not just integer percent values.
In this specific case, it allows `B` to receive $49.5\\%$ and `C` to receive $0.5\\%$.

Suppose that the listener chose to send a 1000-sat boost, which the app decides to distribute immediately among the recipients:

Batch payout: **1000 sats**
  
- Recipient `A` gets a payment of 500 sats (calculated using $1000 \cdot \dfrac{100}{100 + 99 + 1} = 500$)
- Recipient `B` gets a payment of 495 sats (calculated using $1000 \cdot \dfrac{99}{100 + 99 + 1} = 495$)
- Recipient `C` gets a payment of 5 sats (calculated using $1000 \cdot \dfrac{1}{100 + 99 + 1} = 5$)

#### Example 3: recipients with `fee=true`

```xml

<podcast:value type="lightning" method="keysend" suggested="0.00000015000">
    <podcast:valueRecipient
            name="A"
            type="node"
            address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
            split="101"
    />
    <podcast:valueRecipient
            name="B"
            type="node"
            address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
            split="99"
    />
    <podcast:valueRecipient
            name="C"
            type="node"
            address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
            split="16"
            fee="true"
    />
    <podcast:valueRecipient
            name="D"
            type="node"
            address="039e2bd74dadb4a50c3be632a5068cbc820d520635acc5c527a2171ab6cab43a9c"
            split="4"
            fee="true"
    />
</podcast:value>
```

In this case, there are two `fee=true` recipients, so the calculation will have to be done in two steps:

1. `C` will receive $16\\%$ and `D` will receive $4\\%$ off the top of each payment.
2. The rest of the payment (i.e., $100\\% - 16\\% - 4\\% = 80\\%$) will be distributed among `A` and `B` in the ratio $101 : 99$.

Suppose that the listener chose to send a 2500-sat boost, which the app decides to distribute immediately among the recipients:

Batch payout: **2500 sats**

1. `fee=true` recipients are paid first:
  - Recipient `C` gets a payment of 400 sats (calculated using $2500 \cdot 0.16 = 400$)
  - Recipient `D` gets a payment of 100 sats (calculated using $2500 \cdot 0.04 = 100$)
2. After that, `fee=false` recipients get the remaining amount of 2000 sats (calculated using $2500 - 400 - 100 = 2000$):
  - Recipient `A` gets a payment of 1010 sats (calculated using $2000 \cdot \dfrac{101}{101 + 99} = 1010$)
  - Recipient `B` gets a payment of 990 sats (calculated using $2000 \cdot \dfrac{99}{101 + 99} = 990$)

<br><br>

<div class="page"/>

### Supported Currencies and Protocols

The value block is designed to be flexible enough to handle any cryptocurrency, and even fiat currencies with a
given
API that exposes a compatible process.

Currently, development is centered mostly on [Lightning](https://github.com/lightningnetwork) using the `"keysend"`
method. Keysend allows for push
based payments without the recipient needing to generate an invoice to receive them.
Another method to send spontaneous payments via Lightning is AMP, atomic
[multipath payments][AMP]. AMP supersedes keysend in many ways and allows for
more robust and larger payments. However, it is still in beta and thus not
widely implemented as of now.

[AMP]: https://bitcoinops.org/en/topics/multipath-payments/

<br>

#### Lightning

For the `<podcast:value>` tag, the following attributes MUST be used:

- `type` (required): `"lightning"`
- `method` (required): `"keysend"` or `"amp"`
- `suggested` (optional): A float representing a BTC amount.
  e.g. 0.00000005000 is 5 Sats.

For the `<podcast:valueRecipient>` tag, the following attributes MUST be used:

- `type`: "node"
- `address`: \<the destination node's pubkey\>
- `split`: \<the number of shares\>

If the receiving Lightning node, or service, requires application specific data to be sent with the payment in the
lightning message `extension` (a _TLV stream_, see the Appendix section), the `customKey` and `customValue` can be
utilized as follows:

- `type`: "node"
- `method`: "keysend" or "amp"
- `customKey`: \<tlv record type, a 64 bit integer greater than or equal to $2^{16}$ \>
- `customValue`: \<tlv record value, a string\>
- `address`: \<the destination node's pubkey\>
- `split`: \<the number of shares\>

When sending a payment containing application specific data, the client must use UTF-8 as encoding for `customValue`.

**Remarks:**

- `customValue` is specified as a string due to the emergence of known users for this field (see Appendix). If we decide
  to support raw binary data in the future, a new attribute can be introduced to indicate the different behavior
- There is at least one known shared node ([satoshis.stream](https://satoshis.stream/)) that requires, in addition to
  this specification, the inclusion of the TLV record with type `7629169`, as
  defined [here](blip-0010.md), in order to correctly route the payment to the corresponding receiver

<br>

##### Example

This is a live, working example of a Lightning keysend value block in production. It designates four recipients for
payment - two
podcast hosts at 49 and 46 shares respectively, a producer working on per episode chapter creation who gets a 5 share,
and 1% to the Podcastindex.org API.
Since the value block is defined at the `<channel>` level, it applies to every podcast episode.

```xml
...
<channel>
    <podcast:value type="lightning" method="keysend" suggested="0.00000015000">
        <podcast:valueRecipient
                name="Adam Curry (Podcaster)"
                type="node"
                address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
                split="49"
        />
        <podcast:valueRecipient
                name="Dave Jones (Podcaster)"
                type="node"
                address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
                split="46"
        />
        <podcast:valueRecipient
                name="Dreb Scott (Chapter Creation)"
                type="node"
                address="02dd306e68c46681aa21d88a436fb35355a8579dd30201581cefa17cb179fc4c15"
                split="5"
        />
        <podcast:valueRecipient
                name="Podcastindex.org (Donation)"
                type="node"
                address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
                split="1"
                fee="true"
        />
    </podcast:value>
    ...
    <item>...</item>
    <item>...</item>
    ...
</channel>
```

To use Atomic Multipath Payments (AMP) instead of `keysend`, simply set the
payment `method` to `amp`:

```xml
...
<channel>
    <podcast:value type="lightning" method="amp" suggested="0.00000015000">
        ...
    </podcast:value>
</channel>
```

##### Example: `<item>` Override

To set up different payment splits for individual episodes, a value block has to
be defined on the `<item>` level. This will override the value settings set on
the `<channel>` level.

The following example defines different value blocks for each episode in order
to include the guests as value recipients. Payments are split 50/50 between host
and guest.

```xml
...
<channel>
    <podcast:value type="lightning" method="keysend" suggested="0.00000021000">
        <podcast:valueRecipient
                name="John Vallis (Host)"
                type="node"
                address="02a9cd2bca29dd7e29bdfdf485a8e78b8ccf9327517afa03a59be8f62a58792e1b"
                split="100"
        />
    </podcast:value>
    ...
    <item>
        <title>#00 - John's Solo Episode</title>
        ...
    </item>
    <item>
        <title>#01 - John and Gigi</title>
        <podcast:value type="lightning" method="keysend" suggested="0.00000021000">
            <podcast:valueRecipient
                    name="John Vallis (Host)"
                    type="node"
                    address="02a9cd2bca29dd7e29bdfdf485a8e78b8ccf9327517afa03a59be8f62a58792e1b"
                    split="50"
            />
            <podcast:valueRecipient
                    name="Gigi (Guest)"
                    type="node"
                    address="02e12fea95f576a680ec1938b7ed98ef0855eadeced493566877d404e404bfbf52"
                    split="50"
            />
        </podcast:value>
        ...
    </item>
    <item>
        <title>#02 - John and Paul</title>
        <podcast:value type="lightning" method="keysend" suggested="0.00000021000">
            <podcast:valueRecipient
                    name="John Vallis (Host)"
                    type="node"
                    address="02a9cd2bca29dd7e29bdfdf485a8e78b8ccf9327517afa03a59be8f62a58792e1b"
                    split="50"
            />
            <podcast:valueRecipient
                    name="Paul Itoi (Guest)"
                    type="node"
                    address="03a9a8d953fe747d0dd94dd3c567ddc58451101e987e2d2bf7a4d1e10a2c89ff38"
                    split="50"
            />
        </podcast:value>
        ...
    </item>
    ...
</channel>
```

<br>

### Appendix A - TLV Records and Extensions

Lightning payments are performed using lightning messages as specified
in [BOLT #1: Base Protocol](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md).

One part of the message is the `extension`, a TLV (Type-Length-Value) stream. Podcast specific data can be added to
transactions using the key **7629169** with the value described in [bLIP 10](blip-0010.md)

A community maintained registry of additional known custom record types and formats, governed by satoshis.stream, can be
found at the document [TLV record registry](https://github.com/satoshisstream/satoshis.stream/blob/main/TLV_registry.md)
. In special, the section _Fields used in customKey / customValue Pairs_ documents the known use cases for
the `customKey` and `customValue` attributes.

<br>

### Appendix B - Payment Actions

There are currently 3 payment "actions" that are defined in the BLIP-10 TLV extension that is embedded in the payment
payload:  "stream", "boost" and "auto".

* `stream` - This means the payment is a timed interval payment (i.e. - every minute) that is sent or queued while the
             user is engaged in active listening/viewing of the content.
* `boost`  - This means the payment is a user generated one-time payment that happens in response to a user initiated
             action like a "Boost" button push, or some other clearly labeled payment initiation function.  These
             types of payments can contain textual messages (i.e. - a boostagram).
* `auto`   - This means the payment was an app initiated payment that recurs at a specific long-form interval such as 
             weekly, monthly, yearly, etc.  The user configures an interval and payment amount in the app and the app
             then sends that amount at the specified time when each interval passes.
