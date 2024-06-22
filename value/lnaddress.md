## The 'lnaddress' Recipient Type

An `lnaddress` address is a 
"[lightning address](https://github.com/andrerfneves/lightning-address/blob/master/DIY.md)" that resolves to the uri of
a payment options file which contains one or more payment destination addresses.  For example, an `lnaddress` of 
"johndoe@example.com" would be converted to the uri "https://example.com/.well-known/lnurlp/johndoe/options" which MUST
contain an appropriately formatted JSON file.

## Options File

The payment options file MUST contain a JSON object that holds exactly one `options` array.  The purpose of the payment
options file is to hold all the various payment receipt methods (and corresponding destination info for each) that a
user has available to them.  It looks like this:

```json
{
  "status": "OK",
  "options": [
    {
      "type": "lnurlp",
      "callback": "https://example.com/v1/lnurlp/johndoe/pay",
      "minSendable": 1000,
      "maxSendable": 1000000000000,
      "metadata": "[[\"text/plain\",\"Pay @johndoe on example wallet\"]]",
      "commentAllowed": 500
    },
    {
      "type": "keysend",
      "pubkey": "03b6f613e88bd874177c28c6ad83b3baba43c4c656f56be1f8df84669556054b79",
      "customData": [
        {
          "customKey": "906608",
          "customValue": "01hIWsCYxdBJzlDvu5zpT3"
        }
      ]
    }
  ]
}
```

### Options array

Each member of the `options` array is a distinct payment method describing a payment destination for a user.  The 
properties available for an "option" depends on it's `type` and should be taken from the definition of that type as it
exists in the primary documentation.  Using the above example, we can see that there are two payment options defined 
for user John Doe - `lnurlp` and `keysend`.  The structure and properties for `lnurlp` are defined 
[here](https://github.com/lnurl/luds/blob/luds/06.md), while the structure and properties for `keysend` are defined 
[here](#keysend-option).

## Keysend option

The `keysend` payment option has a type of `keysend` and has the following properties:

* `pubkey`(required) - The public address of the lightning node that will receive the keysend payment.
* `customData` - An array that contains two members:
  * `customKey` - A text value that defines a routing key for the receiving node.
  * `customValue` - A text value that defines a routing value for the receiving node.

The `customData` array is optional for cases where the receiving node is a front-end for a multi-wallet 
system.  In this context, the `customKey` will be what the receiving system looks up inside the payment in order to 
retreive the `customValue` which is a virtual wallet identifier.

## BOLT12 option

Under development...