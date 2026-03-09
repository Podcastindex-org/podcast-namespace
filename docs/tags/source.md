# Source

`<podcast:source>`

This element defines a uri location for a [`<podcast:alternateEnclosure>`](alternate-enclosure.md) media file. It is meant to be used as a child of the [`<podcast:alternateEnclosure>`](alternate-enclosure.md) element. At least one `<podcast:source>` element must be present within every [`<podcast:alternateEnclosure>`](alternate-enclosure.md) element.

### Parent

[`<podcast:alternateEnclosure>`](alternate-enclosure.md)

### Count

Multiple

### Attributes

- **uri:** (required) This is the uri where the media file resides.
- **contentType:** (optional) This is a string that declares the mime-type of the file. It is useful if the transport mechanism is different than the file being delivered, as is the case with a torrents.

### Examples

```xml
<podcast:alternateEnclosure type="video/mp4" length="7924786" bitrate="511276.52" height="720">
    <podcast:source uri="https://example.com/file-720.mp4" />
    <podcast:source uri="ipfs://QmX33FYehk6ckGQ6g1D9D3FqZPix5JpKstKQKbaS8quUFb" />
    <podcast:source uri="https://example.com/file-720.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="https://example.onion/file-720.mp4" />
</podcast:alternateEnclosure>
```
