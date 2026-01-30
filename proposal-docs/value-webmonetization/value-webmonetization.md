# Web Monetization using `<podcast:value/>`

To enable and promote Web Monetization (WM) in podcasting, necessary information for WM could be provided for a podcast by adding a `<podcast:value/>` tag to the RSS feed.
  https://github.com/Podcastindex-org/podcast-namespace/blob/main/value/value.md

This `value` tag would have the following values for use with Web Monetization:
```xml
<podcast:value 
    type="webmonetization" 
    method="ILP"
></podcast:value>
```

The `podcast:value.type` defines the cryptocurrency or protocol layer, which in this case is used to define Web Monetization payment information, using the value of `webmonetization`.

The `method` of `ILP` specifies the use of [Interledger  Protocol (ILP)](https://interledger.org/rfcs/0027-interledger-protocol-4/) which is the protocol for payment providers in WM.

Within the `podcast:value` tag we also need to designate recipients. In Web Monetization, payment info is discovered using the [Simple Payment Setup Protocol](https://interledger.org/rfcs/0009-simple-payment-setup-protocol) with which the [Open Payments](https://openpayments.dev/overview/getting-started/#web-monetization) protocol is designed to be [backwards compatible](https://openpayments.dev/resources/glossary/#payment-pointer).

Both SPSP and Open Payments use a recipient address in the form of a [Payment Pointer](https://github.com/interledger/rfcs/blob/master/0026-payment-pointers/0026-payment-pointers.md) which will be provided in the `address` attribute, with the `type` of `paymentpointer`.

```xml
<podcast:value 
    type="webmonetization" 
    method="ILP"
>
    <podcast:valueRecipient
        name="Alice"
        type="paymentpointer"
        address="$example.now/~alice"
        split="100"
    >
</podcast:value>
```

While payments in WM are streamed to a single payment pointer at a time, the WM authors suggest splits can be implemented using [Probabilistic Revenue Sharing](https://webmonetization.org/docs/probabilistic-rev-sharing) or perhaps it should be [implemented at the payment pointer](https://github.com/Podcastindex-org/podcast-namespace/issues/132#issuecomment-780145285) rather than expecting the client to be responsible. In either case, the `podcast:value` and `podcast:valueRecipient` tags can provide sufficient information for a web player or page to setup Web Monetization for the podcast.

------

### Current <podcast:value\>
https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/tags/value.md
https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/examples/value/valueslugs.txt

### Web Monetization and Podcasting Discussion
https://github.com/Podcastindex-org/podcast-namespace/issues/132
https://github.com/WICG/webmonetization/issues/70

### Background WM (i.e. for podcast audio playing on a background tab)
https://github.com/coilhq/web-monetization-projects/issues/387
https://github.com/WICG/webmonetization/issues/17

### PRX Player Implementation (in production)
Player repository: https://github.com/PRX/Play-Next.js
Component to implement monetization: https://github.com/PRX/Play-Next.js/tree/main/components/Player/WebMonetized
Parsing `webmonetization` from the RSS feed: https://github.com/PRX/Play-Next.js/blob/main/lib/parse/data/parseEmbedData.ts#L33-L40

### Castopod Implementation
https://podlibre.social/@Castopod/105278541687633547
https://blog.castopod.org/castopod-supports-web-monetization/
https://github.com/ad-aures/castopod/blob/v1.0.0-beta.14/app/Helpers/rss_helper.php#L85-L95

### PodStation Planning by [Guilherme Dellagustin](https://github.com/dellagustin)
https://github.com/podStation/podStation/issues/185
