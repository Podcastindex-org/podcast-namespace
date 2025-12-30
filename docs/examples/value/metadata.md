## RSS Payment Metadata

Originally the `<podcast:value>` spec used keysend to facilitate payments. However there is now consensus within the Podcasting 2.0 community that keysend is not a long-term solution because many of the most popular Lightning Wallets such as Strike, CashApp, Primal, Wallet of Satoshi do not support it.

The [Lightning Address](https://lightningaddress.com) has also become the de-facto standard for sharing payment information in an easy way.

**"Like an email address, but for money"** - is an easy to understand concept for anyone even if they are new to Bitcoin, and asking someone to share their Lightning Address is simple.

Because of this the Podcasting 2.0 spec has been updated to use `type="lnaddress"` - allowing podcasters to simply add one or more Lightning Address into their RSS feed to instruct apps where to send payments:

```xml
<podcast:value type="lightning">
  <podcast:valueRecipient
    name="Oscar"
    type="lnaddress"
    address="merryoscar@fountain.fm" 
    split="100"
  />
</podcast:value>
```

The final piece to the puzzle is how to share payment metadata such as the show / episode info, sender info, and message, as Lightning Address payments have a maximum description size of around 200 characters.

This document outlines a simple spec for sharing RSS payment metadata that will not require any changes from wallets.

---

### Goals:

- **for sending wallets** - should work out of the box without requiring any changes
- **for receiving wallets:**
  - should give users some basic information about what the payment is related to without the wallet needing to make any changes
  - should allows programmatic parsing of the metadata so that wallets that care about RSS payments (Fountain, Alby, Helipad) can display and aggregate it

---

### Spec:

The `rss::payment` metadata spec uses a prefix in the payment description field, combined with http headers in a linked url, to deliver full payment metadata to services that care about it, whilst giving receiving users on any wallet an easily readable description for the payment:

`rss::payment::{action} {url} {truncated message}`

`rss::payment::boost https://fountain.fm/show/2JCkApiUwyBj2eOG7JJI?payment=DBBhBwUise1bcfoHRCC3 Great episode!`

As well as displaying the episode content, the url (https://fountain.fm/show/2JCkApiUwyBj2eOG7JJI?payment=DBBhBwUise1bcfoHRCC3) will also return an `x-rss-payment` http header, that contains the full structured metadata for the payment as a URI encoded JSON string:

```js
// `x-rss-payment` response header encoded as a uri component
encodeURIComponent({
  "id": "DBBhBwUise1bcfoHRCC3", // payment id (optional)
  "group": "EoL2Qp52RMOSiF1rmGGs", // payment group id (optional)
  
  "action": "boost", // payment action (boost, stream)
  "split": 0.25, // the split as a decimal percentage
  "message": "Great episode!", // message (optional)

  "app_name": "Fountain", // (recommended)
  "app_version": "1.3.10", // (optional)

  "sender_id": "IIaS9X2JuH75yvrRcFoL", // (optional)
  "sender_name": "oscar@fountain.fm", // (optional)

  "recipient_name": "merryoscar@primal.net", // (optional)
  "recipient_address": "merryoscar@primal.net", // (optional)

  "value_msat": 25000, // the amount in millisatoshis
  "value_msat_total": 100000, // the total amount for all splits in millisatoshis
  "value_usd": 0.025797, // the amount in usd (optional)
  "timestamp": "2025-11-05T15:09:10.174Z", // ISO 8601 timestamp
  "position": 120, // episode position in seconds (optional)

  "feed_guid": "",
  "feed_title": "",
  "item_guid": "",
  "item_title": "",
  "publisher_guid": "",
  "publisher_title": "",
  "remote_feed_guid": "",
  "remote_item_guid": "",
  "remote_publisher_guid": ""
})
```
 
---

### Implementation:

- **for apps:**
  - add `rss::payment::{action} {url} {truncated message}` to the BOLT11 description field
  - add the `x-rss-payment` response header to the url
- **for receiving wallets that care about rss payment metadata:**
  - look for payments with description fields that start with `rss::payment` and contain a url
  - load the url and parse the `x-rss-payment` response header
  - build a sexy analytics dashboard, push messages to irc and nostr, turn on lights at your concert venue etc...

