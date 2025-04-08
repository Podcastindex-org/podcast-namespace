# Value Recipient

`<podcast:valueRecipient>`

The `valueRecipient` tag designates various destinations for payments to be sent to during consumption of the enclosed media. Each recipient is considered to receive a "split" of the total payment according to the number of shares given in the `split` attribute.

This element may only exist within a parent `<podcast:value>` element.

There is no limit on how many `valueRecipient` elements can be present in a given `<podcast:value>` element.

This is a complex tag, so implementors are HIGHLY encouraged to read the companion [document](https://github.com/Podcastindex-org/podcast-namespace/blob/main/value/value.md) for a complete understanding of how this tag works and what it is capable of.

### Parent

`<podcast:value>`

### Count

Multiple

### Attributes

- **name** (recommended) A free-form string that designates who or what this recipient is.
- **customKey** (optional) The name of a custom record key to send along with the payment.
- **customValue** (optional) A custom value to pass along with the payment. This is considered the value that belongs to the `customKey`.
- **type** (required) A slug that represents the type of receiving address that will receive the payment.
- **address** (required) This denotes the receiving address of the payee.
- **split** (required) The number of shares of the payment this recipient will receive.
- **fee** (optional) If this attribute is not specified, it is assumed to be false.

### Examples

```xml
<podcast:value type="lightning" method="keysend" suggested="0.00000015000">
  <podcast:valueRecipient
      name="Alice (Podcaster)"
      type="node"
      address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
      split="40"
  />
  <podcast:valueRecipient
      name="Bob (Podcaster)"
      type="node"
      address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
      split="40"
  />
  <podcast:valueRecipient
      name="Carol (Producer)"
      type="node"
      address="02dd306e68c46681aa21d88a436fb35355a8579dd30201581cefa17cb179fc4c15"
      split="15"
  />
  <podcast:valueRecipient
      name="Hosting Provider"
      type="node"
      address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
      split="5"
      fee="true"
  />
</podcast:value>
```
