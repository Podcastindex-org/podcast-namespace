# Live Item

`<podcast:liveItem>`

The `liveItem` tag is used for a feed to deliver a live audio or video stream to podcast apps. It takes the same format as a standard `<item>` episode tag, and all tags that are allowed as children of a normal `<item>` are also allowed as children of `<podcast:liveItem>`. Note that "allowed" is not the same as "supported". So, just like a normal `<item>`, you cannot depend on all apps to support all tags within `<podcast:liveItem>`, especially when the function of the tag is not obvious. For instance, including an `<itunes:duration>` tag in a live item is probably a waste of time since apps will not know what to do with that value in the context of live media.

This tag will also make use of the [podping](https://podping.cloud) notification network. A podping notification SHOULD be sent out by the host when the live stream starts, to let apps know.

### Parent

`<channel>`

### Count

Multiple

### Node Value

All tags that are valid as children of a standard `<item>` tag are also valid as children here.

When specifying the audio/video source, the [`<podcast:alternateEnclosure>`](./alternate-enclosure.md) tag is highly encouraged since it gives the broadest coverage of possible stream types and is explicit in its communication of what transport protocol and media codecs are being used. In addition to [`<podcast:alternateEnclosure>`](./alternate-enclosure.md), a standard [`<enclosure>`](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt) should also be given as a fallback to support podcast apps that don't yet implement [`<podcast:alternateEnclosure>`](./alternate-enclosure.md).

Regardless of which enclosure tag is used, feed owners must be conscious of the fact that choosing a non-mainstream streaming protocol/codec will limit the number of apps that can play the content. For that reason, it's highly recommended to use only the two most widely supported protocols (mp3 and mp4/h.264) to ensure compatibility with the broadest number of apps on various platforms. Choosing a streaming format that is outside of this narrow list might exclude many apps from playing your content. As broader adoption of HLS, Opus, etc. becomes apparent, this recommendation will change to include newer formats.

The [`<podcast:contentLink>`](./content-link.md) tag is also required to be present, to ensure that listeners have a fallback option in case their chosen app cannot play the given content stream directly. In most instances this will just be a link to an HTML page that can play the live stream. Such a page can reside on the podcaster's own website, a page provided by their hosting company or a third party platform they have chosen to use. Podcasters who live stream to multiple platforms at once can also use the [`<podcast:contentLink>`](./content-link.md) tag to provide links to those other platforms.

A robust, well-written `<podcast:liveItem>` tag will include all three of: [`<podcast:alternateEnclosure>`](./alternate-enclosure.md), [`<enclosure>`](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt) and [`<podcast:contentLink>`](./content-link.md) to ensure the broadest interopability with podcast apps.

The function of `<guid>` within a live item tag is the same as it is within a regular item. If the `<guid>` of a `<podcast:liveItem>` changes, it MUST be considered a new stream by podcast apps.

### Attributes

- **status** (required) A string that must be one of `pending`, `live` or `ended`.
- **start** (required) A string representing an ISO8601 timestamp that denotes the time when the stream is intended to start.
- **end** (recommended) A string representing an ISO8601 timestamp that denotes the time when the stream is intended to end.

The `start` and `end` attributes denote when the live stream "should" start and end. But, real life dictates that those times might not be adhered to. Apps are therefore encouraged not to rely solely on those times as anything more than an approximation. The canonical way to know if a stream has started is with the `status` attribute. If `status` is "live" then the stream has started.

### Examples

A complete example:

```xml
<podcast:liveItem status="live" start="2021-09-26T07:30:00.000-0600" end="2021-09-26T09:30:00.000-0600">
    <title>Podcasting 2.0 Live Show</title>
    <description>A look into the future of podcasting and how we get to Podcasting 2.0!</description>
    <link>https://example.com/podcast/live</link>
    <guid isPermaLink="true">https://example.com/live</guid>
    <author>John Doe (john@example.com)</author>
    <podcast:images srcset="https://example.com/images/live/pci_avatar-massive.jpg 1500w,
        https://example.com/images/live/pci_avatar-middle.jpg 600w,
        https://example.com/images/live/pci_avatar-small.jpg 300w,
        https://example.com/images/live/pci_avatar-tiny.jpg 150w"
    />
    <podcast:person href="https://www.podchaser.com/creators/adam-curry-107ZzmWE5f"
                    img="https://example.com/images/adamcurry.jpg">Adam Curry</podcast:person>
    <podcast:person role="guest" href="https://github.com/daveajones/"
                    img="https://example.com/images/davejones.jpg">Dave Jones</podcast:person>
    <podcast:person group="visuals" role="cover art designer"
                    href="https://example.com/artist/beckysmith">Becky Smith</podcast:person>
    <podcast:alternateEnclosure type="audio/mpeg" length="312" default="true">
        <podcast:source uri="https://example.com/pc20/livestream" />
    </podcast:alternateEnclosure>
    <enclosure url="https://example.com/pc20/livestream?format=.mp3" type="audio/mpeg" length="312" />
    <podcast:contentLink href="https://youtube.com/pc20/livestream">YouTube!</podcast:contentLink>
    <podcast:contentLink href="https://twitch.com/pc20/livestream">Twitch!</podcast:contentLink>
    <podcast:contentLink href="https://example.com/html/livestream">Listen Live!</podcast:contentLink>
</podcast:liveItem>
```

A bare bones example:

```xml
<podcast:liveItem status="live" start="2021-09-26T07:30:00.000-0600" end="2021-09-26T09:30:00.000-0600">
    <title>Podcasting 2.0 Live Stream</title>
    <guid>e32b4890-983b-4ce5-8b46-f2d6bc1d8819</guid>
    <enclosure url="https://example.com/pc20/livestream?format=.mp3" type="audio/mpeg" length="312" />
    <podcast:contentLink href="https://example.com/html/livestream">Listen Live!</podcast:contentLink>
</podcast:liveItem>
```
