# Podroll

`<podcast:podroll>`

This element allows for a podcaster to include references to one or more podcasts in it's `<channel>` as a way of "recommending" other podcasts to their listener.

### Parent

`<channel>`

### Count

Single

### Node value

The node value must be one or more [`<podcast:remoteItem>`](remoteItem.md) elements.

### Example
- Recomendations with just the required `feedGuid`:
```xml
<podcast:podroll>
    <podcast:remoteItem feedGuid="29cdca4a-32d8-56ba-b48b-09a011c5daa9" />
    <podcast:remoteItem feedGuid="396d9ae0-da7e-5557-b894-b606231fa3ea" />
    <podcast:remoteItem feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810" />
</podcast:podroll>
```

- Recomendations with the required `feedGuid` and the optional `feedUrl`:
```xml
<podcast:podroll>
    <podcast:remoteItem feedGuid="610e9ea8-edf0-407f-9e6c-72375a0e17db" feedUrl="https://feeds.redcircle.com/610e9ea8-edf0-407f-9e6c-72375a0e17db" />
    <podcast:remoteItem feedGuid="68b52970-05d8-5cb1-8bd3-d15fa3961e09" feedUrl="https://accesibilidadtl.gitlab.io/feed" />
    <podcast:remoteItem feedGuid="a9a56b87-575a-5f6f-9636-cdf7b73e6230" feedUrl="https://kdeexpress.gitlab.io/feed" />
</podcast:podroll>
```
