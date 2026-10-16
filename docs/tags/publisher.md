# Publisher

`<podcast:publisher>`

This element allows a podcast feed to link to the publisher feeds of the entities that have a relationship to it. An artist and a record label, or a show and its network, can each attest to the same feed. Each link says what that relationship is.

The element contains one or more [`<podcast:remoteItem medium="publisher">`](remote-item.md) elements. Each one points at a different publisher feed. For widest compatibility, it is highly recommended to use the `feedUrl` attribute of the [`<podcast:remoteItem>`](remote-item.md) element.

For complete implementation details regarding publisher feeds and how to create them, please see the full publisher feed [documentation](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/examples/publishers/publishers.md) and the `publisher` medium [here](./medium.md).

### Parent

`<channel>`

### Count

Single

### Node value

One or more [`<podcast:remoteItem>`](remote-item.md) elements. Each must use `medium="publisher"` and point at a feed whose [`<podcast:medium>`](medium.md) is `publisher`.

### The `rel` attribute

Each remote item may include a `rel` attribute. The value is a space-separated list of tokens naming how that publisher feed's entity relates to this feed. Use one remote item per party. When that party has more than one role on this feed, put every role on that one remote item.

One publisher feed can hold several roles for the same work. Jimmy V is the artist and the producer of The Billy Big Rig Album. A second remote item with the same `feedGuid` would look like a second party. `rel="artist producer"` keeps both roles on Jimmy V's single publisher feed.

If `rel` is omitted, the link has its original meaning: that entity publishes this feed.

These tokens are a starting list. Others may be added as new relationships show up in real feeds.

- `artist` — the musical act or band. For example, FM Rodeo, Joe Martin.
- `host` — the podcast host or personality portfolio. For example, Joe Rogan, Adam Curry.
- `author` — the writer or audiobook bibliography. For example, Stephen King, Ayn Rand.
- `label` — the record label catalog. For example, Rubber Factory Records, Canabis Records.
- `producer` — the producer of a work. For example, Jimmy V.
- `network` — the podcast network. For example, TWiT, NPR.
- `hosting` — the infrastructure provider or server platform. For example, Bluebrry, Buzzsprout.
- `sponsor` — the commercial patron or brand portfolio. For example, Black Rifle Coffee, Anton's Land of Biltong.

The publisher feed repeats the same `rel` on its channel-level [`<podcast:remoteItem>`](remote-item.md) for this feed. That is the other half of the link, described below.

### Confirmation

A link counts only when both feeds point at each other.

- This feed names the publisher feed's `feedGuid` on a `<podcast:remoteItem>` inside `<podcast:publisher>`.
- That publisher feed names this feed's `feedGuid` on a channel-level `<podcast:remoteItem>`.

If either side omits `rel`, treat the link as the original publisher relationship, so existing feeds keep validating. If both sides include `rel`, they must name the same set of tokens. Order does not matter, so `artist producer` agrees with `producer artist`. When the sets differ, discard that association.

An app that only reads the first remote item still sees one publisher. Put the primary relationship first.

On the publisher feed, the matching child link looks like this:

```xml
<podcast:medium>publisher</podcast:medium>
<podcast:remoteItem medium="music" rel="label" feedGuid="469b403f-db2d-574c-9db9-96dbb4f6561c" feedUrl="https://example.org/itsamood/rss.xml"/>
```

### Examples

A feed with one publisher. `rel` is omitted, so the link keeps its original meaning: that entity publishes this feed.

```xml
<channel>
    <title><![CDATA[It's A Mood]]></title>
    <description>A value4value happenstance music show.</description>
    <link>https://example.org/itsamood</link>
    <generator>Sovereign Feeds</generator>
    <podcast:person href="https://example.org/mikeneumann" group="cast" role="host" img="https://example.org/mikeneumann/image.jpg">Mike Neumann</podcast:person>
    <podcast:guid>469b403f-db2d-574c-9db9-96dbb4f6561c</podcast:guid>
    <podcast:medium>podcast</podcast:medium>
    <podcast:publisher>
        <podcast:remoteItem medium="publisher" feedGuid="003af0a0-6a45-55cf-b765-68e3d349551a" feedUrl="https://agilesetmedia.com/assets/static/feeds/publisher.xml"/>
    </podcast:publisher>
    <item>
        <title><![CDATA[Runnin']]></title>
        <pubDate>Wed, 03 Apr 2024 02:06:28 +0000</pubDate>
        ...
    </item>
</channel>
```

An album claimed by both its artist and its label. Each party has its own publisher feed, and each feed lists this album back with the same `rel`:

```xml
<channel>
    <title><![CDATA[Drift]]></title>
    <podcast:guid>a1b2c3d4-e5f6-5718-9abc-def012345678</podcast:guid>
    <podcast:medium>music</podcast:medium>
    <podcast:publisher>
        <podcast:remoteItem medium="publisher" rel="artist" feedGuid="6f9619ff-8b86-5653-93a9-4e1d0a801b97" feedUrl="https://example.org/drift-current/publisher.xml"/>
        <podcast:remoteItem medium="publisher" rel="label" feedGuid="1b9d6bcd-bbfd-5642-92fe-9089d1c8d331" feedUrl="https://example.org/rubber-factory/publisher.xml"/>
    </podcast:publisher>
</channel>
```

One party with two roles. Jimmy V is the artist and the producer of The Billy Big Rig Album, so his publisher feed is named once. The album repeats `rel="artist producer"` to agree. `rel="producer artist"` would agree too.

In the album feed:

```xml
<channel>
    <title><![CDATA[The Billy Big Rig Album]]></title>
    <podcast:guid>c3d4e5f6-a7b8-5901-2345-6789abcdef01</podcast:guid>
    <podcast:medium>music</podcast:medium>
    <podcast:publisher>
        <podcast:remoteItem medium="publisher" rel="artist producer" feedGuid="d4e5f6a7-b8c9-5012-3456-789abcdef012" feedUrl="https://example.org/jimmy-v/publisher.xml"/>
    </podcast:publisher>
</channel>
```

In Jimmy V's publisher feed:

```xml
<podcast:medium>publisher</podcast:medium>
<podcast:remoteItem medium="music" rel="artist producer" feedGuid="c3d4e5f6-a7b8-5901-2345-6789abcdef01" feedUrl="https://example.org/billy-big-rig/rss.xml"/>
```
