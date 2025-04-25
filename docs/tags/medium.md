# Medium

`<podcast:medium>`

The `medium` tag tells an application what the content contained within the feed IS, as opposed to what the content is ABOUT in the case of a category. This allows a podcast app to modify it's behavior or UI to give a better experience to the user for this content. For example, if a podcast has `<podcast:medium>music</podcast:medium>` an app may choose to reset playback speed to 1x and adjust it's EQ settings to be better for music vs. spoken word.

Accepted medium names are curated within a list maintained by the community as new mediums are discovered over time. Newly proposed mediums should require some level of justification to be added to this list. One may argue and/or prove use of a new medium even for only one application, should it prove different enough from existing mediums to have meaning.

### Parent

`<channel>`

### Count

Single

### Node Value

The node value is a string denoting one of the following possible values:

- `podcast` (default) - Describes a feed for a podcast show. If no `medium` tag is present in the channel, this medium is assumed.
- `music` - A feed of music organized into an "album" with each item a song within the album.
- `video` - Like a "podcast" but used in a more visual experience. Something akin to a dedicated video channel like would be found on YouTube.
- `film` - Specific types of videos with one item per feed. This is different than a `video` medium because the content is considered to be cinematic; like a movie or documentary.
- `audiobook` - Specific types of audio with one item per feed, or where items represent chapters within the book.
- `newsletter` - Describes a feed of curated written articles. Newsletter articles now sometimes have an spoken version audio enclosure attached.
- `blog` - Describes a feed of informally written articles. Similar to `newsletter` but more informal as in a traditional blog platform style.
- `publisher` - Describes a feed that links to other feeds a publisher owns using the [`<podcast:remoteItem>`](remoteItem.md) element. To understand the structure of how "publisher" feeds work, please see the dedicated document [here](../../publishers/publishers.md) and the [`<podcast:publisher>`](publisher.md) tag [here](./publisher.md).
- `course` - A feed of training material (audio or video courses) with each item being a session or chapter of the course or conference track.

### List Mediums

In addition to the above mediums, each medium also has a counterpart "list" variant, where the original medium name is suffixed by the letter "L" to indicate that it is a "List" of that type of content. For example, `podcast` would become `podcastL`, `music` would become `musicL`, `audiobook` would become `audiobookL`, etc.

There is also a dedicated list medium for mixed content:

- `mixed` - This list medium type describes a feed of [`<podcast:remoteItem>`](remoteItem.md)'s that point to different remote medium types. For instance, a single list feed might point to music, podcast and audiobook items in other feeds. An example would be a personal consumption history feed.

A "list" medium feed should not be expected to have regular `<item>`'s,[`<podcast:liveItem>`](liveItem.md)'s, or any future similar new item type. Rather, a "List" feed is intended to exclusively contain one or more [`<podcast:remoteItem>`](remoteItem.md)'s, allowing one to reference a feed by its [`<podcast:guid>`](guid.md) and the guid of an item.

### Examples

Example use for a standard "podcast" feed:

```xml
<podcast:medium>podcast</podcast:medium>
```

Example use for a "music" feed:

```xml
<podcast:medium>music</podcast:medium>
```

Example use for a "music" playlist feed:

```xml
<rss xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" version="2.0">
    <channel>
        <title>Picking the Hits 2.0!</title>
        <description>All the hits played on the Podcasting 2.0 show.</description>
        <link>https://podcastindex.org</link>
        <language>en-US</language>
        <pubDate>Wed, 07 Jun 2023 04:30:38 GMT</pubDate>
        <image>
            <url>https://example.com/images/pci_avatar-massive.jpg</url>
        </image>

        <podcast:guid>3f2a8e4e-263a-51aa-9d3d-0d71f82a1564</podcast:guid>
        <podcast:medium>musicL</podcast:medium>
        <itunes:image>https://example.com/images/pci_avatar-massive.jpg</itunes:image>

        <podcast:value type="lightning" method="keysend" suggested="0.00000005000">
            <podcast:valueRecipient name="podcaster" type="node" address="036557ea56b3b86f08be31bcd2557cae8021b0e3a9413f0c0e52625c6696972e57" split="99" />
            <podcast:valueRecipient name="hosting company" type="node" address="036557ea56b3b86f08be31bcd2557cae8021b0e3a9413f0c0e52625c6696972e57" split="1" />
        </podcast:value>

        <podcast:remoteItem
                feedGuid="ff519475-6e90-5231-91a0-37d092088d88"
                feedUrl="https://media.rss.com/joemartinmusic/feed.xml"
                itemGuid="e75771b1-e8d4-4133-9392-c579822247d9"
                medium="music"
        />

        <podcast:remoteItem
                feedGuid="47081700-bd65-511f-b535-f545f3cd660c"
                feedUrl="https://player.wavlake.com/feed/d1ed0ec9-21a8-4eda-b2c9-b17c8019a7e8"
                itemGuid="7b03666e-b323-499d-93a7-ca51ce627ffd"
                medium="music"
        />

       <podcast:remoteItem
               feedGuid="483dde8e-7e94-59a7-8eb0-2b0dc64a87bd"
               feedUrl="https://player.wavlake.com/feed/1dd1bbd8-1084-4fdc-9788-dddaa62fbc6a"
               itemGuid="8501fb64-a6a3-475a-8b10-9c746f0fe579"
               medium="music"
       />

       <podcast:remoteItem
               feedGuid="b40ffcf7-2c48-5cfe-8daa-b65d766b2c25"
               feedUrl="https://www.wavlake.com/feed/92b04241-97f5-4ff7-be11-cf45f70812e7"
               itemGuid="9a48aab8-6da6-4cc1-9951-5b049c333580"
               medium="music"
       />
    </channel>
</rss>
```
