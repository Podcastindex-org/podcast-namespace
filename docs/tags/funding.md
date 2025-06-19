# Funding

`<podcast:funding>`

This tag lists possible donation/funding links for the podcast. The content of the tag is the recommended string to be used with the link.

### Parent

`<channel>`, `<item>` or [`<podcast:liveItem>`](live-item.md)

### Count

Multiple

### Node value

This is a free form string supplied by the creator which they expect to be displayed in the app next to the link. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

### Attributes

- **url (required):** The URL to be followed to fund the podcast.

### Examples

```xml
<podcast:funding url="https://www.example.com/donations">Support the show!</podcast:funding>
```

```xml
<podcast:funding url="https://www.example.com/members">Become a member!</podcast:funding>
```
