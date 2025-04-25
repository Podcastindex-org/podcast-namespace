# Remote Item

`<podcast:remoteItem>`

This element provides a way to "point" to another feed or an `<item>` in another feed in order to obtain some sort of data that the other feed or feed item has. This allows a podcast app to know where to go and fetch the data being requested. What data is being requested is determined by the parent item. For instance, if the parent item is a [`<podcast:podroll>`](podroll.md) element, then the remote feed's `<channel>` metadata is needed.

Using the `feedGuid` attribute is the preferred way to address a remote feed since, but there are times when an app may not have access to a list of resolvable [`<podcast:guid>`](guid.md)'s. In that case, it can be beneficial to include the `feedUrl` attribute for those cases as a fallback. If both are present and the app is capable the `feedGuid` should be resolved and used.

### Parent

`<channel>` or [`<podcast:podroll>`](podroll.md) or [`<podcast:valueTimeSplit>`](valueTimeSplit.md) or [`<podcast:publisher>`](publisher.md)

### Count

Multiple

### Attributes

- **feedGuid** (required) The [`<podcast:guid>`](guid.md) of the remote feed being pointed to.
- **feedUrl** (optional) The url of the remote feed being pointed to.
- **itemGuid** (optional) If this remote item element is intended to point to an `<item>` in the remote feed, this attribute should contain the value of the `<guid>` of that `<item>`.
- **medium** (optional) If the feed being pointed to is not of medium type 'podcast', this attribute should contain it's [`<podcast:medium>`](medium.md) type from the [list](./medium.md#medium) of types available in this document. The reason this is helpful is to give the app a heads up on what type of data this is expected to be since that may affect the way it approaches fetch and display.
- **title** (optional) A string that represents the title of the remote item. The purpose of this attribute is to give a hint to apps so that they can display the title without having to do a remote lookup.

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
