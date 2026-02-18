# The 'lnaddress' Recipient Type

The `lnaddress` recipient type refers to a "[Lightning Address](https://github.com/andrerfneves/lightning-address/blob/master/DIY.md)", which is an email-like address used to resolve a recipient's `lnurlp` and `keysend` details from "well-known" URIs.

The username and domain of the Lightning Address is used to construct the well-known URI, for example:

* [LNURLp](#lnurlp) info for "johndoe@example.com" would be located at the URI: "https://example.com/.well-known/lnurlp/johndoe".
* [Keysend](#keysend) info for "johndoe@example.com" would be located at the URI: "https://example.com/.well-known/keysend/johndoe".

## LNURLp

Provides information to request a BOLT11 invoice as defined by the [LUD-06: payRequest spec](https://github.com/lnurl/luds/blob/luds/06.md).

```json
{
  "tag": "payRequest",
  "status": "OK",
  "callback": "https://example.com/lnurlp/johndoe/",
  "minSendable": 1000,
  "maxSendable": 16000000000,
  "metadata": "[[\"text/plain\",\"Pay johndoe\"],[\"text/identifier\",\"johndoe@example.com\"]]",
  "commentAllowed": 200
}
```

See [LUD-06](https://github.com/lnurl/luds/blob/luds/06.md) and [LUD-12](https://github.com/lnurl/luds/blob/luds/12.md) for more details.

## Keysend

Provides public key and custom data fields needed to send a keysend payment.

```json
{
  "status": "OK",
  "tag": "keysend",
  "pubkey": "0123456789abcdeffedcba9876543210123456789abcdeffedcba9876543210123",
  "customData": [
    {
      "customKey": "696969",
      "customValue": "1"
    }
  ]
}
```

### Fields

* `tag` - Indicates a "keysend" response
* `status` - Status of response (either "OK" or "ERROR" with `reason` field)
* `pubkey` - The public address of the lightning node that will receive the keysend payment.
* `customData` - An array that contains routing info for the receiving node:
  * `customKey` - A text value that defines a routing key for the receiving node.
  * `customValue` - A text value that defines a routing value for the receiving node.

The `customData` array is optional for cases where the receiving node is a front-end for a multi-wallet 
system.  In this context, the `customKey` will be what the receiving system looks up inside the payment in order to 
retrieve the `customValue` which is a virtual wallet identifier.