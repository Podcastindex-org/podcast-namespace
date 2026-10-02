# The Publisher Medium

v1.0 - April 5, 2024

<br>

Below, you will find implementation details about using the `publisher` value in the [`<podcast:medium>`](../../tags/medium.md) tag to
create "publisher feeds".

<br>

## Overview

The idea of a "publisher" is that a single entity (person, organization, record label, etc) might be the responsible
party which produces multiple podcast feeds. In such a case it would be useful to be able to see all of a
publisher's podcasts collected in a single place. For instance, a news organization might produce 12 different
podcast feeds. Or, a music artist might produce 3 albums of music using the `<podcast:medium>` tag of `music`. In
those cases, having a high level feed that references these other feeds would make it easier for podcast apps to
associate those feeds with a particular publishing entity.

Likewise, it is helpful if the produced feeds link back to the "publisher feed" so that podcast apps can walk back
up the chain from a podcast feed to it's publisher in order to find other relevant content from that publishing
entity. For instance, a listener may subscribe to a music album by an artist and want to find their other
albums and singles.

When a publisher feed links to it's "child" feeds, and those "child" feeds link back to their "parent" publisher
feeds, this provides a two-way validation that a feed is indeed a valid part of a publishing entities portfolio of
content. If a feed links to a publisher feed without the publisher feed referencing it, that association should be
discarded.

A child feed may name more than one publisher feed. An artist and a label can both attest to the same album. Each
party is one [`<podcast:remoteItem>`](../../tags/remote-item.md) inside [`<podcast:publisher>`](../../tags/publisher.md).
The optional `rel` attribute on that remote item says how the party relates to the child feed. When one party has
several roles, list every role on that single remote item, for example `rel="artist producer"`. A second remote item
with the same `feedGuid` would look like a second party. The publisher feed repeats the same `rel` on its remote item
for that child. The token list is on the [`<podcast:publisher>`](../../tags/publisher.md) tag.

If either side omits `rel`, the link is the original publisher relationship, and existing feeds keep validating. If
both sides include `rel`, they must name the same set of tokens. Order does not matter, so `artist producer` agrees
with `producer artist`. When the sets differ, discard that association. Apps should keep tokens they do not recognize
and compare the full set.

<br>

## Publisher Feed Requirements

A publisher feed must have the following parts in it's `<channel>`:

1. A [`<podcast:medium>`](../../tags/medium.md) tag with a value of `publisher`.
2. A valid [`<podcast:guid>`](../../tags/guid.md).
3. One or more [`<podcast:remoteItem>`](../../tags/remote-item.md) tags, one per child feed. Use `rel` when the relationship is more specific than a plain publisher link. The child feed must repeat those same tokens.

### Example

The following example shows a publisher feed that links to all of the feeds published by the "AgileSet Media" entity.
This feed also makes use of the [`<podcast:person>`](../../tags/person.md) tag to define a responsible person at the
publishing entity. These remote items omit `rel`, so each link means AgileSet publishes that feed.

```xml
<rss xmlns:podcast="https://podcastindex.org/namespace/1.0" version="2.0">
    <channel>
        <title>AgileSet Media</title>
        <link>https://agilesetmedia.com</link>
        <description>AgileSet Media is an unincorporated, unregistered, and unpapered entity of AgileSet LLC for producing and publishing stuff by Mike Neumann. It is based in Texas, USA.</description>
        <image>
            <url>https://agilesetmedia.com/assets/static/AgileSet-logo-square-sm-144.png</url>
            <title>AgileSet Media</title>
            <link>https://agilesetmedia.com</link>
            <width>144</width>
            <height>144</height>
        </image>
        <podcast:person href="https://mikeneumann.net" group="Creative Direction" role="Director" img="https://itsamood.org/assets/static/MikeNeumann_202310.jpg">Mike Neumann</podcast:person>
        <podcast:guid>003af0a0-6a45-55bf-b765-68e3d349551a</podcast:guid>
        <podcast:medium>publisher</podcast:medium>
        <podcast:remoteItem medium="podcast" feedGuid="469b403f-db2d-574c-9db9-96dbb3f6561c" feedUrl="https://itsamood.org/itsamoodrss.xml"/>
        <podcast:remoteItem medium="podcast" feedGuid="72816866-317e-5e48-8895-8193d58e5b57" feedUrl="https://mikesmixtape.com/mikesmixtaperss.xml"/>
        <podcast:remoteItem medium="podcast" feedGuid="7a2d292c-8656-5fcf-88d2-31b10e54d7c7" feedUrl="https://mikeneumann.show/themnshowrss.xml"/>
    </channel>
</rss>
```

