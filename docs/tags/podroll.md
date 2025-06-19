# Podroll

`<podcast:podroll>`

This element allows for a podcaster to include references to one or more podcasts in its `<channel>` as a way of "recommending" other podcasts to their listener. It's normally shown in user interfaces as "creator recommendations", or "shows you might like".

### Parent

`<channel>`

### Count

Single

### Node value

The node value must be one or more [`<podcast:remoteItem>`](remoteItem.md) elements.

### Examples

```xml
<podcast:podroll>
    <podcast:remoteItem feedGuid="29cdca4a-32d8-56ba-b48b-09a011c5daa9" />
    <podcast:remoteItem feedGuid="396d9ae0-da7e-5557-b894-b606231fa3ea" />
    <podcast:remoteItem feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810" />
</podcast:podroll>
```

Above, the simplest way to use a podroll, using the [one mandatory value for the `remoteItem`](remoteItem.md).

```xml
<podcast:podroll>
    <podcast:remoteItem
        feedGuid="29cdca4a-32d8-56ba-b48b-09a011c5daa9"
        feedUrl="https://feeds.buzzsprout.com/231452.rss"
        title="Buzzcast"
    />
    <podcast:remoteItem
        feedGuid="396d9ae0-da7e-5557-b894-b606231fa3ea"
        feedUrl="https://feeds.buzzsprout.com/1538779.rss"
        title="Podnews Weekly Review"
    />
    <podcast:remoteItem
        feedGuid="917393e3-1b1e-5cef-ace4-edaa54e1f810"
        feedUrl="http://mp3s.nashownotes.com/pc20rss.xml"
        title="Podcasting 2.0"
    />
</podcast:podroll>
```

Above, a recommended podroll entry. It includes the `feedUrl` as well as the `feedGuid`, to help podcast apps discover shows without the GUID if they need to. It also includes a title, which is helpful for humans, and potentially for display in the app.

If information differs, information discoverable from the GUID always takes precedence. RSS feeds can change; the [GUID](guid.md) does not.

While the remoteItem includes an optional `itemGuid`, it is not expected that a podroll would normally link to individual episodes.

(Humans: you can [put a GUID into the search box](https://podcastindex.org/search?q=917393e3-1b1e-5cef-ace4-edaa54e1f810&type=all) on the Podcast Index website).
