# The Publisher Medium
v1.0 - April 5, 2024

<br>

Below, you will find implementation details about using the `publisher` value in the [`<podcast:medium>`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#medium) tag to 
create "publisher feeds".

<br>

## Overview

The idea of a "publisher" is that a single entity (person, organization, record label, etc) might be the responsible 
party which produces multiple podcast feeds.  In such a case it would be useful to be able to see all of a 
publisher's podcasts collected in a single place.  For instance, a news organization might produce 12 different 
podcast feeds.  Or, a music artist might produce 3 albums of music using the `<podcast:medium>` tag of `music`.  In 
those cases, having a high level feed that references these other feeds would make it easier for podcast apps to 
associate those feeds with a particular publishing entity.

Likewise, it is helpful if the produced feeds link back to the "publisher feed" so that podcast apps can walk back 
up the chain from a podcast feed to it's publisher in order to find other relevant content from that publishing 
entity.  For instance, a listener may subscribe to a music album by an artist and want to find their other 
albums and singles.

When a publisher feed links to it's "child" feeds, and those "child" feeds link back to their "parent" publisher 
feeds, this provides a two-way validation that a feed is indeed a valid part of a publishing entities portfolio of 
content.  If a feed links to a publisher feed without the publisher feed referencing it, that association should be 
discarded.

<br>

## Publisher Feed Requirements

A publisher feed must have the following parts in it's `<channel>`:

1. A [`<podcast:medium>`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#medium) tag with a value of `publisher`.
2. A valid [`<podcast:guid>`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#guid).
3. One or more [`<podcast:remoteItem>`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#remoteItem) tags that link to podcast feeds.

### Example

The following example shows a publisher feed that links to all of the feeds published by the "AgileSet Media" entity.
This feed also makes use of the [`<podcast:person>`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#person) tag to define a responsible person at the 
publishing entity.

```xml
<rss xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" version="2.0">
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
makes discovery of your other content much easier.  Podcast apps can see this linkage and "walk back up the chain" 
to your publisher feed and then recommend your other podcast content to a listener.

### Example

The following example snippet shows a podcast feed produced by "AgileSet Media" that links to the publisher feed 
example above.

```xml
<rss xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" xmlns:content="http://purl.org/rss/1.0/modules/content/" xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" version="2.0">
    <channel>
        <title><![CDATA[It's A Mood]]></title>
        <description>A value4value happenstance music show.</description>
        <link>https://itsamood.org</link>
        <generator>Sovereign Feeds</generator>
        <podcast:person href="https://mikeneumann.net" group="cast" role="host" img="https://itsamood.org/assets/static/MikeNeumann_202310.jpg">Mike Neumann</podcast:person>
        <podcast:guid>469b403f-db2d-574c-9db9-96dbb3f6561c</podcast:guid>
        <podcast:medium>podcast</podcast:medium>
        <podcast:remoteItem medium="publisher" feedGuid="003af0a0-6a45-55bf-b765-68e3d349551a" feedUrl="https://agilesetmedia.com/assets/static/feeds/publisher.xml"/>
        <item>
            <title><![CDATA[Runnin']]></title>
            <pubDate>Wed, 03 Apr 2024 02:06:28 +0000</pubDate>
            ...
        </item>
    </channel>
</rss>
```