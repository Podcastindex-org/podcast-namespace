# Value

`<podcast:value>`

This element designates the cryptocurrency or payment layer that will be used, the transport method for transacting the payments, and a suggested amount denominated in the given cryptocurrency.

This element can exist at either the `<channel>` or `<item>` level. When it exists at the `<item>` level, it should be treated as an "override" of whatever is defined at the `<channel>` level.

This is a complex tag, so implementors are HIGHLY encouraged to read the companion [document](https://github.com/Podcastindex-org/podcast-namespace/blob/main/value/value.md) for a complete understanding of how this tag works and what it is capable of.

### Parent

`<channel>` or `<item>`

### Count

Multiple

### Node Value

The node value must be one or more [`<podcast:valueRecipient>`](value-recipient.md) elements.

### Attributes

- **type:** (required) This is the service slug of the cryptocurrency or protocol layer.
- **method:** (required) This is the transport mechanism that will be used.
- **suggested:** (optional) This is an optional suggestion on how much cryptocurrency to send with each payment.

### Examples

```xml
<podcast:value
    type="lightning"
    method="keysend"
    suggested="0.00000005000"
></podcast:value>
```
