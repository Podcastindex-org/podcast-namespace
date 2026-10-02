# Remote Item

`<podcast:remoteItem>`

This element provides a way to "point" to another feed or an `<item>` in another feed in order to obtain some sort of data that the other feed or feed item has. This allows a podcast app to know where to go and fetch the data being requested. What data is being requested is determined by the parent item. For instance, if the parent item is a [`<podcast:podroll>`](podroll.md) element, then the remote feed's `<channel>` metadata is needed.

Using the `feedGuid` attribute is the preferred way to address a remote feed since, but there are times when an app may not have access to a list of resolvable [`<podcast:guid>`](guid.md)'s. In that case, it can be beneficial to include the `feedUrl` attribute for those cases as a fallback. If both are present and the app is capable the `feedGuid` should be resolved and used.

### Parent

`<channel>` or [`<podcast:podroll>`](podroll.md) or [`<podcast:valueTimeSplit>`](value-time-split.md) or [`<podcast:publisher>`](publisher.md)

### Count

Multiple

### Attributes

- `feedGuid` **(required)**: The [`<podcast:guid>`](guid.md) of the remote feed being pointed to.
- `feedUrl` (optional): The url of the remote feed being pointed to.
- `itemGuid` (optional): If this remote item element is intended to point to an `<item>` in the remote feed, this attribute should contain the value of the `<guid>` of that `<item>`.
- `medium` (optional): If the feed being pointed to is not of medium type 'podcast', this attribute should contain it's [`<podcast:medium>`](medium.md) type from the [list](./medium.md#medium) of types available in this document. The reason this is helpful is to give the app a heads up on what type of data this is expected to be since that may affect the way it approaches fetch and display.
- `title` (optional): A string that represents the title of the remote item. The purpose of this attribute is to give a hint to apps so that they can display the title without having to do a remote lookup.
- `rel` (optional): When this remote item links a content feed and a publisher feed, a space-separated list of tokens naming how the publisher feed's entity relates to the content feed. Use one remote item per party. When that party has more than one role on the feed, put every role on that one remote item. A second remote item with the same `feedGuid` would look like a second party. Jimmy V is the artist and the producer of The Billy Big Rig Album, so one remote item uses `rel="artist producer"` and the album feed repeats those tokens to agree. If `rel` is omitted, the link has its original meaning: that entity publishes the feed. Use it on a remote item inside [`<podcast:publisher>`](publisher.md), and on the channel-level remote item in that publisher feed which points back at the content feed. If both sides include `rel`, they must name the same set of tokens. Order does not matter. Apps should ignore `rel` on a remote item that is not part of a publisher link. See [`<podcast:publisher>`](publisher.md) for how a link is confirmed. Starting tokens:
  - `artist` — the musical act or band. For example, FM Rodeo, Joe Martin.
  - `host` — the podcast host or personality portfolio. For example, Joe Rogan, Adam Curry.
  - `author` — the writer or audiobook bibliography. For example, Stephen King, Ayn Rand.
  - `label` — the record label catalog. For example, Rubber Factory Records, Canabis Records.
  - `producer` — the producer of a work. For example, Jimmy V.
  - `network` — the podcast network. For example, TWiT, NPR.
  - `hosting` — the infrastructure provider or server platform. For example, Bluebrry, Buzzsprout.
  - `sponsor` — the commercial patron or brand portfolio. For example, Black Rifle Coffee, Anton's Land of Biltong.

### Examples

```xml
<podcast:remoteItem feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810" />
```

```xml
<podcast:remoteItem
    feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810"
    itemGuid="asdf089j0-ep240-20230510"
/>
```

```xml
<podcast:remoteItem
    feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810"
    feedUrl="https://feeds.example.org/917393e3-1b1e-5cef-ace4-edaa54e1f810/rss.xml"
    itemGuid="asdf089j0-ep240-20230510"
    medium="music"
    title="Here Comes the Sun"
/>
```

A publisher link. The same `rel` is set on both sides. In the album feed, inside `<podcast:publisher>`:

```xml
<podcast:remoteItem
    medium="publisher"
    rel="label"
    feedGuid="1b9d6bcd-bbfd-5642-92fe-9089d1c8d331"
    feedUrl="https://example.org/rubber-factory/publisher.xml"
/>
```

In the label's publisher feed, listing that album:

```xml
<podcast:remoteItem
    medium="music"
    rel="label"
    feedGuid="a1b2c3d4-e5f6-5718-9abc-def012345678"
    feedUrl="https://example.org/drift/rss.xml"
/>
```

One party with two roles. Jimmy V is the artist and the producer of the same album, so his publisher feed is named once. Both sides use the same set of tokens. Order does not matter.

In the album feed, inside `<podcast:publisher>`:

```xml
<podcast:remoteItem
    medium="publisher"
    rel="artist producer"
    feedGuid="d4e5f6a7-b8c9-5012-3456-789abcdef012"
    feedUrl="https://example.org/jimmy-v/publisher.xml"
/>
```

In Jimmy V's publisher feed:

```xml
<podcast:remoteItem
    medium="music"
    rel="artist producer"
    feedGuid="c3d4e5f6-a7b8-5901-2345-6789abcdef01"
    feedUrl="https://example.org/billy-big-rig/rss.xml"
/>
```
