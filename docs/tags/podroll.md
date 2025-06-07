# Podroll

`<podcast:podroll>`

This element allows for a podcaster to include references to one or more podcasts in it's `<channel>` as a way of "recommending" other podcasts to their listener.

### Parent

`<channel>`

### Count

Single

### Node value

The node value must be one or more [`<podcast:remoteItem>`](remote-item.md) elements.

### Example

```xml
<podcast:podroll>
    <podcast:remoteItem feedGuid="29cdca4a-32d8-56ba-b48b-09a011c5daa9" />
    <podcast:remoteItem feedGuid="396d9ae0-da7e-5557-b894-b606231fa3ea" />
    <podcast:remoteItem feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810" />
</podcast:podroll>
```
