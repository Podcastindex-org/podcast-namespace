# Alternate Enclosure

`<podcast:alternateEnclosure>`

This element is meant to provide different versions of, or companion media to the main [`<enclosure>`](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt) file. This could be an audio only version of a video podcast to allow apps to switch back and forth between audio/video, lower (or higher) bitrate versions for bandwidth constrained areas, alternative codecs for different device platforms, alternate URI schemes and download types such as IPFS or WebTorrent, commentary tracks or supporting source clips, etc.

This is a complex tag, so implementors are highly encouraged to read the companion [document](https://github.com/Podcastindex-org/podcast-namespace/blob/main/proposal-docs/alternateEnclosure/alternateEnclosure.md) for a fuller understanding of how this tag works and what it is capable of.

### Parent

`<item>`

### Count

Multiple

### Node Value

The node value must be one or more [`<podcast:source>`](source.md) elements that each define a uri where the media file can be downloaded or streamed. A single, optional [`<podcast:integrity>`](integrity.md) element may also be included to allow for file integrity checking.

### Attributes

- **type:** (required) Mime type of the media asset.
- **length:** (recommended) Length of the file in bytes.
- **bitrate:** (optional) Average encoding bitrate of the media asset, expressed in bits per second.
- **height:** (optional) Height of the media asset for video formats.
- **lang:** (optional) An [IETF language tag (BCP 47)](https://en.wikipedia.org/wiki/BCP_47) code identifying the language of this media.
- **title:** (optional) A human-readable string identifying the name of the media asset. Should be limited to 32 characters for UX.
- **rel:** (optional) Provides a method of offering and/or grouping together different media elements. If not set, or set to "default", the media will be grouped with the enclosure and assumed to be an alternative to the enclosure's encoding/transport. This attribute can and should be the same for items with the same content encoded by different means. Should be limited to 32 characters for UX.
- **codecs:** (optional) An [RFC 6381](https://datatracker.ietf.org/doc/html/rfc6381) string specifying the codecs available in this media.
- **default:** (optional) Boolean specifying whether or not the given media is the same as the file from the enclosure element and should be the preferred media element. The primary reason to set this is to offer alternative transports for the enclosure. If not set, this should be assumed to be false.

### Examples

```xml
<enclosure url="https://example.com/file-0.mp3" length="43200000" type="audio/mpeg" />

<podcast:alternateEnclosure type="audio/mpeg" length="43200000" bitrate="128000" default="true" title="Standard">
    <podcast:source uri="https://example.com/file-0.mp3" />
    <podcast:source uri="ipfs://someRandomMpegFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="32400000" bitrate="96000" title="High quality">
    <podcast:source uri="https://example.com/file-high.opus" />
    <podcast:source uri="ipfs://someRandomHighBitrateOpusFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/aac" length="54000000" bitrate="160000" title="High quality AAC">
    <podcast:source uri="https://example.com/file-proprietary.aac" />
    <podcast:source uri="ipfs://someRandomProprietaryAACFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="5400000" bitrate="16000" title="Low bandwidth">
    <podcast:source uri="https://example.com/file-low.opus" />
    <podcast:source uri="ipfs://someRandomLowBitrateOpusFile" />
</podcast:alternateEnclosure>
```

```xml
<podcast:alternateEnclosure type="audio/mpeg" length="2490970" bitrate="160707.74">
    <podcast:source uri="https://example.com/file-0.mp3" />
    <podcast:source uri="ipfs://QmdwGqd3d2gFPGeJNLLCshdiPert45fMu84552Y4XHTy4y" />
    <podcast:source uri="https://example.com/file-0.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="https://example.onion/file-0.mp3" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="10562995" bitrate="681483.55" height="1080">
    <podcast:source uri="https://example.com/file-1080.mp4" />
    <podcast:source uri="ipfs://QmfQKJcp2xdByEt8mzWr1AJUhwvb9rdWPoacvdq2roDhgh" />
    <podcast:source uri="https://example.com/file-1080.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="https://example.onion/file-1080.mp4" />
</podcast:alternateEnclosure>
```