<br>

## Linking to Publisher Feeds

While not strictly required, adding a reference to the publisher feed from the "child" feeds is a good idea, as it
makes discovery of your other content much easier. Podcast apps can see this linkage and "walk back up the chain"
to your publisher feed and then recommend your other podcast content to a listener. To do this, add a
[`<podcast:publisher>`](../../tags/publisher.md) element in the `<channel>` of the child feed. It holds one
[`<podcast:remoteItem medium="publisher">`](../../tags/remote-item.md) per publisher feed. `feedUrl` is recommended.
Put the primary party first. An app that only reads the first remote item still sees one publisher.

### Example

The following example snippet shows a podcast feed produced by "AgileSet Media" that links to the publisher feed
example above. `rel` is omitted, which matches the plain publisher links in that feed.

```xml
<rss xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" xmlns:content="http://purl.org/rss/1.0/modules/content/" xmlns:podcast="https://podcastindex.org/namespace/1.0" version="2.0">
    <channel>
        <title><![CDATA[It's A Mood]]></title>
        <description>A value4value happenstance music show.</description>
        <link>https://itsamood.org</link>
        <generator>Sovereign Feeds</generator>
        <podcast:person href="https://mikeneumann.net" group="cast" role="host" img="https://itsamood.org/assets/static/MikeNeumann_202310.jpg">Mike Neumann</podcast:person>
        <podcast:guid>469b403f-db2d-574c-9db9-96dbb3f6561c</podcast:guid>
        <podcast:medium>podcast</podcast:medium>
        <podcast:publisher>
            <podcast:remoteItem medium="publisher" feedGuid="003af0a0-6a45-55bf-b765-68e3d349551a" feedUrl="https://agilesetmedia.com/assets/static/feeds/publisher.xml"/>
        </podcast:publisher>
        <item>
            <title><![CDATA[Runnin']]></title>
            <pubDate>Wed, 03 Apr 2024 02:06:28 +0000</pubDate>
            ...
        </item>
    </channel>
</rss>
```

<br>

## More than one relationship

### An artist and a label

Drift is an album. Drift Current is the artist, and Rubber Factory Records is the label. Each has its own publisher
feed, and each feed lists this album back with the same `rel`.

In the album:

```xml
<podcast:guid>a1b2c3d4-e5f6-5718-9abc-def012345678</podcast:guid>
<podcast:medium>music</podcast:medium>
<podcast:publisher>
    <podcast:remoteItem medium="publisher" rel="artist" feedGuid="6f9619ff-8b86-5653-93a9-4e1d0a801b97" feedUrl="https://example.org/drift-current/publisher.xml"/>
    <podcast:remoteItem medium="publisher" rel="label" feedGuid="1b9d6bcd-bbfd-5642-92fe-9089d1c8d331" feedUrl="https://example.org/rubber-factory/publisher.xml"/>
</podcast:publisher>
```

In Drift Current's publisher feed, and in the label's publisher feed, the child link uses the album's medium and the same `rel`:

```xml
<podcast:medium>publisher</podcast:medium>
<podcast:remoteItem medium="music" rel="artist" feedGuid="a1b2c3d4-e5f6-5718-9abc-def012345678" feedUrl="https://example.org/drift/rss.xml"/>
```

```xml
<podcast:medium>publisher</podcast:medium>
<podcast:remoteItem medium="music" rel="label" feedGuid="a1b2c3d4-e5f6-5718-9abc-def012345678" feedUrl="https://example.org/drift/rss.xml"/>
```

### One party, two roles

Jimmy V is the artist and the producer of The Billy Big Rig Album. His publisher feed is named once.
`rel="producer artist"` would agree too.

In the album:

```xml
<podcast:guid>c3d4e5f6-a7b8-5901-2345-6789abcdef01</podcast:guid>
<podcast:medium>music</podcast:medium>
<podcast:publisher>
    <podcast:remoteItem medium="publisher" rel="artist producer" feedGuid="d4e5f6a7-b8c9-5012-3456-789abcdef012" feedUrl="https://example.org/jimmy-v/publisher.xml"/>
</podcast:publisher>
```

In Jimmy V's publisher feed:

```xml
<podcast:medium>publisher</podcast:medium>
<podcast:remoteItem medium="music" rel="artist producer" feedGuid="c3d4e5f6-a7b8-5901-2345-6789abcdef01" feedUrl="https://example.org/billy-big-rig/rss.xml"/>
```
