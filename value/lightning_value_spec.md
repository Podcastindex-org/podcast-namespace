# Lightning Value Block Specification

The *Lightning Network* (LN) was the first Value Block specification to see wide adoption.

## Version 2.0.0

The following extensions to the Lightning Value Block Specification are added:

For the `<podcast:value>` tag, the following attributes MUST be used:

 - `type` (required): "lightning"
 - `method` (required): "keysend" or "amp"
 - `suggested` (optional): A float representing a BTC amount.
        e.g. 0.00000005000 is 5 Sats.
 - `version` (required): `"2.0.0"`

For the `<podcast:valueRecipient>` tag, the following attributes MUST be used:

 - `type`: "node"
 - `address`: \<the destination node's pubkey\>
 - `split`: \<the number of shares\>
 - `lnaddress` (optional): \<the destination node's Lightning Address\>

The `lnaddress`, which resembles an email address, should resolve to an http GET request as follows:

`lnaddress`: `podcastindexorg@v4v.app`

 resolves to:

 `https://v4v.app/.well-known/keysend/podcastindexorg`

 Which returns:

 ```json
 {
  "status": "OK",
  "tag": "keysend",
  "pubkey": "0266ad2656c7a19a219d37e82b280046660f4d7f3ae0c00b64a1629de4ea567668",
  "customData": [
    {
      "customKey": 818818,
      "customValue": "podcastindexorg"
    }
  ]
}
```

## Version 1.0.0

For the `<podcast:value>` tag, the following attributes MUST be used:

 - `type` (required): "lightning"
 - `method` (required): "keysend" or "amp"
 - `suggested` (optional): A float representing a BTC amount.
        e.g. 0.00000005000 is 5 Sats.

For the `<podcast:valueRecipient>` tag, the following attributes MUST be used:

 - `type`: "node"
 - `address`: \<the destination node's pubkey\>
 - `split`: \<the number of shares\>

If the receiving Lightning node, or service, requires application specific data to be sent with the payment in the lightning  message `extension` (a _TLV stream_, see the Appendix section), the `customKey` and `customValue` can be utilized as follows:

 - `type`: "node"
 - `method`: "keysend" or "amp"
 - `customKey`: \<tlv record type, a 64 bit integer greater than or equal to 2^16\>
 - `customValue`: \<tlv record value, a string\>
 - `address`: \<the destination node's pubkey\>
 - `split`: \<the number of shares\>

 When sending a payment containing application specific data, the client must use UTF-8 as encoding for `customValue`.

 **Remarks:**

 - `customValue` is specified as a string due to the emergence of known users for this field (see Appendix). If we decide to support raw binary data in the future, a new attribute can be introduced to indicate the different behavior
 - There is at least one known shared node ([satoshis.stream](https://satoshis.stream/)) that requires, in addition to this specification, the inclusion of the TLV record with type `7629169`, as defined [here](https://github.com/satoshisstream/satoshis.stream/blob/main/TLV_registry.md#field-7629169), in order to correctly route the payment to the corresponding receiver

<br>

##### Example

This is a live, working example of a Lightning keysend value block in production.  It designates four recipients for payment - two
podcast hosts at 49 and 46 shares respectively, a producer working on per episode chapter creation who gets a 5 share, and
a single share (effectively 1%) fee to the Podcastindex.org API.
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

##### Example: `<Item>` Override

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

##### Appendix

Lightning payments are performed using lightning messages as specified in [BOLT #1: Base Protocol](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md).

One part of the message is the `extension`, a TLV (Type-Length-Value) stream. Application specific data can be added to transactions using _custom records_ on the TLV Stream.

A community maintained registry of known custom record types and formats, governed by satoshis.stream, can be found at the document [TLV record registry](https://github.com/satoshisstream/satoshis.stream/blob/main/TLV_registry.md). In special, the section _Fields used in customKey / customValue Pairs_ documents the known use cases for the `customKey` and `customValue` attributes.
