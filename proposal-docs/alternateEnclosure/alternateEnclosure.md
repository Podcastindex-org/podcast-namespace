# The "podcast:alternateEnclosure" Specification

<small>Version 1.0 by Alecks Gates(@agates) - 2021.04.13</small>

<br>

## Purpose

This tag is a named collection of different transports available to acquire a single type of media.  There can be multiple as needed, to provide media encoded in different ways or provided by different means of transport.

This is designed to provide sufficient information to the podcast player to be able to determine what type of media it supports along with the transports available to acquire it.  At the same time, sufficient information should be provided to be able to allow sane defaults or preferences presented to the user.

For example, one podcast player might only support audio, but can play it in various different codecs such as OPUS, AAC, MP3, etc.  Another may support video, but only video in h264 format.

If desired, media related to the enclosure -- but which does not have the same content -- can be provided.  Perhaps a "bloopers" take, or the same episode in a different language.

Lastly, I propose optional information to verify integrity of downloaded media via hashes or maybe even PGP signatures.  Obviously, PGP verification would require the podcast application to separately import trusted public keys to be useful.  Adhering to the Subresource Integrity standard will help here for hashes.

<br>

## Specification

- **\<podcast:alternateEnclosure**
    type="[mime type]"
    length="[(int)]"
    bitrate="[(float)]"
    height="[(int)]"
    lang="[(string)]"
    title="[(string)]"
    rel="[(string)]"
    codecs="[(string)]"
    default="[(boolean)]">
   [one or more of <podcst:source> and <podcast:integrity>]
  **\</podcast:alternateEnclosure>**
   Item (optional | multiple)

   This element defines a media file. One or more `<podcast:source>` tags must be contained within this element to list available methods to obtain the file.  This is meant to provide different versions of a media file -- such as low or high bitrate, alternate formats (different codecs or video), alternate URI schemes (IPFS or live streaming), or alternate download types not indicated by the URI and type (like torrents).

   The "rel" attribute is meant to allow presentation of media different from the main content given in the enclosure.  For example, "director's cut" or "behind the scenes". _If you are only offering one media content (say, a podcast in MP3, AAC, Opus, over HTTPS and IPFS), you can likely ignore the "rel" attribute._

   The media element always refers to an available media version.  The standard RSS enclosure element is always the default media to be played.

   An `<enclosure>` tag must be present along with this tag within the item.

   - `type` (required) Mime type of the media asset.
   - `length` (required) Length of the file in bytes.
   - `bitrate` (optional) Encoding bitrate of media asset.
   - `height` (optional) Height of the media asset for video formats
   - `lang` (optional) An [IETF language tag (BCP 47)](https://en.wikipedia.org/wiki/BCP_47) code identifying the language of this media.
   - `title` (optional) A human-readable string identifying the name of the media asset.  Should be limited to 32 characters for UX.
   - `rel` (optional) Provides a method of offering and/or grouping together different media elements.  If not set, or set to "default", the media will be grouped with the enclosure and assumed to be an alternative to the enclosure's encoding/transport.  This attribute can and should be the same for items with the same content encoded by different means.  Should be limited to 32 characters for UX.
   - `codecs` (optional) A [RFC 6381](https://tools.ietf.org/html/rfc6381) string specifying the codecs available in this media.
   - `default` (optional) Boolean specifying whether or not the given media is the same as the file from the _enclosure_ element and should be the preferred media element.  The primary reason to set this is to offer alternative transports for the enclosure.  If not set, this should be assumed to be _false_.

<br><br>

- **\<podcast:source uri="[uri of media asset]" contentType="[mime type]" />**

   podcast:alternateEnclosure (required | multiple)

   This element defines available transport methods to obtain media, such as HTTP, HTTPS, IPFS, Tor, magnet, etc, via alternate URI schemes.  If the mime type of the transport medium, such as a torrent file, is different than the media file
   that will ultimately be delivered (the one specified in the `type` attribute of `<podcast:alternateEnclosure>`) then the `contentType` attribute should be used to specify it here.

   There should be one or more source elements in a media tag.

   - `uri` (required) This a an [IANA-registered](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml) URI for the media asset.
   - `contentType` (optional) Mime type of the data retrieved from the URI, if different from the media's "type"

<br><br>

- **\<podcast:integrity type="[integrity type]" value="[integrity value]" />**

   podcast:alternateEnclosure (optional | multiple)

   This element defines a method of verifying integrity the media given either an [SRI-compliant integrity string](https://www.w3.org/TR/SRI/) or a base64 encoded PGP signature.

   There may be one or more integrity elements in a media tag.

   - `type` (required) Type of integrity, either "sri" or "pgp-signature".
   - `value` (required) Value of the sri string or base64 encoded pgp signature.


Example of content served via audio in mp3, [high-bitrate Opus](https://wiki.xiph.org/Opus_Recommended_Settings), "hi-fi" AAC, low-bitrate Opus -- over both HTTPS and IPFS:
```xml
<enclosure url="https://best-podcast.com/file-0.mp3" length="43200000" type="audio/mpeg" />

<podcast:alternateEnclosure type="audio/mpeg" length="43200000" bitrate="128000" default="true" title="Standard">
    <podcast:source uri="https://best-podcast.com/file-0.mp3" />
    <podcast:source uri="ipfs://someRandomMpegFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="32400000" bitrate="96000" title="High quality">
    <podcast:source uri="https://best-podcast.com/file-high.opus" />
    <podcast:source uri="ipfs://someRandomHighBitrateOpusFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/aac" length="54000000" bitrate="160000" title="High quality AAC">
    <podcast:source uri="https://best-podcast.com/file-proprietary.aac" />
    <podcast:source uri="ipfs://someRandomProprietaryAACFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="5400000" bitrate="16000" title="Low bandwidth">
    <podcast:source uri="https://best-podcast.com/file-low.opus" />
    <podcast:source uri="ipfs://someRandomLowBitrateOpusFile" />
</podcast:alternateEnclosure>
```

Example of content served via audio (mp3) and video in different resolutions (mp4) with dynamic streaming options:
```xml
<podcast:alternateEnclosure type="audio/mpeg" length="2490970" bitrate="160707.74">
    <podcast:source uri="https://best-podcast.com/file-0.mp3" />
    <podcast:source uri="ipfs://QmdwGqd3d2gFPGeJNLLCshdiPert45fMu84552Y4XHTy4y" />
    <podcast:source uri="https://best-podcast.com/file-0.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somerandom.onion/file-0.mp3" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="10562995" bitrate="681483.55" height="1080">
    <podcast:source uri="https://best-podcast.com/file-1080.mp4" />
    <podcast:source uri="ipfs://QmfQKJcp2xdByEt8mzWr1AJUhwvb9rdWPoacvdq2roDhgh" />
    <podcast:source uri="https://best-podcast.com/file-1080.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-1080.mp4" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="7924786" bitrate="511276.52" height="720">
    <podcast:source uri="https://best-podcast.com/file-720.mp4" />
    <podcast:source uri="ipfs://QmX33FYehk6ckGQ6g1D9D3FqZPix5JpKstKQKbaS8quUFb" />
    <podcast:source uri="https://best-podcast.com/file-720.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-720.mp4" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="6081197" bitrate="392335.29" height="480">
    <podcast:source uri="https://best-podcast.com/file-480.mp4" />
    <podcast:source uri="ipfs://QmQHNcr88kHp2ieNQYcBRczM7XpMtjRSQcLek6CaJwd81m" />
    <podcast:source uri="https://best-podcast.com/file-480.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-480.mp4" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="4086007" bitrate="327833.03" height="360">
    <podcast:source uri="https://best-podcast.com/file-360.mp4" />
    <podcast:source uri="ipfs://QmeK3EQMuV6cR766kuyG2QUUEJqUVfkJKGPNRceXzXC3ED" />
    <podcast:source uri="https://best-podcast.com/file-360.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-360.mp4" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="2490970" bitrate="263613.35" height="240">
    <podcast:source uri="https://best-podcast.com/file-240.mp4" />
    <podcast:source uri="ipfs://QmdjB94TUMSQu1P8QvPnGnPjNLiWycjtraSaCsiVi4xUNi" />
    <podcast:source uri="https://best-podcast.com/file-240.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-240.mp4" />
</podcast:alternateEnclosure>
<podcast:alternateEnclosure type="application/x-mpegURL" length="10562995">
    <podcast:source uri="https://best-podcast.com/master.m3u8" />
    <podcast:source uri="ipfs://exampleLinkThatDoesntWorkHLS" />
    <podcast:source uri="http://somerandom.onion/master.m3u8" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="application/dash+xml" length="10562995">
    <podcast:source uri="https://example.com/master.mpd" />
    <podcast:source uri="ipfs://exampleLinkThatDoesntWorkDASH" />
    <podcast:source uri="http://somerandom.onion/master.mpd" />
</podcast:alternateEnclosure>
```

Example use of the "rel" attribute offering "Behind the Scenes" content:
```xml
<podcast:alternateEnclosure type="audio/mp4" length="2490970" bitrate="160707.74" rel="Behind the Scenes">
    <podcast:source uri="https://example.com/file.mp4" />
    <podcast:source uri="ipfs://exampleLinkThatDoesntWork" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="10562995" bitrate="681483.55" height="1080" rel="Behind the Scenes">
    <podcast:source uri="https://example.com/video.mp4" />
    <podcast:source uri="ipfs://exampleLinkThatDoesntWorkVideo" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="application/x-mpegURL" length="10562995" rel="Behind the Scenes">
    <podcast:source uri="https://example.com/master.m3u8" />
    <podcast:source uri="ipfs://exampleLinkThatDoesntWorkHLS" />
</podcast:alternateEnclosure>
```